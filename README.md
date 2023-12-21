# Vue 3 + Vite
## Disclaimer : this application using vue with tailwind CSS

    src/
    |-- components/
    |   |-- todoapp.vue

Script : 

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

- todos: untuk menyimpan daftar tugas yang harus dilakukan (todos). Data ini diambil dari penyimpanan lokal menggunakan localStorage.getItem('todos'). Jika data tidak ada di penyimpanan lokal, array ini akan kosong ([]).
- doneTodos: Untuk menyimpan daftar tugas yang sudah selesai. Seperti todos, data ini diambil dari penyimpanan lokal menggunakan localStorage.getItem('doneTodos'), dan jika tidak ada, array ini akan kosong ([]).
- newTodo: Untuk menangkap input dari pengguna ketika mereka menambahkan tugas baru. Dengan dua properti, name untuk nama tugas baru dan priority untuk tingkat prioritasnya yang defaultnya ke 'low'
  
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

Mendefinisikan properti komputasi. Properti komputasi ini menghitung nilai berdasarkan data yang ada, dan mereka diperbarui secara otomatis ketika data yang mereka gunakan berubah.

- pendingTodos: Ini menghitung jumlah tugas yang belum selesai
- lowPriorityTodos, mediumPriorityTodos, highPriorityTodos: Masing-masingnya menghitung jumlah tugas dengan prioritas rendah, sedang, dan tinggi.

**Methods :**

    methods: {
        addTodo() {
            if (this.newTodo.name) {
                const newTodo = {
                id: this.todos.length + 1,
                text: this.newTodo.name,
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


Methods ini untuk menambahkan tugas baru. Jika nama tugas (newTodo.name) tidak kosong, sebuah objek tugas baru dibuat dan ditambahkan ke array todos. Kemudian, data todos disimpan ke penyimpanan lokal menggunakan localStorage.setItem.

    deleteTodo(id) {
        this.todos = this.todos.filter((todo) => todo.id !== id);

        // Menyimpan data ke penyimpanan lokal setelah menghapus todo
        localStorage.setItem('todos', JSON.stringify(this.todos));
    },

Methods ini untuk menghapus tugas berdasarkan ID, yang menggunakan metode filter untuk membuat array baru yang tidak memasukkan tugas dengan ID yang sesuai. Setelah itu, data todos diperbarui dan disimpan ke penyimpanan lokal.


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


Methods ini digunakan ketika pengguna menyelesaikan tugas. Dengan cara berdasarkan ID, kemudian menggeser tugas dari array todos ke array doneTodos. Setelah itu, kedua array diperbarui dan disimpan ke penyimpanan lokal.
    
## Summary Section
Berisi informasi ringkasan (summary) dari aplikasi "Todo App". Di dalamnya terdapat informasi mengenai jumlah tugas yang masih menunggu (Pending), jumlah tugas dengan prioritas rendah (Low Priority), jumlah tugas dengan prioritas sedang (Medium Priority), dan jumlah tugas dengan prioritas tinggi (High Priority). 

## Form Section
## Todo List Section
