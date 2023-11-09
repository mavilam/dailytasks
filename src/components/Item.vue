<template>
  <div class="flex justify-between items-center p-2 border-b">
    <div class="flex items-center max-w-[75%]">
      <input type="checkbox" v-model="item.isChecked"/>
      <span 
        :class="{ 'line-through': item.isChecked }" 
        :style="{ color: item.isChecked ? 'slate' : 'black', 'text-decoration-thickness': item.isChecked ? '0.15em' : 'initial' }" 
        class="m-2 break-words"
      >
        {{ item.taskTitle }}
      </span>
    </div>
    <button v-if="showDeleteButton" class="inline-flex items-center px-1 py-2 bg-red-600 hover:bg-red-700 text-white text-sm font-medium rounded-md" @click="removeTask">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
      </svg>
    </button>
  </div>
</template>

<script>
export default {
  props: {
    item: Object,
    showDeleteButton: Boolean
  },
  data() {
    return { }
  },
  methods: {
    removeTask() {
      this.$emit('remove', this.item);
    }
  }
}
</script>
