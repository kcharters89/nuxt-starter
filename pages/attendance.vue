<template>
  <!--Page html -->
  <main id="app">
    <aside>
      <span>IN790 </span>
      <label for="birthday">Date:</label>
      <input type="text" id="datepicker" />
      <label for="autofill">Autofill</label>
      <select @change="autofill($event)" v-model="key" id="autofill">
        <option value="" disabled selected>Select your option</option>
      </select>
    </aside>
 
    <section>
     
      <table id="mytab1" class="table">
      
        <tbody>
          
          <tr>
            <th>Student ID</th>
            <!--vue and nuxt use the v-for and v-bind to loop, data is sliced for first 16 of the api-->
            <td
              v-for="(persons, index) in data.slice(0, 16)"
              v-bind:key="index"
            >
              {{ persons.id }}
            </td>
          </tr>
          <tr>
            <th>Student Name</th>
            <td
              v-for="(persons, index) in data.slice(0, 16)"
              v-bind:key="index">
              {{ persons.name.first + " " + persons.name.last }}
            </td>
          </tr>
        </tbody>
      
      </table>
  
      <table id="dtHorizontalExample" class="table" >
       
       <tbody>
          <tr id="status">
            <th id="statusday"></th>
            <td
              id="dropdowns"
              v-for="(persons, index) in data.slice(0, 16)"
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
    
      <button type="button" @click="alert" class="save">Save</button>
    
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
    this.datepicker();


  },
  //methods are functions that you call after site load, for onclick or onchange
  methods: {
    //method to make console log work on vue pages
    log(msg) {
      console.log(msg);
    },
    datepicker() {
      // date picker showing todays date
      $("#datepicker")
        .datepicker()
        .datepicker("setDate", new Date());
    },
    autofill(e) {
      //auto fill of all select options
      //find row
      var x = document.getElementById("dtHorizontalExample").rows;
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
        //save previous data and then adding new row for next day
        var clonedtable = $("#dtHorizontalExample tr:last").clone(true);
        var clonedselect = clonedtable.find("select");
       
        clonedselect.prop("disabled", false);
        clonedtable.appendTo("#dtHorizontalExample");
      } else {
        // Do nothing!
      }
    },
    save() {
      //making the options disabled for that coloumn when the save button is pressed and make new row
      $('select:not("#autofill")').prop("disabled", true);
      var date = $("#datepicker").datepicker("getDate");
      var setDate = $.datepicker.formatDate("dd-mm-yy", date);
     
      $('#statusday').text(setDate);
      
     
    }
  }
};
</script>
