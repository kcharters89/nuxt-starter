<template>
  <!--Page html -->
  <main id="app">
    <aside>
      <span>IN790 </span>
      <label for="birthday">Date:</label>
      <input type="text" id="date_picker" name="date" />
      <label for="autofill">Autofill</label>
      <select @change="autofill($event)" v-model="key" id="autofill">
        <option value="" disabled selected>Select your option</option>
      </select>
    </aside>
    <div class="container">
      <div class="overflow">
        <table id="mytab1" class="table">
          <!--vue and nuxt use the v-for and v-bind to loop, data is sliced for first 16 of the api-->

          <tbody>
            <tr>
              <th>Student ID</th>
              <td
                id="id"
                class="id"
                v-for="(persons, index) in data.slice(25,50)"
                v-bind:key="index"
              >
                {{ persons.id }}
              </td>
            </tr>

            <tr>
              <th>Student Name</th>
              <td
                id="names"
                class="names"
                v-for="(persons, index) in data.slice(25,50)"
                v-bind:key="index"
              >
                {{ persons.name.first + " " + persons.name.last }}
              </td>
            </tr>
            <tr>
              <th id="statusday">Status</th>
              <td
                id="dropdowns"
                class="dropdowns"
                v-for="(persons, index) in data.slice(25,50)"
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
      </div>
    </div>
    <button type="button" @click="alert" class="btn btn-success">Save</button>
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
      $("#date_picker")
        .datepicker()
        .on("input change", function() {
          $("#dropdowns").load(document.URL + " #dropdowns");
        });
      $("#date_picker")
        .datepicker()
        .datepicker("setDate", new Date());
    },
    autofill(e) {
      //auto fill of all select options
      //find row
      var x = document.getElementById("mytab2").rows;
      //starting at row 2 where our select status options start
      for (var i = 0; i < x.length; i++) {
        //pick next coloumn when a new coloumn is made
        var y = x[i].cells;
      }
      //loop through all options
      for (var i = 1; i < y.length; i++) {
        var present = document.getElementById("Present").value;
        var sick = document.getElementById("Sick").value;
        var late = document.getElementById("Late").value;
        var explained = document.getElementById("Explained").value;
        var absent = document.getElementById("Absent").value;
        //check that the option picked in autofill matches the value
        //change that value for all depending on which one is chosen
        if (e.target.value == present) {
          y[i].firstChild.value = present;
        } else if (e.target.value == sick) {
          y[i].firstChild.value = sick;
        } else if (e.target.value == late) {
          y[i].firstChild.value = late;
        } else if (e.target.value == explained) {
          y[i].firstChild.value = explained;
        } else if (e.target.value == absent) {
          y[i].firstChild.value = absent;
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
      var date = $("#date_picker")
        .datepicker({ dateFormat: "yy-mm-dd" })
        .val();

      $("#statusday").text(date);
    }
  }
};
</script>
