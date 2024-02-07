<template>
  <div class="h-screen w-full bg-[#0C090A] flex justify-center items-center">
    <div class="absolute top-0 h-full w-full flex justify-center items-center bg-[rgba(0,0,0,0.5)]" v-if="editTodoId > -1">
      <div class="w-1/4 border-2 rounded-md border-white p-2 bg-[#0C090A]">
      <div class="w-full flex justify-end"><button @click="handleClose()"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 text-white">
  <path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12" />
</svg>
</button></div>
      <input class="w-full bg-inherit text-white rounded-md outline-0 my-2 border-b-2 border-b-white" type="text" v-model="editTodoVal" placeholder="Enter Todo" />
      <button class="w-full py-2 shadow-sm shadow-white text-white" @click="handleUpdateTodoValue()">Update Value</button>
      </div>
    </div>
    <div class="absolute top-0 h-full w-full flex justify-center items-center bg-[rgba(0,0,0,0.5)]" v-if="editDoneId > -1">
      <div class="w-1/4 border-2 rounded-md border-white p-2 bg-[#0C090A]">
      <div class="w-full flex justify-end"><button @click="handleClose()"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 text-white">
  <path stroke-linecap="round" stroke-linejoin="round" d="M6 18 18 6M6 6l12 12" />
</svg>
</button></div>
      <input class="w-full bg-inherit text-white rounded-md outline-0 my-2 border-b-2 border-b-white" type="text" v-model="editDoneVal" placeholder="Enter Todo" />
      <button class="w-full py-2 shadow-sm shadow-white text-white" @click="handleUpdateDoneValue()">Update Value</button>
      </div>
    </div>
    <div class="w-[40%] h-[60%] rounded-md shadow-md shadow-white p-2">
    <div class="w-full h-[12%] flex justify-between">
    <input class="w-[85%] rounded-md bg-inherit text-white" type="text" v-model="todo" placeholder="Enter Todo" />
    <button class="w-[13%] text-white border-white hover:bg-white hover:text-black border-2 rounded-md" @click="handleAddTodo()">Add</button>
    </div>
    <div class="h-[90%] w-full flex py-2 justify-between">
    <div class="h-full w-[49%] border-2 border-white rounded-md flex flex-col items-center">
      <div class="h-[10%] w-full flex justify-center items-center"><p class="text-white">TODO</p></div>
    <div class="h-[90%] w-[96%] overflow-y-auto" v-if="todoList.length > 0">
      <div class="shadow-sm shadow-white p-2 my-2" v-for="e in todoList" :key="e.id">
                <p class="text-white"> {{ e.value }} </p>
        <div class="w-full flex justify-between items-center"><p class="text-white text-sm">{{ new Date(e.date).toLocaleDateString() }}</p>
        <div><button @click="handleEditTodo(e.id)"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 text-white">
  <path stroke-linecap="round" stroke-linejoin="round" d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
</svg>
</button>
        <button @click="handleUpdate(e.id)">Done</button>
        <button @click="handleDeleteTodo(e.id)"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 text-white">
  <path stroke-linecap="round" stroke-linejoin="round" d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
</svg>
</button>
      </div>
      </div>
      </div>
    </div>
    </div>

    <div class="h-full w-[49%] border-2 flex flex-col items-center border-white rounded-md">
      <div class="h-[10%] w-full flex justify-center items-center"><p class="text-white">DONE</p></div>
    <div class="h-[90%] w-[96%] overflow-y-auto"  v-if="doneList.length > 0">
      <div class="shadow-sm shadow-white p-2 my-2" v-for="e in doneList" :key="e.id">
        <p class="text-white line-through"> {{ e.value }} </p>
        <div class="w-full flex justify-between items-center">
        <p class="text-white text-sm"> {{ new Date(e.date).toLocaleDateString() }}</p>
        <div>
        <button @click="handleEditDone(e.id)"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 text-white">
  <path stroke-linecap="round" stroke-linejoin="round" d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10" />
