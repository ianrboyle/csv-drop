<template>
  <div class="container">
    <div class="large-12 medium-12 small-12 cell">
      <label>
        File
        <input type="file" accept=".csv" @change="handleFileUpload($event)" />
      </label>
      <button v-on:click="submitFile()">Submit</button>
    </div>
    <p v-for="(row, rowKey) in data" v-bind:key="'row-' + rowKey">{{ row }}</p>
    <table v-if="parsed" style="width: 100%">
      <thead>
        <tr>
          <th v-for="(header, key) in content.meta.fields" v-bind:key="'header-' + key">
            {{ header }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rowKey) in data" v-bind:key="'row-' + rowKey">
          <td v-for="(column, columnKey) in content.meta.fields" v-bind:key="'row-' + rowKey + '-column-' + columnKey">
            <!-- column is the column name -->
            <p>{{ row["Symbol"] }}, {{ column }}</p>
          </td>
        </tr>
      </tbody>
    </table>
    <table v-if="parsed" style="width: 100%">
      <thead>
        <tr>
          <th v-for="(header, key) in content.meta.fields" v-bind:key="'header-' + key">
            {{ header }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rowKey) in content.data" v-bind:key="'row-' + rowKey">
          <td v-for="(column, columnKey) in content.meta.fields" v-bind:key="'row-' + rowKey + '-column-' + columnKey">
            <input v-model="content.data[rowKey][column]" />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// @ is an alias to /src
// import axios from "axios";
import Papa from "papaparse";

export default {
  data() {
    return {
      file: "",
      content: [],
      parsed: false,
      data: [],
    };
  },
  methods: {
    handleFileUpload(event) {
      this.file = event.target.files[0];
      this.parseFile();
    },
    submitFile() {
      let formData = new FormData();
      formData.append("file", this.file);
      console.log(formData);
      // axios
      //   .post("/single-file", formData, {
      //     headers: {
      //       "Content-Type": "multipart/form-data",
      //     },
      //   })
      //   .then(function () {
      //     console.log("SUCCESS!!");
      //   })
      //   .catch(function () {
      //     console.log("FAILURE!!");
      //   });
    },
    parseFile() {
      Papa.parse(this.file, {
        header: true,
        skipEmptyLines: true,
        complete: function (results) {
          this.content = results;
          this.data = this.content.data;
          console.log(this.content.data);
          this.parsed = true;
        }.bind(this),
      });
    },
  },
};
</script>
