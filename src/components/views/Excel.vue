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
import axios from 'axios'

var hostname = 'localhost:8023'
// var hostname = '10.199.14.46:8023'
var update = function(instance, cell, x, y, val) {
  axios
    .get('http://' + hostname + '/api/mahasiswa/')
    .then((response) => {
      var index = Object.values(response.data[y])
      index[x] = val
      console.log(index)
      axios.put('http://' + hostname + '/api/mahasiswa/' + index[0], {
        id: index[0],
        nrp: index[1],
        name: index[2],
        jk: index[3],
        tl: index[4],
        ukt: index[5],
        aktif: index[6],
        link: index[7]
      }).then((res) => {
        console.log(res.data)
      })
    })
}

var addrow = function(instance) {
  axios({
    method: 'post',
    url: 'http://' + hostname + '/api/mahasiswa/',
    data: {
    }
  })
    .then((response) => {
      console.log(response.data)
    })
}

var delrow = function(instance, id) {
  axios({
    method: 'get',
    url: 'http://' + hostname + '/api/mahasiswa/'
  })
    .then((response) => {
      var temp = Object.keys(response.data[id]).map(function (key) {
        return response.data[id][key]
      })
      axios
        .delete('http://' + hostname + '/api/mahasiswa/' + temp[0])
      console.log(response.data)
    })
}

export default {
  name: 'App',
  mounted: function () {
    axios.get('http://' + hostname + '/api/mahasiswa/').then(res => {
      console.log(res.data)
      var jexcelOptions = {
        data: res.data,
        onchange: update,
        oninsertrow: addrow,
        ondeleterow: delrow,
        allowToolbar: true,
        columns: [
          { type: 'hidden' },
          { type: 'text', title: 'NRP', width: '200px' },
          { type: 'text', title: 'Nama', width: '200px' },
          { type: 'dropdown', title: 'Kelamin', width: '100px', source: [ 'Laki-laki', 'Perempuan' ] },
          { type: 'calendar', title: 'Tgl Lahir', width: '100px' },
          { type: 'numeric', title: 'UKT', width: '100px' },
          { type: 'checkbox', title: 'Aktif', width: '50px' },
          { type: 'image', title: 'Photo', width: '300px' }
        ]
      }
      let spreadsheet = jexcel(this.$el, jexcelOptions)
      Object.assign(this, { spreadsheet })
    })
  }
}
</script>