</svg></button>
        <button @click="handleUpdateTodo(e.id)">Todo</button>
        <button @click="handleDeleteDone(e.id)"><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5 text-white">
  <path stroke-linecap="round" stroke-linejoin="round" d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0" />
</svg>
</button>
</div>
</div>
      </div>
    </div>
    </div>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todo: '',
      todoList: [],
      doneList: [],
      editTodoId: -1,
      editDoneId: -1,
      editTodoVal: '',
      editDoneVal: '',
      index: 0,
    };
  },
  methods: {
    handleAddTodo() {
      let obj = { id: this.index, value: this.todo, date: new Date(), status: 'todo' };
      this.index++;
      localStorage.setItem('indexVal', JSON.stringify(this.index));
      let newTodoList = [...this.todoList, obj];
      this.todoList = newTodoList;
      localStorage.setItem('todoList', JSON.stringify(newTodoList));
      this.todo = '';
    },
    handleEditTodo(id) { 
      const updateIndex = this.todoList.findIndex(x => x.id == id); 
      this.editTodoId = updateIndex; 
      this.editTodoVal = this.todoList[updateIndex]?.value; 
    },
    handleEditDone(id) { 
      const updateIndex = this.doneList.findIndex(x => x.id == id);
      this.editDoneId = updateIndex;
      this.editDoneVal = this.doneList[updateIndex]?.value; 
    },
    handleClose() { 
      this.editDoneId = -1;
      this.editTodoId = -1; 
    },
    handleDeleteTodo(id) {
      let updatedTodoList = this.todoList.filter(x => x.id !== id);
      this.todoList = updatedTodoList;
      localStorage.setItem('todoList', JSON.stringify(updatedTodoList));
    },
    handleDeleteDone(id) {
      let updatedTodoList = this.doneList.filter(x => x.id !== id);
      this.doneList = updatedTodoList;
      localStorage.setItem('doneList', JSON.stringify(updatedTodoList));
    },
    handleUpdateTodoValue() {
      let updatedTodoList = [...this.todoList];
      updatedTodoList[this.editTodoId].value = this.editTodoVal;
      this.todoList = updatedTodoList;
      localStorage.setItem('todoList', JSON.stringify(updatedTodoList));
      this.editTodoId = -1;
    },
    handleUpdateDoneValue() {
      let updatedTodoList = [...this.doneList];
      updatedTodoList[this.editDoneId].value = this.editDoneVal;
      this.doneList = updatedTodoList;
      localStorage.setItem('doneList', JSON.stringify(updatedTodoList));
      this.editDoneId = -1;
    },
    handleUpdate(id) { 
      const updateIndex = this.todoList.findIndex(x => x.id == id);
      this.doneList.push(this.todoList[updateIndex]);
      const updatedTodoList = this.todoList.filter(x => x.id != id);
      this.todoList = updatedTodoList;
      localStorage.setItem('todoList', JSON.stringify(updatedTodoList));
      localStorage.setItem('doneList', JSON.stringify(this.doneList));
    }, 
    handleUpdateTodo(id) { 
      const updateIndex = this.doneList.findIndex(x => x.id == id);
      this.todoList.push(this.doneList[updateIndex]);
      const updatedDoneList = this.doneList.filter(x => x.id != id);
      this.doneList = updatedDoneList;
      localStorage.setItem('todoList', JSON.stringify(this.todoList));
      localStorage.setItem('doneList', JSON.stringify(updatedDoneList));
    }
  },
  mounted() {
    let todoItems = localStorage.getItem('todoList');
    let doneItems = localStorage.getItem('doneList');
    let currentIndex = localStorage.getItem('indexVal');
    if (todoItems) this.todoList = JSON.parse(todoItems);
    if (doneItems) this.doneList = JSON.parse(doneItems);
    if (currentIndex) this.index = JSON.parse(currentIndex);
  },
};
</script>

<style>
*::-webkit-scrollbar {
  width: 5px;
}

*::-webkit-scrollbar-track {
  border-radius: 5px;
}

*::-webkit-scrollbar-thumb {
  background-color: rgba(255, 255, 255, 0.4);
  border-radius: 20px;
}
</style>
