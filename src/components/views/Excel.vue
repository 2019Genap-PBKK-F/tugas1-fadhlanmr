<template>
  <section class="content">
    <div class="container">
      <div class="row">
        <div>
          <div id="app" ref="spreadsheet"></div>
          <div class="col-md-11">
            <input type="button" class="btn btn-primary" value="Tambah Data Mahasiswa" @click="() => spreadsheet.insertRow()" />
            <input type="button" class="btn btn-danger pull-right" value="Delete Data" @click="() => spreadsheet.deleteRow()" />
          </div>
          <hr>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from 'axios'
import jexcel from 'jexcel'
import 'jexcel/dist/jexcel.css'

export default {
  // name: 'App',
  data() {
    return {
      mahasiswa: [],
      form: {
        id: '',
        nrp: '',
        nama: '',
        angkatan: '',
        jk: '',
        lahir: '',
        foto: '',
        aktif: ''
      }
    }
  },
  mounted() {
    let spreadsheet = jexcel(this.$el, this.jexcelOptions)
    Object.assign(this, { spreadsheet })
  },
  methods: {
    newRow() {
      axios.post('http://localhost:8022/api/mahasiswa/', this.form).then(res => {
        console.log(res.data)
      })
    },
    updateRow(instance, cell, columns, row, value) {
      axios.get('http://localhost:8022/api/mahasiswa/').then(res => {
        var index = Object.values(res.data[row])
        index[columns] = value
        console.log(index)
        axios.put('http://localhost:8022/api/mahasiswa/' + index[0], {
          id: index[0],
          nrp: index[1],
          nama: index[2],
          angkatan: index[3],
          jk: index[4],
          lahir: index[5],
          foto: index[6],
          aktif: index[7]
        }).then(res => {
          console.log(res.data)
        })
      })
    },
    deleteRow(instance, row) {
      axios.get('http://localhost:8022/api/mahasiswa/').then(res => {
        var index = Object.values(res.data[row])
        // console.log(index)
        console.log(row)
        axios.delete('http://localhost:8022/api/mahasiswa/' + index[0])
      })
    }
  },
  computed: {
    jexcelOptions() {
      return {
        data: this.mahasiswa,
        allowToolbar: true,
        url: 'http://localhost:8022/api/mahasiswa/',
        onchange: this.updateRow,
        oninsertrow: this.newRow,
        ondeleterow: this.deleteRow,
        columns: [
          { type: 'hidden', title: 'id', width: '10px' },
          { type: 'text', title: 'NRP', width: '120px' },
          { type: 'text', title: 'Nama', width: '200px' },
          { type: 'text', title: 'Angkatan', width: '80px' },
          { type: 'dropdown', title: 'Jenis Kelamin', width: '120px', source: [ 'Laki-laki', 'Perempuan' ], autocomplete: true },
          { type: 'calendar', title: 'Tanggal Lahir', width: '120px' },
          { type: 'image', title: 'Photo', width: '120px' },
          { type: 'checkbox', title: 'Aktif', width: '80px' }
        ]
      }
    }
  }
}
</script> 
