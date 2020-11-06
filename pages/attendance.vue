<template>
  <main id="app">
    <h4>IN790</h4>

    <span>Teacher</span>

    <label for="autofill">Autofill</label>
    <select @change="autofill($event)" v-model="key" id="autofill">
      <option value="" disabled selected>Select your option</option>
    </select>
<label for="birthday">Date:</label>
  <input type="date" >
    <section>
      <table id="mytab1" class="paleBlueRows">
        <tbody>
          <tr>
            <th>Student ID</th>
            <td
              v-for="(persons, index) in data.slice(0, 16)"
              v-bind:key="index"
            >
              {{ persons.id }}
            </td>
          </tr>
          <tr>
            <th>Student Name</th>
            <td v-for="(persons, idx) in data.slice(0, 16)" v-bind:key="idx">
              <span @click="modal">
                {{ persons.name.first + " " + persons.name.last }}</span
              >
            </td>
          </tr>

          <tr id = "status">
            <th>Status</th>
            <td
              id="dropdowns"
              v-for="(persons, idx) in data.slice(0, 16)"
              v-bind:key="idx"
            >
              <select id="select"
                ><option id="options" value="" disabled selected
                  >Select your option</option
                ></select
              >
            </td>
          </tr>
        </tbody>
      </table>
      <button type="button" @click="save">Save</button>
    </section>

    <!-- The Modal -->
    <span
      v-for="(person, index) in students"
      v-bind:key="index"
      id="myModal"
      class="modal"
    >
      <!-- Modal content -->
      <!-- <span class="modal-content">-->
      <span class="close">&times;</span>
      <p>
        {{ person.name.first + " " + person.name.last }}
      </p>
    </span>
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
      students: [],
      key: ""
    };
  },
asyncData(){
   return axios
      .get("https://api.github.com/gists/b40fa9bba517ff258da395c79edd2477")
      .then(res => {
        return {
          data: JSON.parse(res.data.files["attendance.json"].content),
          students: JSON.parse(res.data.files["attendance.json"].content)
        };
      });
      // //not sure how to do local storage on nuxt, it requires plugins and knowledge on the store
  // //which had taken days to try and understand
},
  //mounted is a preset function that runs as the the site loads
  mounted() {
   var values = ["Present", "Sick", "Absent", "Late", "Explained"];
    //takes each select and adds the 5 options
    var elements = document.getElementsByTagName("select");
    for (var i = 0; i < elements.length; i++) {
      for (const val of values) {
        var option = document.createElement("option");
        option.value = val;
        option.id = val;
        option.text = val.charAt(0).toUpperCase() + val.slice(1);

        elements[i].append(option);
      }
    }
  },
  //methods are functions that you call after site load, for onclick or onchange
  methods: {
    //method to make console log work on vue pages
    log(msg) {
      console.log(msg);
    },
    autofill(e) {
      var x = document.getElementById("mytab1").rows;

      var y = x[2].cells;

      for (var i = 1; i < y.length; i++) {
var present = document.getElementById("Present").value
var sick = document.getElementById("Sick").value
var late = document.getElementById("Late").value
var explained = document.getElementById("Explained").value
var absent = document.getElementById("Absent").value
if(e.target.value == present){
y[i].firstChild.value = "Present"
}
else if (e.target.value == sick){
  y[i].firstChild.value = "Sick"
}
else if (e.target.value == late){
  y[i].firstChild.value = "Late"
}
else if (e.target.value == explained){
  y[i].firstChild.value = "Explained"
}
else if (e.target.value == absent){
  y[i].firstChild.value = "Absent"
}

      }
    
    },
    modal() {
      var modal = document.getElementById("myModal");

      // Get the <span> element that closes the modal
      var span = document.getElementsByClassName("close")[0];

      // When the user clicks on the button, open the modal

      modal.style.display = "block";
      // When the user clicks on <span> (x), close the modal
      span.onclick = function() {
        modal.style.display = "none";
      };
      // When the user clicks anywhere outside of the modal, close it
      window.onclick = function(event) {
        if (event.target == modal) {
          modal.style.display = "none";
        }
      };
    },
    save(){
     
    var x=document.getElementById("mytab1").tBodies[0];  //get the table
      var node=x.rows[2].cloneNode(true);    //clone the previous node or row
      
      x.appendChild(node);   //add the node or row to the table
    }
  },
  computed: {
    info_title: function() {
      return [...new Set(this.data.map(i => i.class))];
    }
  }
};
</script>
