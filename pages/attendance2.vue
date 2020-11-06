<template>
  <main id="app">
    <h3>IN760</h3>
    
    <span>Teacher</span>

    <label for="autofill">Autofill</label>
    <select @change="autofill($event)" v-model="key" id="autofill">
     
    </select>

    <section>
      <table id="mytab1">
        <tbody>
          <tr>
            <th>Student ID</th>
            <td v-for="(persons, index) in data.slice(17, 33)" v-bind:key="index">{{ persons.id }}</td>
          </tr>
          <tr>
            <th>Student Name</th>
            <td
              v-for="(persons, idx) in data.slice(17, 33)"
              v-bind:key="idx"
            >{{ persons.name.first + " " + persons.name.last }}</td>
          </tr>
          <tr>
            <th>Status</th>
            <td id="status" v-for="(persons, idx) in data.slice(17, 33)" v-bind:key="idx"></td>
          </tr>
          <tr>
            <th>Dropdowns</th>
            <td id="dropdowns" v-for="(persons, idx) in data.slice(17, 33)" v-bind:key="idx">
              <select id="select"></select>
            </td>
          </tr>
        </tbody>
      </table>
    </section>
    <aside></aside>
  </main>
</template>

<script>
import axios from "axios";

export default {
  name: "index",
  data() {
    return {
      name: "",
      data: [],
      key: ""
    };
  },
  asyncData() {
    // We can return a Promise instead of calling the callback
    return axios
      .get("https://api.github.com/gists/b40fa9bba517ff258da395c79edd2477")
      .then(res => {
        return {
          data: JSON.parse(res.data.files["attendance.json"].content)
        };
      });
  },
  mounted() {
    var values = ["Present", "Sick", "Absent", "Late", "Explained"];

    var select = document.getElementById("select");

    var elements = document.getElementsByTagName("select");
    for (var i = 0; i < elements.length; i++) {
      for (const val of values) {
      var option = document.createElement("option");
      option.value = val;
      option.text = val.charAt(0).toUpperCase() + val.slice(1);
      
      elements[i].append(option);
      }
      
    }
  },
  before() {},
  methods: {
 
    //method to make console log work on vue pages
    log(msg) {
      console.log(msg);
    },
   
    autofill(e) {},
    submitData(){
      var storeDropdown = document.getElementById('select').value;
    }
  },
  computed: {
    info_title: function() {
      return [...new Set(this.data.map(i => i.class))];
    }
  }
};
</script>

