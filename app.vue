<template>
  <div class="p-4">
    <h1 class="text-2xl font-bold mb-4">Todo List</h1>

    <!-- Form to add new todo -->
    <form @submit.prevent="addTodo" class="flex mb-4">
      <input type="text" v-model="newTodo" placeholder="Enter a new todo" class="px-4 py-2 mr-2 border border-gray-300 rounded-md" />
      <select v-model="newTodoGroup" class="px-4 me-2 py-2 border border-gray-300 rounded-md">
        <option value="">No Group</option>
        <option >Work</option>
        <option >Chores</option>
        <option >Personal</option>
        <option >Home</option>
      </select>
      <button type="submit" class="px-4 py-2 bg-blue-500 text-white rounded-md">Add Todo</button>
    </form>

    <!-- List of todos -->
    <ul>
      <li v-for="todo in todos" :key="todo.id" class="flex items-center mb-2">
        <span class="mr-2">{{ todo.text }}</span>
        <span v-if="todo.group" :class="getGroupStyle(todo.group)" class="me-2 text-sm text-white rounded-full px-2 py-1">{{ todo.group }}</span>
        <button @click="deleteTodo(todo.id)" class="me-2 px-2 py-1 bg-red-500 text-white rounded-md">Delete</button>
        <button @click="editTodo(todo)" class="me-2 px-2 py-1 bg-blue-500 text-white rounded-md">Edit</button>
      </li>
    </ul>

    <!-- Edit todo Form will bw shown when the edit button is clicked -->
    <div v-if="showEditModal" class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-50">
      <div class="bg-white p-4">
        <h2 class="text-lg font-bold mb-2">Edit Todo</h2>
        <input type="text" v-model="editTodoText" class="px-4 py-2 mb-2 border border-gray-300 rounded-md" />
        <select v-model="editTodoGroup" class="px-4 py-2 border border-gray-300 rounded-md">
          <option value="">No Group</option>
          <option >Work</option>
          <option >Chores</option>
          <option >Personal</option>
          <option >Home</option>
        </select>
        <div class="flex justify-end">  
          <button @click="updateTodo" class="px-4 py-2 bg-blue-500 text-white rounded-md mr-2">Update</button>
          <button @click="cancelEdit" class="px-4 py-2 bg-gray-300 text-gray-800 rounded-md">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      newTodo: '',
      newTodoGroup: '',
      showEditModal: false,
      editTodoId: null,
      editTodoText: '',
      editTodoGroup: '',
    };
  },
  methods: {
    addTodo() {
      const newTodo = {
        id: Math.random(),
        text: this.newTodo,
        group: this.newTodoGroup,
      };
      this.todos.push(newTodo);
      // reset the values inside the form
      this.newTodo = '';
      this.newTodoGroup = '';
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    editTodo(todo) {
      this.editTodoId = todo.id;
      this.editTodoText = todo.text;
      this.editTodoGroup = todo.group ;
      this.showEditModal = true;
    },
    updateTodo() {
      const todo = this.todos.find((todo) => todo.id === this.editTodoId);
      if (todo) {
        todo.text = this.editTodoText;
        todo.group = this.editTodoGroup;
      }
      this.cancelEdit();
    },
    cancelEdit() {
      this.showEditModal = false;
      this.editTodoId = null;
      this.editTodoText = '';
      this.editTodoGroup = '';
    },
    getGroupStyle(group) {
      const styles = {
        Home: 'bg-blue-500',
        Chores: 'bg-green-500',
        Work: 'bg-yellow-500',
        Personal: 'bg-purple-500',
      };
      return styles[group];
    },
  },
};
</script>

