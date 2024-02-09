<template>
  <div class="h-screen w-full bg-[#8758FF] flex justify-center items-center">
    <div
      class="absolute top-0 h-full w-full flex justify-center items-center bg-[rgba(0,0,0,0.5)]"
      v-if="editTodoId > -1"
    >
      <div class="w-1/4 rounded-md p-2 bg-[#1A1A40]">
        <div class="w-full flex justify-between">
          <p class="text-white">Modify Todo</p>
          <button @click="handleClose()">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-6 h-6 text-white"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M6 18 18 6M6 6l12 12"
              />
            </svg>
          </button>
        </div>
        <input
          class="w-full bg-white border-none text-black rounded-md outline-0 my-2"
          type="text"
          v-model="editTodoVal"
          placeholder="Enter Todo"
        />
        <button
          class="w-full py-2 shadow-sm bg-[#8758FF] hover:bg-[#6D35D8] rounded-sm shadow-black text-white"
          @click="handleUpdateTodoValue()"
        >
          UPDATE VALUE
        </button>
      </div>
    </div>
    <div
      class="absolute top-0 h-full w-full flex justify-center items-center bg-[rgba(0,0,0,0.5)]"
      v-if="editDoneId > -1"
    >
      <div class="w-1/4 rounded-md p-2 bg-[#1A1A40]">
        <div class="w-full flex justify-between items-center">
          <p class="text-white">Modify Todo</p>
          <button @click="handleClose()">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-6 h-6 text-white"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M6 18 18 6M6 6l12 12"
              />
            </svg>
          </button>
        </div>
        <input
          class="w-full bg-white border-none text-black rounded-md outline-0 my-2"
          type="text"
          v-model="editDoneVal"
          placeholder="Enter Todo"
        />
        <button
          class="w-full py-2 shadow-sm bg-[#8758FF] hover:bg-[#6D35D8] rounded-sm shadow-black text-white"
          @click="handleUpdateDoneValue()"
        >
          UPDATE VALUE
        </button>
      </div>
    </div>
    <div
      class="w-[80%] sm:w-[70%] lg:w-[40%] h-[90%] sm:h-[80%] lg:h-[70%] rounded-md shadow-md bg-[#1A1A40] shadow-[#1A1A40] p-2"
    >
      <div class="w-full h-[12%] flex justify-between">
        <input
          class="w-[85%] rounded-md bg-white text-lg outline-none border-none"
          type="text"
          v-model="todo"
          placeholder="Enter Todo"
        />
        <button
          class="w-[13%] bg-[#8758FF] font-semibold text-white hover:bg-[#6D35D8] rounded-md"
          @click="handleAddTodo()"
        >
          ADD
        </button>
      </div>
      <div class="h-[90%] w-full flex flex-col sm:flex-row py-2 justify-between">
        <div
          class="h-[49%] sm:h-full w-full sm:w-[49.5%] bg-[#1A1A40] rounded-md flex flex-col items-center"
        >
          <div class="h-[10%] w-full flex justify-center items-center">
            <p class="font-semibold text-white">TODO</p>
          </div>
          <div
            class="h-[90%] flex justify-center items-center w-[96%] overflow-y-auto"
            v-if="todoList.length == 0"
          >
            <p class="font-semibold text-white text-xl">No Task To do</p>
          </div>
          <div
            class="h-[90%] w-[96%] overflow-y-auto"
            v-if="todoList.length > 0"
          >
            <div
              class="shadow-sm shadow-[#8758FF] bg-[#8758FF] rounded-md p-2 my-2"
              v-for="e in todoList"
              :key="e.id"
            >
              <p class="text-white text-lg">{{ e.value }}</p>
              <div class="w-full flex justify-between items-center">
                <p class="text-white text-sm">
                  {{ new Date(e.date).toLocaleDateString() }}
                </p>
                <div>
                  <button @click="handleEditTodo(e.id)">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      fill="currentColor"
                      class="w-5 h-5 text-white mx-0.5"
                    >
                      <path
                        d="M21.731 2.269a2.625 2.625 0 0 0-3.712 0l-1.157 1.157 3.712 3.712 1.157-1.157a2.625 2.625 0 0 0 0-3.712ZM19.513 8.199l-3.712-3.712-12.15 12.15a5.25 5.25 0 0 0-1.32 2.214l-.8 2.685a.75.75 0 0 0 .933.933l2.685-.8a5.25 5.25 0 0 0 2.214-1.32L19.513 8.2Z"
                      />
                    </svg>
                  </button>
                  <button class="text-black" @click="handleUpdate(e.id)">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      fill="currentColor"
                      class="w-5 h-5 text-white mx-0.5"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M7.502 6h7.128A3.375 3.375 0 0 1 18 9.375v9.375a3 3 0 0 0 3-3V6.108c0-1.505-1.125-2.811-2.664-2.94a48.972 48.972 0 0 0-.673-.05A3 3 0 0 0 15 1.5h-1.5a3 3 0 0 0-2.663 1.618c-.225.015-.45.032-.673.05C8.662 3.295 7.554 4.542 7.502 6ZM13.5 3A1.5 1.5 0 0 0 12 4.5h4.5A1.5 1.5 0 0 0 15 3h-1.5Z"
                        clip-rule="evenodd"
                      />
                      <path
                        fill-rule="evenodd"
                        d="M3 9.375C3 8.339 3.84 7.5 4.875 7.5h9.75c1.036 0 1.875.84 1.875 1.875v11.25c0 1.035-.84 1.875-1.875 1.875h-9.75A1.875 1.875 0 0 1 3 20.625V9.375Zm9.586 4.594a.75.75 0 0 0-1.172-.938l-2.476 3.096-.908-.907a.75.75 0 0 0-1.06 1.06l1.5 1.5a.75.75 0 0 0 1.116-.062l3-3.75Z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                  <button @click="handleDeleteTodo(e.id)">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      fill="currentColor"
                      class="w-5 h-5 text-white"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M16.5 4.478v.227a48.816 48.816 0 0 1 3.878.512.75.75 0 1 1-.256 1.478l-.209-.035-1.005 13.07a3 3 0 0 1-2.991 2.77H8.084a3 3 0 0 1-2.991-2.77L4.087 6.66l-.209.035a.75.75 0 0 1-.256-1.478A48.567 48.567 0 0 1 7.5 4.705v-.227c0-1.564 1.213-2.9 2.816-2.951a52.662 52.662 0 0 1 3.369 0c1.603.051 2.815 1.387 2.815 2.951Zm-6.136-1.452a51.196 51.196 0 0 1 3.273 0C14.39 3.05 15 3.684 15 4.478v.113a49.488 49.488 0 0 0-6 0v-.113c0-.794.609-1.428 1.364-1.452Zm-.355 5.945a.75.75 0 1 0-1.5.058l.347 9a.75.75 0 1 0 1.499-.058l-.346-9Zm5.48.058a.75.75 0 1 0-1.498-.058l-.347 9a.75.75 0 0 0 1.5.058l.345-9Z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div
          class="h-[49%] sm:h-full w-full sm:w-[49.5%] flex flex-col items-center bg-[#1A1A40] rounded-md"
        >
          <div class="h-[10%] w-full flex justify-center items-center">
            <p class="font-semibold text-white">DONE</p>
          </div>
          <div
            class="h-[90%] flex justify-center items-center w-[96%] overflow-y-auto"
            v-if="doneList.length == 0"
          >
            <p class="font-semibold text-xl text-white">No Task Completed</p>
          </div>
          <div
            class="h-[90%] w-[96%] overflow-y-auto"
            v-if="doneList.length > 0"
          >
            <div
              class="shadow-sm shadow-[#8758FF] bg-[#8758FF] rounded-md p-2 my-2"
              v-for="e in doneList"
              :key="e.id"
            >
              <p class="text-white line-through">{{ e.value }}</p>
              <div class="w-full flex justify-between items-center">
                <p class="text-white text-sm">
                  {{ new Date(e.date).toLocaleDateString() }}
                </p>
                <div>
                  <button @click="handleEditDone(e.id)">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      fill="currentColor"
                      class="w-5 h-5 text-white mx-0.5"
                    >
                      <path
                        d="M21.731 2.269a2.625 2.625 0 0 0-3.712 0l-1.157 1.157 3.712 3.712 1.157-1.157a2.625 2.625 0 0 0 0-3.712ZM19.513 8.199l-3.712-3.712-12.15 12.15a5.25 5.25 0 0 0-1.32 2.214l-.8 2.685a.75.75 0 0 0 .933.933l2.685-.8a5.25 5.25 0 0 0 2.214-1.32L19.513 8.2Z"
                      />
                    </svg>
                  </button>
                  <button class="text-black" @click="handleUpdateTodo(e.id)">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      fill="currentColor"
                      class="w-5 h-5 text-white mx-0.5 font-semibold"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M9.53 2.47a.75.75 0 0 1 0 1.06L4.81 8.25H15a6.75 6.75 0 0 1 0 13.5h-3a.75.75 0 0 1 0-1.5h3a5.25 5.25 0 1 0 0-10.5H4.81l4.72 4.72a.75.75 0 1 1-1.06 1.06l-6-6a.75.75 0 0 1 0-1.06l6-6a.75.75 0 0 1 1.06 0Z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                  <button @click="handleDeleteDone(e.id)">
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 24 24"
                      fill="currentColor"
                      class="w-5 h-5 text-white"
                    >
                      <path
                        fill-rule="evenodd"
                        d="M16.5 4.478v.227a48.816 48.816 0 0 1 3.878.512.75.75 0 1 1-.256 1.478l-.209-.035-1.005 13.07a3 3 0 0 1-2.991 2.77H8.084a3 3 0 0 1-2.991-2.77L4.087 6.66l-.209.035a.75.75 0 0 1-.256-1.478A48.567 48.567 0 0 1 7.5 4.705v-.227c0-1.564 1.213-2.9 2.816-2.951a52.662 52.662 0 0 1 3.369 0c1.603.051 2.815 1.387 2.815 2.951Zm-6.136-1.452a51.196 51.196 0 0 1 3.273 0C14.39 3.05 15 3.684 15 4.478v.113a49.488 49.488 0 0 0-6 0v-.113c0-.794.609-1.428 1.364-1.452Zm-.355 5.945a.75.75 0 1 0-1.5.058l.347 9a.75.75 0 1 0 1.499-.058l-.346-9Zm5.48.058a.75.75 0 1 0-1.498-.058l-.347 9a.75.75 0 0 0 1.5.058l.345-9Z"
                        clip-rule="evenodd"
                      />
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
import axios from "axios";
export default {
  data() {
    return {
      todo: "",
      todoList: [],
      doneList: [],
      editTodoId: -1,
      editDoneId: -1,
      editTodoVal: "",
      editDoneVal: "",
      index: 0,
    };
  },
  methods: {
    async handleAddTodo() {
      let obj = {
        value: this.todo,
        date: new Date(),
      };
      await axios.post("http://localhost:3000/todoList", obj);
      const todoRes = await axios.get("http://localhost:3000/todoList");
      this.todoList = todoRes.data;
      this.todo = "";
    },
    async getData() {
      try {
        const todoRes = await axios.get("http://localhost:3000/todoList");
        const doneRes = await axios.get("http://localhost:3000/doneList");
        this.todoList = todoRes.data;
        this.doneList = doneRes.data;
      } catch (e) {
        console.error(e);
      }
    },
    handleEditTodo(id) {
      const updateIndex = this.todoList.findIndex((x) => x.id == id);
      this.editTodoId = updateIndex;
      this.editTodoVal = this.todoList[updateIndex]?.value;
    },
    handleEditDone(id) {
      const updateIndex = this.doneList.findIndex((x) => x.id == id);
      this.editDoneId = updateIndex;
      this.editDoneVal = this.doneList[updateIndex]?.value;
    },
    handleClose() {
      this.editDoneId = -1;
      this.editTodoId = -1;
    },
    async handleDeleteTodo(id) {
      let updatedTodoList = this.todoList.filter((x) => x.id !== id);
      this.todoList = updatedTodoList;
      await axios.delete(`http://localhost:3000/todoList/${id}`);
    },
    async handleDeleteDone(id) {
      let updatedTodoList = this.doneList.filter((x) => x.id !== id);
      this.doneList = updatedTodoList;      
      await axios.delete(`http://localhost:3000/doneList/${id}`);
    },
    async handleUpdateTodoValue() {
      let obj = this.todoList[this.editTodoId];
      obj.value = this.editTodoVal;
      await axios.put(`http://localhost:3000/todoList/${obj.id}`, obj);
      this.editTodoId = -1;
    },
    async handleUpdateDoneValue() {
      let obj = this.doneList[this.editDoneId];
      obj.value = this.editDoneVal;
      await axios.put(`http://localhost:3000/doneList/${obj.id}`, obj);
      await this.getData();
      this.editDoneId = -1;
    },
    async handleUpdate(id) {
      const updateIndex = this.todoList.findIndex((x) => x.id == id);
      await axios.post("http://localhost:3000/doneList", this.todoList[updateIndex]);
      await axios.delete(`http://localhost:3000/todoList/${id}`);
      await this.getData();
    },
    async handleUpdateTodo(id) {
      const updateIndex = this.doneList.findIndex((x) => x.id == id);
      await axios.post("http://localhost:3000/todoList", this.doneList[updateIndex]);
      await axios.delete(`http://localhost:3000/doneList/${id}`);
      await this.getData();
    },
  },
  mounted() {
    this.getData();
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
