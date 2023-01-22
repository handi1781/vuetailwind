<script>
import axios from 'axios';

export default {
  data() {
    return {
      nama: '',
      kasus: '',
      jenis_kelamin: '',
      users: [],
      selectedUser: null,
      createdUser: null,
    
    }
  },
  created() {
    this.tampilkanData();
  },
  methods: {

    async tampilkanData() {
      try {
        const response = await axios.get('http://localhost:5000/users/');
        this.users = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    async buatData(event) {
      try {
        // prevent form submission
        event.preventDefault();

        const response = await axios.post('http://localhost:5000/users/', {
          nama: this.nama,
          kasus: this.kasus,
          jenis_kelamin: this.jenis_kelamin
        });
        this.createUser= response.data;
        this.nama = '';
        this.kasus = '';
        this.jenis_kelamin = '';
        window.location.reload()
      } catch (error) {
        console.error(error);
      }
    },
    async updateData(id) {
      try {
        const response = await axios.patch(`http://localhost:5000/users/${id}`, {
          nama: this.nama,
          kasus: this.kasus,
          jenis_kelamin: this.jenis_kelamin
        });
        this.selectedUser = response.data;
      } catch (error) {
        console.error(error);
      }
      // Membersihkan data yang tersimpan di properti data selectedUser
      this.selectedUser = null;
    },
    async hapusData(id) {
      try {
        await axios.delete(`http://localhost:5000/users/${id}`);
       
        this.tampilkanData();
      } catch (error) {
        console.error(error);
      }
        
    },
    selectUser(user) {
      this.selectedUser = user;
    },

    createUser(users) {
      this.createdUser = users;
    }
  }
}
</script>
<style>
#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 0rem;
  font-weight: normal;
}


</style>

<template>
    
<nav class="bg-white border-gray-200 px-2 sm:px-4 py-2.5 rounded dark:bg-gray-900">
  <div class="container flex flex-wrap items-center justify-between mx-auto">
  <a  class="flex items-center">
      <img src="./assets/criminal-in-jail-silhouette.png" class="h-6 mr-3 sm:h-9" alt="Flowbite Logo" />
      <p class="text-4xl font-black text-gray-900 dark:text-white">Data Tahanan Telkom</p>
  </a>
  <div class="flex md:order-2">
    <button type="button" class="text-gray-900 bg-white hover:bg-gray-100 border border-gray-200 focus:ring-4 focus:outline-none focus:ring-gray-100 font-medium rounded-lg text-sm px-5 py-2.5 text-center inline-flex items-center dark:focus:ring-gray-600 dark:bg-gray-800 dark:border-gray-700 dark:text-white dark:hover:bg-gray-700 mr-2 mb-2"
    @click="createUser(users)"><svg fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" aria-hidden="true">
  <path stroke-linecap="round" stroke-linejoin="round" d="M12 10.5v6m3-3H9m4.06-7.19l-2.12-2.12a1.5 1.5 0 00-1.061-.44H4.5A2.25 2.25 0 002.25 6v12a2.25 2.25 0 002.25 2.25h15A2.25 2.25 0 0021.75 18V9a2.25 2.25 0 00-2.25-2.25h-5.379a1.5 1.5 0 01-1.06-.44z"></path>
</svg>
Tambah Data
    </button>
   
  </div>
  </div>
  
<div class="relative overflow-x-auto shadow-md sm:rounded-lg">
    <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr>
                <th scope="col" class="px-6 py-3">
                    Nama
                </th>
                <th scope="col" class="px-6 py-3">
                    Kasus
                </th>
                <th scope="col" class="px-6 py-3">
                    Jenis Kelamin
                </th>
                <th scope="col" class="px-6 py-3">
                    Foto
                </th>
                <th scope="col" class="px-6 py-3">
                    Metode
                </th>
              
            </tr>
        </thead>
        <tbody>
            <tr v-for="user in users" :key="user.id"
            class="bg-white border-b dark:bg-gray-900 dark:border-gray-700">
                <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                    {{ user.nama }}
                </th>
                <td class="px-6 py-4">
                    {{ user.kasus }}
                </td>
                <td class="px-6 py-4">
                    {{ user.jenis_kelamin }}
                </td>
                <td class="px-6 py-4">
                  <div class="relative w-10 h-10 overflow-hidden bg-gray-100 rounded-full dark:bg-gray-600">
    <svg class="absolute w-12 h-12 text-gray-400 -left-1" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z" clip-rule="evenodd"></path></svg>
</div>
                
                </td>
                <td class="px-6 py-4">
                    <button type="button" class="text-white bg-gray-800 hover:bg-gray-900 focus:outline-none focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-gray-800 dark:hover:bg-gray-700 dark:focus:ring-gray-700 dark:border-gray-700"
                    @click="selectUser(user)">
            
Edit</button>
                    <button type="button" class="text-white bg-gray-800 hover:bg-gray-900 focus:outline-none focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-gray-800 dark:hover:bg-gray-700 dark:focus:ring-gray-700 dark:border-gray-700"
                    @click="hapusData(user.id)">
                    
      
                    Hapus</button>
                </td>
            </tr>
        </tbody>
    </table>
</div>

<div class="relative overflow-x-auto shadow-md sm:rounded-lg">
    <table v-if="createdUser"
    class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr>
                <th scope="col" class="px-6 py-3">
                    Nama
                </th>
                <th scope="col" class="px-6 py-3">
                    Kasus
                </th>
                <th scope="col" class="px-6 py-3">
                    Jenis Kelamin
                </th>
                
               
            </tr>
        </thead>
        <tbody>
            <tr class="bg-white border-b dark:bg-gray-900 dark:border-gray-700">
                <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                <input
                  class=""
                  type="text" id="nama" v-model= "nama" />
                </th>
                <td class="px-6 py-4">
                <input
                  class=""
                  type="text" id="nama" v-model= "kasus" />
                </td>
                <td class="px-6 py-4">
                <input
                  class=""
                  type="text" id="nama" v-model= "jenis_kelamin" />
                </td>
                <td class="px-6 py-4">
                <button type="button" class="text-white bg-gray-800 hover:bg-gray-900 focus:outline-none focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-gray-800 dark:hover:bg-gray-700 dark:focus:ring-gray-700 dark:border-gray-700"
                @click.prevent="buatData">
                Tambah Data</button>
                </td>
            </tr>
        </tbody>
    </table>
</div>


<div class="relative overflow-x-auto">
    <table v-if="selectedUser"
     class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr>
                <th scope="col" class="px-6 py-3">
                    Nama
                </th>
                <th scope="col" class="px-6 py-3">
                    Kasus
                </th>
                <th scope="col" class="px-6 py-3">
                    Jenis Kelamin
                </th>
            </tr>
        </thead>
        <tbody>
            <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
                <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                <input
                  class=""
                  type="text" id="nama" v-model="selectedUser.nama" />
                </th>
                <td class="px-6 py-4">
                <input
                  class=""
                  type="text" id="nama" v-model="selectedUser.kasus" />
                </td>
                <td class="px-6 py-4">
                <input
                  class=""
                  type="text" id="nama" v-model="selectedUser.jenis_kelamin" />
                </td>
                <td class="px-6 py-4">
                <button type="button" class="text-white bg-gray-800 hover:bg-gray-900 focus:outline-none focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-gray-800 dark:hover:bg-gray-700 dark:focus:ring-gray-700 dark:border-gray-700"
                @click="updateData(selectedUser.id)">
                Update Data
                </button>
                </td>
            </tr>
        </tbody>
    </table>
</div>

  
</nav>
</template>
