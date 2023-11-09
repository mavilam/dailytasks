<template>
<div id="list" class="flex flex-col justify-center h-screen sm:w-80 md:w-40">
  <div v-for="(item, index) in items" :key="index">
    <Item :item="item" :showDeleteButton="showDeleteButtons" @remove="removeTask" />
  </div>
  <div class="relative h-10 w-full min-w-[200px]" v-if="addTaskMode">
    <input v-model="taskTitle" ref="taskTitleInput" @keydown.enter="handleKeyup" @keydown.esc="handleKeyup" class="peer h-full w-full max-w-full rounded-[7px] border border-blue-gray-200 border-t-transparent bg-transparent px-3 py-2.5 font-sans text-sm font-normal text-blue-gray-700 outline outline-0 transition-all placeholder-shown:border placeholder-shown:border-blue-gray-200 placeholder-shown:border-t-blue-gray-200 focus:border-2 focus:border-gray-500 focus:border-t-transparent focus:outline-0 disabled:border-0 disabled:bg-blue-gray-50  mr-10" placeholder=" " />
    <button class="!absolute right-1 top-1 z-10 select-none rounded bg-gray-500 py-2 px-4 text-center align-middle font-sans text-xs font-bold uppercase text-white shadow-md shadow-gray-500/20 transition-all hover:shadow-lg hover:shadow-gray-500/40 focus:opacity-[0.85] focus:shadow-none active:opacity-[0.85] active:shadow-none peer-placeholder-shown:pointer-events-none peer-placeholder-shown:bg-blue-gray-500 peer-placeholder-shown:opacity-50 peer-placeholder-shown:shadow-none" type="button" data-ripple-light="true" @click="confirmTask">
      Confirmar
    </button>
    <label class="before:content[' '] after:content[' '] pointer-events-none absolute left-0 -top-1.5 flex h-full w-full select-none text-[11px] font-normal leading-tight text-blue-gray-400 transition-all before:pointer-events-none before:mt-[6.5px] before:mr-1 before:box-border before:block before:h-1.5 before:w-2.5 before:rounded-tl-md before:border-t before:border-l before:border-blue-gray-200 before:transition-all after:pointer-events-none after:mt-[6.5px] after:ml-1 after:box-border after:block after:h-1.5 after:w-2.5 after:flex-grow after:rounded-tr-md after:border-t after:border-r after:border-blue-gray-200 after:transition-all peer-placeholder-shown:text-sm peer-placeholder-shown:leading-[3.75] peer-placeholder-shown:text-blue-gray-500 peer-placeholder-shown:before:border-transparent peer-placeholder-shown:after:border-transparent peer-focus:text-[11px] peer-focus:leading-tight peer-focus:text-gray-500 peer-focus:before:border-t-2 peer-focus:before:border-l-2 peer-focus:before:border-gray-500 peer-focus:after:border-t-2 peer-focus:after:border-r-2 peer-focus:after:border-gray-500 peer-disabled:text-transparent peer-disabled:before:border-transparent peer-disabled:after:border-transparent peer-disabled:peer-placeholder-shown:text-blue-gray-500">
      Task
    </label>
  </div>
  <div class="flex flex-col items-center mt-2">
    <button class="px-2 py-2 bg-gray-400 hover:bg-gray-500 text-white rounded-lg w-36" @click="addTask" v-if="!addTaskMode">
    <div class="flex items-center justify-center">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
        <path d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z" />
      </svg>
    </div>
  </button>
  <button class="px-2 py-2 bg-red-400 hover:bg-red-5s00 text-white rounded-lg w-36" @click="showDeleteButtons = !showDeleteButtons" v-if="canEdit()">
    <div class="flex items-center justify-center">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
        <path d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z" />
      </svg>
    </div>
  </button>
  </div>

</div>
</template>

<script>
import Item from './Item.vue';

export default {
  components: {
    Item
  },
  data() {
    return {
      items: [],
      addTaskMode: false,
      showDeleteButtons: false,
      taskTitle: ''
    }
  },
  created() {
    const savedItems = JSON.parse(localStorage.getItem('tasks'));
    if (savedItems) {
      this.items = savedItems;
      console.log("Restored tasks from localStorage")
    }
  },
  watch: {
    items: {
      handler() {
        localStorage.setItem('tasks', JSON.stringify(this.items));
        console.log("Saved tasks to localStorage")
      },
      deep: true
    }
  },
  methods: {
    addTask() {
      this.showDeleteButtons = false;
      this.addTaskMode = true;
      this.$nextTick(() => {
        this.$refs.taskTitleInput.focus();
      });
    },
    confirmTask() {
      if (this.taskTitle.trim() !== '') {
        this.items.push({
          taskTitle: this.taskTitle,
          isChecked: false 
        });
        this.taskTitle = '';
      }
      this.addTaskMode = false;
    },
    handleKeyup(event) {
      if (event.key === 'Enter') {
        this.confirmTask();
      } else if (event.key === 'Escape') {
        this.addTaskMode = false;
      }
    },
    removeTask(item) {
      this.items = this.items.filter(i => i !== item);
    },
    toggleDeleteButtons() {
      this.showDeleteButtons = !this.showDeleteButtons;
    },
    canEdit() {
      return this.items.length > 0 && !this.addTaskMode;
    }
  }
}
</script>
