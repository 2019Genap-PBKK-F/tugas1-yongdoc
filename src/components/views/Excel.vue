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

var update = function(instance, cell, x, y, val) {
  x = parseInt(x)
  y = parseInt(y)
  var temp = []
  temp[0] = y + 1
  axios
    .get('http://localhost:3000/mahasiswa/' + temp[0])
    .then((response) => {
      Object.keys(response.data).map(function (key) {
        if (key === 'nrp') {
          temp[1] = response.data['nrp']
        }
        if (key === 'name') {
          temp[2] = response.data['name']
        }
        if (key === 'tl') {
          temp[3] = response.data['tl']
        }
        if (key === 'jk') {
          temp[4] = response.data['jk']
        }
        if (key === 'ukt') {
          temp[5] = response.data['ukt']
        }
        if (key === 'aktif') {
          temp[6] = response.data['aktif']
        }
        if (key === 'link') {
          temp[7] = response.data['link']
        }
      })
      temp[x] = val
      axios({
        method: 'put',
        url: 'http://localhost:3000/mahasiswa/' + temp[0],
        data: {
          id: temp[0],
          nrp: temp[1],
          name: temp[2],
          tl: temp[3],
          jk: temp[4],
          ukt: temp[5],
          aktif: temp[6],
          link: temp[7]
        }
      })
        .then((response) => {
          console.log(response.data)
        })
    })
}

var addrow = function(instance) {
  axios({
    method: 'post',
    url: 'http://localhost:3000/mahasiswa/',
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
    url: 'http://localhost:3000/mahasiswa/'
  })
    .then((response) => {
      var temp = Object.keys(response.data[id]).map(function (key) {
        return response.data[id][key]
      })
      axios
        .delete('http://localhost:3000/mahasiswa/' + temp[0])
      console.log(response.data)
    })
}

var options = {
  url: 'http://localhost:3000/mahasiswa',
  onchange: update,
  oninsertrow: addrow,
  ondeleterow: delrow,
  allowToolbar: true,
  columns: [
    { type: 'hidden' },
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
