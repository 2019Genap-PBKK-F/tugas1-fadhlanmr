<template>
  <section class="content">
    <div class="row">
      <div class="col-lg-12">
        <div>
          <div id="app" ref="spreadsheet"></div>
          <div><input type="button" class="btn btn-primary" value="Tambah Data Mahasiswa" @click="() => spreadsheet.insertRow()" /></div>
          <hr>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import jexcel from 'jexcel'
import 'jexcel/dist/jexcel.css'
var data = [
  ['05111740000123', 'Muhammad Fadhlan', '2017', '06/12/1998', '', true],
  ['05111740009090', 'Jonathan Joestar', '2018', '12/06/2001', '', true],
  ['05111740000909', 'Sutejo', '2019', '01/05/2005', '', true]
]
var options = {
  data: data,
  allowToolbar: true,
  columns: [
    { type: 'text', title: 'NRP', width: '120px' },
    { type: 'text', title: 'Nama', width: '240px' },
    { type: 'dropdown', title: 'Angkatan', width: '80px', source: [ '2012', '2013', '2014', '2015', '2016', '2017', '2018', '2019' ] },
    { type: 'calendar', title: 'Tanggal Lahir', width: '100px', options: { format: 'DD/MM/YYYY' } },
    { type: 'image', title: 'Photo', width: '120px' },
    { type: 'checkbox', title: 'Aktif', width: '80px' }
    // { type: 'numeric', title: 'Price', width: '100px', mask: '$ #.##,00', decimal: ',' }
  ]
}
export default {
  name: 'App',
  mounted: function () {
    let spreadsheet = jexcel(this.$el, options)
    Object.assign(this, { spreadsheet })
  }
}
</script> 
