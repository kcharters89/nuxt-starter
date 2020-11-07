<template>
<main>

  <section class="form"> 
    <img src="/loginlogo.png"/>
    <form class="login-form">
      <input type="text" placeholder="username"/>
      <input type="password" placeholder="password"/>
     <nuxt-link to="/classes">  <button type="button"> login</button>  </nuxt-link>
    </form>
  </section>

</main>

</template>
<script>
export default {

    
}
</script>
