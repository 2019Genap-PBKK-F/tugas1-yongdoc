<template>
  <div>
    <div id="app" ref="spreadsheet">
      <input type="button" value="Add id" @click="() => spreadsheet.insertRow()"/>
      <input type="button" value="Delete id" @click="() => spreadsheet.deleteRow()"/>
    </div>
  </div>
</template>

<script>
import jexcel from 'jexcel'
import 'jexcel/dist/jexcel.css'
var options = {
  url: 'http://localhost:3000/mahasiswa',
  allowToolbar: true,
  columns: [
    { type: 'text', title: 'NRP', width: '200px' },
    { type: 'text', title: 'Nama', width: '200px' },
    { type: 'calendar', title: 'Tgl Lahir', width: '100px' },
    { type: 'dropdown', title: 'Kelamin', width: '100px', source: [ 'Laki-laki', 'Perempuan' ] },
    { type: 'numeric', title: 'UKT', width: '100px', mask: 'Rp ###' },
    { type: 'checkbox', title: 'Aktif', width: '50px' },
    { type: 'image', title: 'Photo', width: '300px' }
  ],
  style: {

  }
}
export default {
  name: 'App',
  mounted: function () {
    let spreadsheet = jexcel(this.$el, options)
    Object.assign(this, { spreadsheet })
  }
}
</script>
