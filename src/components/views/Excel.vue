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
var data = [
  ['05111740000128', 'Yong', '1999-07-01', 'Laki-laki', 'Rp 7500000', true, '']
]
var options = {
  data: data,
  allowToolbar: true,
  columns: [
    { type: 'text', title: 'NRP', width: '150px' },
    { type: 'text', title: 'Nama', width: '200px' },
    { type: 'calendar', title: 'Tgl Lahir', width: '100px' },
    { type: 'dropdown', title: 'Kelamin', width: '100px', source: [ 'Laki-laki', 'Perempuan' ] },
    { type: 'numeric', title: 'UKT', width: '100px', mask: 'Rp ###' },
    { type: 'checkbox', title: 'Aktif', width: '50px' },
    { type: 'image', title: 'Photo', width: '300px' }
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
