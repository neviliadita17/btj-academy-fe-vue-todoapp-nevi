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
Bagian ini adalah formulir untuk menambahkan tugas baru (todo) ke daftar. Berikut adalah penjelasan singkat dari setiap elemen dalam formulir:

`<form v-on:submit="addTodo">`: Ini adalah formulir yang menggunakan directive `v-on` untuk menangani event `submit`. Ketika formulir ini disubmit, method `addTodo` akan dipanggil untuk menambahkan tugas baru

`<input v-model="newTodo.name" type="text" id="name" name="name" required class="mt-1 p-2 w-full border rounded-md" />`: Ini adalah input teks untuk memasukkan nama tugas. Properti `newTodo.name` menggunakan `v-model`, sehingga nilainya akan diupdate saat pengguna memasukkan teks

`<select v-model="newTodo.priority" id="priority" name="priority" class="mt-1 p-2 w-full border rounded-md">`: Ini adalah dropdown select untuk memilih tingkat prioritas tugas. Properti `newTodo.priority` ini menggunakan `v-model`, sehingga nilai prioritas dapat dipilih

Tombol "Save":
   ```html
   <button type="submit" v-bind:disabled="!newTodo.name" class="bg-green-500 text-white px-4 py-2 rounded-md hover:bg-green-600 focus:outline-none -right">Save</button>
   ```
   - Tombol ini hanya dapat diklik jika `newTodo.name` memiliki nilai (tidak kosong), karena `v-bind:disabled` diatur berdasarkan kondisi ini.
   - Ketika tombol ini diklik, formulir akan disubmit, dan method `addTodo` akan dipanggil

Dengan form ini kita dapat memasukkan nama tugas dan memilih prioritasnya, kemudian menekan tombol "Save" untuk menambahkan tugas baru ke dalam daftar

## Todo List Section
Code yang Anda berikan adalah bagian dari tampilan daftar tugas (todo list) menggunakan Vue.js dan Tailwind CSS. Berikut adalah penjelasan singkatnya:

### Bagian Todo:
   - Daftar tugas yang belum selesai (`todos`) ditampilkan di bagian kiri dari layout dan setiap tugas ditampilkan dalam elemen div dengan kelas-kelas dari Tailwind CSS.
   - `v-if`: jika tidak ada tugas yang belum selesai, pesan "No todos yet." ditampilkan
   - `v-fo`: untuk melakukan iterasi melalui array todos dan doneTodos dan membuat elemen untuk setiap tugas.
     `v-on:click`: untuk menangani event klik pada tombol "Delete" dan "Done" dan memanggil method deleteTodo dan markDone untuk melakukan tindakan yang sesuai
   - Prioritas tugas ditampilkan dengan warna latar belakang yang berbeda berdasarkan prioritas (low: biru, medium: kuning, high: merah) menggunakan
     
            v-bind:class="{
             'bg-blue-500': doneTodo.priority === 'low',
             'bg-yellow-500': doneTodo.priority === 'medium',
             'bg-red-500': doneTodo.priority === 'high',
             }"

### Bagian Done:
   - Daftar tugas yang sudah selesai (`doneTodos`) ditampilkan di bagian kanan dari layout dan setiap tugas yang sudah selesai ditampilkan dalam elemen div dengan kelas-kelas dari Tailwind CSS.
   - `v-if`: Jika tidak ada tugas yang sudah selesai, pesan "No completed todos yet." ditampilkan.
   - `v-fo`r: untuk melakukan iterasi melalui array todos dan doneTodos dan membuat elemen untuk setiap tugas.
   - Untuk label kategori tugas juga sama dengan todo, menggunakan `v-bind:class`
