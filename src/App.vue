<template>
<div class="row" style="background-color: pink; font-family: cursive">
  <div class="kepo">
    <marquee direction="up" style="text-align: center">
    <h1>SELAMAT DATANG PARA SISWA</h1>
     <h1> DI PERPUSTAKAAN MILIK BERSAMA</h1>
     <h1>PILIH BUKU YANG AKAN ANDA PINJAM:D</h1>
    </marquee>
      <div class="a">
       <h2>Form Peminjaman Buku&#127803;</h2>
        <form @submit.prevent="add">
        <input type="hidden" v-model="form.id" required>
        <label for="nama siswa">Nama Siswa&#127773; </label><br>
        <input type="text" v-model="form.judulBuku" required ><br><br>
        <label for="judul buku">Judul Buku&#127773;</label><br>
        <input type="text" v-model="form.pengarang" required><br><br>
        <label for="tanggal pinjam">Tanggal Peminjaman&#127773;</label><br>
        <input type="text" v-model="form.tahunTerbit" required><br><br>
        <label for="tanggal pengembalian">Tanggal Pengembalian&#127773;</label><br>
        <input type="text" v-model="form.kategori" required><br><br>
        <button type="submit" v-show="!updateSubmit" style="background-color:skyblue; border-radius: 3px; margin: auto;font-family:cursive">Tambah Peminjaman</button>  
        <button type="button" v-show="updateSubmit" style="background-color:skyblue; border-radius: 3px;font-family:cursive" @click="update(form)">Perpanjang</button> <br>
    </form>
    </div>
    <div class="a">
      <h2>Tabel Daftar Buku&#127809;</h2>
      <p>Nb: Kembalikan buku pada waktu deadline yang telah di tentukan!</p>
      <p>Apabila waktu peminjaman sudah lebih dari waktu yang telah ditentukan maka harus siap-siap terkena denda.</p>
       <table class="container">
                        <thead style="border: 1px solid white">
                            <tr>
                                <th>No.</th>
                                <th>Nama Siswa</th>
                                <th>Judul Buku</th>
                                <th>Tanggal Peminjaman</th>
                                <th>Tanggal Pengembalian</th>
                                <th>Aksi</th>

                            </tr>
                        </thead>
                        <tbody style="border: 1px solid white;" v-for="user in users" :key="user.id">
                            <tr>
                            <td>{{ user.id+1 }}</td>
                            <td>{{ user.judulBuku }}</td>
                            <td>{{ user.pengarang }}</td>
                            <td>{{ user.tahunTerbit }}</td>
                            <td>{{ user.kategori }}</td>
                            <td><button class="btn btn-danger btn-sm" style="background-color: green; border-radius:3px;font-family:cursive" :disabled="isLoading" @click="edit(user)">
         {{ isLoading ? 'Loading...':'Perpanjang' }}
         </button><br><button class="btn btn-danger btn-sm" style="background-color: red; border-radius:3px; font-family:cursive; padding: 6px 26px" :disabled="isLoading"  @click="del(user)">
         {{ isLoading ? 'Loading...':'  Kembali  ' }}
         </button></td>

                            </tr>
                        </tbody>
                    </table>
                    <p>Semoga harimu menyenangkan &#127804;</p>
    </div>
  </div>
</div>
</template>

<script>
/* eslint-disable */ 
import axios from 'axios'
export default {
  data(){
    return{
        form: {
          id: '',
          judulBuku: '',
          pengarang: '',
          tahunTerbit:'',
          kategori:''
        },
        users: '',
        updateSubmit: false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load(){
        axios.get('http://localhost:3000/users').then(res => {
        this.users = res.data
      }).catch ((err) => {
        console.log(err);
        
      })
    },
      add(){
      axios.post('http://localhost:3000/users/', this.form).then(res => {
          this.load()
          this.form.id = '',
         this.form.judulBuku = '',
         this.form.pengarang = '',
         this.form.tahunTerbit = '',
        this.form.kategori = ''
      })
    },
    edit(user){ 
        this.updateSubmit = true
        this.form.id = user.id 
        this.form.judulBuku = user.judulBuku
        this.form.pengarang = user.pengarang
        this.form.tahunTerbit = user.tahunTerbit
        this.form.kategori = user.kategori
    },
    update(form){ 
       return axios.put('http://localhost:3000/users/' + form.id , {judulBuku: this.form.judulBuku, pengarang: this.form.pengarang, tahunTerbit: this.form.tahunTerbit, kategori: this.form.kategori}).then(res => {
        this.load()
        this.form.id = ''
        this.form.judulBuku = ''
        this.form.pengarang = ''
        this.form.tahunTerbit = ''
        this.form.kategori = ''
        this.updateSubmit = false
      }).catch((err) => {
        console.log(err);
        
      })
    },
    del(user){
      axios.delete('http://localhost:3000/users/' + user.id).then(res =>{
          this.load()
          let index = this.users.indexOf(form.name)
          this.users.splice(index,1)
      })
    }
  }
}
</script>
<style scoped>

* {
    box-sizing: border-box;
  }
  
  /* Create four equal columns that floats next to each other */
  .a {
    float: left;
    width: 40%;
    padding: 50px;
  }
  
  /* Clear floats after the columns */
  .row:after {
    content: "";
    display: table;
    clear: both;
  }
  
  /* Responsive layout - makes a two column-layout instead of four columns */
  @media screen and (max-width: 500px) {
    .a  {
      width: 40%;
    }
  }
  
  /* Responsive layout - makes the two columns stack on top of each other instead of next to each other */
  @media screen and (max-width: 500px) {
    .a {
      width: 40%;
    }
  }
.kepo {
background-color: pink;
}
button {
  opacity: 100%;
}
table {
  border-collapse: collapse;
  width: 100%;
}

th, td {
  text-align: left;
  padding: 8px;
}

tr:nth-child(even){background-color: #f2f2f2}

th {
  background-color: #04AA6D;
  color: white;
}
button {
  font-size: 15px;
  padding: 5px 15px;
}
</style>