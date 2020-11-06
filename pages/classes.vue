<template>
<main>
    <section class="form">
      <h3>Your Classes</h3>
 <span class="dropdown">
  <button @click="streamlist" id="dropbtn">IN705 Databases 3</button>
  <span id="myDropdown" class="dropdown-content">
    <ul>
   <li> <a > <nuxt-link to="/attendance">Stream A</nuxt-link></a></li>
   <li> <a > <nuxt-link to="/attendance2">Stream B</nuxt-link></a></li>
    </ul>
  </span>
</span>
     </section>
     </main>        
</template>
<script>
export default {
methods:{
   streamlist(){
    
  document.getElementById("myDropdown").classList.toggle("show");


// Close the dropdown if the user clicks outside of it
window.onclick = function(event) {
  if (!event.target.matches('#dropbtn')) {
    var dropdowns = document.getElementsByClassName("dropdown-content");
    var i;
    for (i = 0; i < dropdowns.length; i++) {
      var openDropdown = dropdowns[i];
      if (openDropdown.classList.contains('show')) {
        openDropdown.classList.remove('show');
      }
    }
  }
}
   }
}
    
}
</script>
<style lang="scss">

.dropdown-content {
  display: none;
  
}
.show {display: block;}
</style>