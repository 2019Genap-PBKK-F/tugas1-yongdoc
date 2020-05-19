<template>
  <div style="padding-left: 20px; padding-right: 20px">
    <br>
    <label >Pilih Satuan Kerja</label>
    <select class="form-control" v-model="filterSatker" @change="load()">
      <option v-for="data in fk_satker" v-bind:value='data.id_sk'>{{ data.nama }}</option>
    </select>
    <div class="span4">
      <img src="/static/img/its.png" class="img-fluid" style="max-width: 75px; float: left">
      <h1 style="padding-left: 90px">
        Indikator Kontrak Kinerja 2020
        <br>
        <small>
          Kepala Departemen
        </small>
      </h1>
    </div>
    <br>
    <table class="table table-striped">
      <thead class="thead-dark">
        <tr role="row">
          <th>No</th>
          <th>Aspek</th>
          <th>Komponen</th>
          <th>Indikator</th>
          <th>Bobot</th>
          <th>Target</th>
          <th>Capaian</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="insatker in indikatorData" role="row">
          <td>{{ insatker.num }}</td>
          <td>{{ insatker.Aspek }}</td>
          <td>{{ insatker.Komponen }}</td>
          <td>{{ insatker.Indikator }}</td>
          <td>{{ insatker.Bobot }}</td>
          <td>{{ insatker.Target }}</td>
          <td>{{ insatker.Capaian }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import 'webdatarocks/webdatarocks.min.css'
import axios from 'axios'

// var host = 'http://10.199.14.46:8023/'
var host = 'http://localhost:8023/'

export default {
  name: 'App',
  data() {
    return {
      indikatorData: [],
      filterSatker: [],
      fk_satker: []
    }
  },
  mounted() {
    this.getSatker()
    // this.load()
  },
  methods: {
    load() {
      axios.get(host + 'api/indikatorsatker/' + this.filterSatker).then(res => {
        console.log(res.data)
        this.indikatorData = Object.values(res.data)
      })
    },
    getSatker() {
      axios.get(host + 'api/dropdownkonkin').then(res => {
        this.fk_satker = res.data
      })
    }
  }
}
</script>
