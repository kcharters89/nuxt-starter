<template>
  <!--Page html -->
  <main id="app">
  <div class ="container">  
    <header>
      <h4>Stream A </h4>
      <label for="birthday" class="info" >Todays Date:</label>
      <input type="date" id="date_picker" @change="handler($event)"/>
      <label for="autofill" class="info">Autofill:</label>
      <select @change="autofill($event)" v-model="key" id="autofill">
        <option value="" disabled selected>Select your option</option>
      </select>
     <nuxt-link to="/classes">  <button type="button" class="info" >Classes</button></nuxt-link>
      <nuxt-link to="/">  <button type="button" class="info">Logout</button></nuxt-link>
    </header>
    
<section>  

        <table id="mytab1" class="table">
          <!--vue and nuxt use the v-for and v-bind to loop, data is sliced for first 16 of the api-->

          <tbody>
            <tr>
              <th scope="row">Student ID</th>
              <td
                id="id"
                class="id"
                v-for="(persons, index) in data.slice(0, 15)"
                v-bind:key="index"
              >
                {{ persons.id }}
              </td>
            </tr>

            <tr>
              <th scope="row">Student Name</th>
              <td
                id="names"
                class="names"
                v-for="(persons, index) in data.slice(0, 15)"
                v-bind:key="index"
              >
                {{ persons.name.first + " " + persons.name.last }}
              </td>
            </tr>
            <tr>
              <th id="statusday" scope="row">Status</th>
              <td
                id="dropdowns"
                class="dropdowns"
                v-for="(persons, index) in data.slice(0, 15)"
                v-bind:key="index"
              >
                <select id="select" name="dropdown" class="notdisable"
                  ><option id="options" value="" disabled selected
                    >Select your option</option
                  ></select
                >
              </td>
            </tr>
          </tbody>
        </table>
</section>
     
    <button type="submit" @click="alert" class="btn btn-success">Save</button>
   </div>
  </main>
</template>
<!--Page script, no sure how to add an external javascript yet--->
<script>
import axios from "axios";

export default {
  name: "index",
  //data holds all arrays and data for use
  data() {
    return {
      name: "",
      data: [],
      students: [],
      key: ""
    };
  },
  asyncData() {
    //axios get request and parse's data to arrays created in data method
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
    this.datepicker();
    this.statusdropdowns();
  },
  //methods are functions that you call after site load, for onclick or onchange
  methods: {
    //method to make console log work on vue pages
    log(msg) {
      console.log(msg);
    },
    statusdropdowns() {
      var values = ["Present", "Sick", "Absent", "Late", "Explained"];
      //takes each select and adds the 5 options to it
      
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
    datepicker() {
      // date picker showing todays date
     
    Date.prototype.toDateInputValue = (function() {
    var local = new Date(this);
    local.setMinutes(this.getMinutes() - this.getTimezoneOffset());
    return local.toJSON().slice(0,10);
});
document.getElementById('date_picker').value = new Date().toDateInputValue();

    },
    handler(e){

document.getElementById('dropdowns').selectedIndex = 0;
 $('#statusday').text("Status");
 $('select:not("#autofill")').prop("disabled", false);
    },
    
    autofill(e) {
      //auto fill of all select options
     
      var x = document.querySelectorAll("#dropdowns");
     
      //loop through all options
      for (var i = 0; i < x.length; i++) {
        var present = document.getElementById("Present").value;
        var sick = document.getElementById("Sick").value;
        var late = document.getElementById("Late").value;
        var explained = document.getElementById("Explained").value;
        var absent = document.getElementById("Absent").value;
        //check that the option picked in autofill matches the value
        //change that value for all depending on which one is chosen
        if (e.target.value == present) {
          x[i].firstChild.value = present;
        } else if (e.target.value == sick) {
          x[i].firstChild.value = sick;
        } else if (e.target.value == late) {
          x[i].firstChild.value = late;
        } else if (e.target.value == explained) {
          x[i].firstChild.value = explained;
        } else if (e.target.value == absent) {
          x[i].firstChild.value = absent;
        }
      }
    },
    alert() {
      //alert to confirm if that want this data saved for the day
      if (confirm("Are you sure you want to save this data?")) {
        this.save();
      } else {
        // Do nothing!
      }
    },
    save() {
      //making the options disabled for that coloumn when the save button is pressed and make new row
     
      $('select:not("#autofill")').prop("disabled", true);
      var date = document.getElementById('date_picker').value;
    
      $("#statusday").text(date);
      
    }
  }
};
</script>
