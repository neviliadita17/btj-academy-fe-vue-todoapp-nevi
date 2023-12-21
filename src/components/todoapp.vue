<template>
  <div class="flex flex-col items-center justify-center p-8 bg-gray-100 min-h-screen">
    <div class="max-w-4xl mx-auto p-5 bg-white shadow-md rounded-md">
      <!-- Bagian Summary -->
      <div class="p-4 bg-white rounded-md shadow-md mb-8">
        <h1 class="text-3xl font-semibold mb-4">Todo App Summary</h1>

          <div class="grid grid-cols-4 gap-4">
            <div class="col-span-1 border rounded-md p-3">
              <h2 class="text-xl font-semibold mb-2">Pending</h2>
              <p class="text-2xl">{{ pendingTodos }}</p>
            </div>

            <div class="col-span-1 border rounded-md p-3">
              <h2 class="text-xl font-semibold mb-2">Low Priority</h2>
              <p class="text-2xl">{{ lowPriorityTodos }}</p>
            </div>

            <div class="col-span-1 border rounded-md p-3">
              <h2 class="text-xl font-semibold mb-2">Medium Priority</h2>
              <p class="text-2xl">{{ mediumPriorityTodos }}</p>
            </div>

            <div class="col-span-1 border rounded-md p-3">
              <h2 class="text-xl font-semibold mb-2">High Priority</h2>
              <p class="text-2xl">{{ highPriorityTodos }}</p>
            </div>
          </div>
      </div>

      <!-- Bagian Form -->
      <div class="p-4 bg-white rounded-md shadow-md mb-8">
        <h2 class="text-2xl font-semibold mb-4">Add Todo</h2>
        <form v-on:submit="addTodo">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label for="name" class="block text-sm font-medium text-gray-700">Name:</label>
              <input
                v-model="newTodo.name"
                type="text"
                id="name"
                name="name"
                required
                class="mt-1 p-2 w-full border rounded-md"
              />
            </div>
            <div>
              <label for="priority" class="block text-sm font-medium text-gray-700">Priority:</label>
              <select
                v-model="newTodo.priority"
                id="priority"
                name="priority"
                class="mt-1 p-2 w-full border rounded-md"
              >
                <option value="low">Low</option>
                <option value="medium">Medium</option>
                <option value="high">High</option>
              </select>
            </div>
          </div>
          <div class="flex justify-end mt-4">
            <button
              type="submit"
              v-bind:disabled="!newTodo.name"
              class="bg-green-500 text-white px-4 py-2 rounded-md hover:bg-green-600 focus:outline-none -right"
            >
              Save
            </button>
          </div>
        </form>
      </div>

      <!-- Bagian ToDo List -->
      <div class="p-4 bg-white rounded-md shadow-md">
        <h2 class="text-2xl font-semibold mb-4">Todo List</h2>

        <!-- Bagian Todo -->
        <div class="flex space-x-4">
          <div class="w-1/2">
            <h3 class="text-lg font-semibold mb-2">Todo</h3>
            <div v-if="todos.length === 0" class="text-gray-500">No todos yet.</div>
            <div v-for="todo in todos" :key="todo.id" class="bg-white p-4 mb-4 rounded-md shadow-md">
              <div class="flex justify-between items-center mb-2">
                <div>
                  <p class="text-xl font-semibold">{{ todo.name }}</p>
                  <div class="mt-1 flex items-center">
                    <span class="text-sm font-medium text-gray-500">Priority:</span>
                    <span
                      :class="{
                        'bg-blue-500': todo.priority === 'low',
                        'bg-yellow-500': todo.priority === 'medium',
                        'bg-red-500': todo.priority === 'high',
                      }"
                      class="ml-2 w-4 h-4 rounded-full inline-block"
                    ></span>
                    <span class="ml-2">{{ todo.priority.charAt(0).toUpperCase() + todo.priority.slice(1) }}</span>
                  </div>
                </div>
                <div class="flex flex-col space-y-2 sm:flex-row sm:space-x-2 sm:space-y-0">
                  <button
                    v-on:click="deleteTodo(todo.id)"
                    class="px-4 py-2 rounded-md bg-red-500 text-white sm:px-2 sm:py-1 md:px-3 md:py-2 lg:px-4 lg:py-2"
                  >
                    Delete
                  </button>
                  <button
                    v-on:click="markDone(todo.id)"
                    class="px-4 py-2 rounded-md bg-green-500 text-white sm:px-2 sm:py-1 md:px-3 md:py-2 lg:px-4 lg:py-2"
                  >
                    Done
                  </button>
                </div>
              </div>
            </div>
          </div>

          <!-- Bagian Done -->
          <div class="w-1/2">
            <h3 class="text-lg font-semibold mb-2">Done</h3>
            <div v-if="doneTodos.length === 0" class="text-gray-500">No completed todos yet.</div>
            <div v-for="doneTodo in doneTodos" :key="doneTodo.id" class="bg-white p-4 mb-4 rounded-md shadow-md">
              <div>
                <p class="text-xl font-semibold">{{ doneTodo.name }}</p>
                <div class="mt-1 flex items-center">
                  <span class="text-sm font-medium text-gray-500">Priority:</span>
                  <span
                    :class="{
                      'bg-blue-500': doneTodo.priority === 'low',
                      'bg-yellow-500': doneTodo.priority === 'medium',
                      'bg-red-500': doneTodo.priority === 'high',
                    }"
                    class="ml-2 w-4 h-4 rounded-full inline-block"
                  ></span>
                  <span class="ml-2">{{ doneTodo.priority.charAt(0).toUpperCase() + doneTodo.priority.slice(1) }}</span>
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
      // Mengambil data dari penyimpanan lokal saat komponen dimuat
      todos: JSON.parse(localStorage.getItem('todos')) || [],
      doneTodos: JSON.parse(localStorage.getItem('doneTodos')) || [],
      newTodo: {
        name: '',
        priority: 'low',
      },
    };
  },
  computed: {
    pendingTodos() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    lowPriorityTodos() {
      return this.todos.filter((todo) => todo.priority === 'low').length;
    },
    mediumPriorityTodos() {
      return this.todos.filter((todo) => todo.priority === 'medium').length;
    },
    highPriorityTodos() {
      return this.todos.filter((todo) => todo.priority === 'high').length;
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.name) {
        const newTodo = {
          id: this.todos.length + 1,
          name: this.newTodo.name,
          priority: this.newTodo.priority,
          completed: false,
        };

        this.todos.push(newTodo);
        
        // Menyimpan data ke penyimpanan lokal setelah menambahkan todo
        localStorage.setItem('todos', JSON.stringify(this.todos));

        this.newTodo.name = '';
        this.newTodo.priority = 'low';
      }
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);

      // Menyimpan data ke penyimpanan lokal setelah menghapus todo
      localStorage.setItem('todos', JSON.stringify(this.todos));
    },
    markDone(id) {
      const todoIndex = this.todos.findIndex((todo) => todo.id === id);
      if (todoIndex !== -1) {
        const doneTodo = this.todos.splice(todoIndex, 1)[0];
        this.doneTodos.push(doneTodo);

        // Menyimpan data ke penyimpanan lokal setelah menyelesaikan todo
        localStorage.setItem('todos', JSON.stringify(this.todos));
        localStorage.setItem('doneTodos', JSON.stringify(this.doneTodos));
      }
    },
    // clearDoneTodos() {
    //   // Remove done todos from both component state and localStorage
    //   this.doneTodos = [];
    //   localStorage.removeItem('doneTodos');
    // },
  },
};
</script>

<!-- <script>
export default {
  data() {
    return {
      todos: [],
      doneTodos: [],
      newTodo: {
        name: '',
        priority: 'low',
      },
    };
  },
  computed: {
    pendingTodos() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    lowPriorityTodos() {
      return this.todos.filter((todo) => todo.priority === 'low').length;
    },
    mediumPriorityTodos() {
      return this.todos.filter((todo) => todo.priority === 'medium').length;
    },
    highPriorityTodos() {
      return this.todos.filter((todo) => todo.priority === 'high').length;
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.name) {
        this.todos.push({
          id: this.todos.length + 1,
          text: this.newTodo.name,
          priority: this.newTodo.priority,
          completed: false,
        });
        this.newTodo.name = '';
        this.newTodo.priority = 'low';
      }
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    markDone(id) {
      const todoIndex = this.todos.findIndex((todo) => todo.id === id);
      if (todoIndex !== -1) {
        const doneTodo = this.todos.splice(todoIndex, 1)[0];
        this.doneTodos.push(doneTodo);
      }
    },
  },
};
</script> -->

<style scoped>
</style>
