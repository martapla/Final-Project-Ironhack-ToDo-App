<template>
  <nav>
      <section class="desktop">
          <ul class="nav-links">

              <li>
                <router-link to="/" class="link-router">Home</router-link>
              </li>
              <li>
                <router-link to="/" class="link-router">Tasks</router-link>
              </li>

              <li>
                <router-link to="/account" class="link-router">Account</router-link>
              </li>
          </ul>

          <div>
            <ul class="log-out-welcome">
                <li >
                  <p class="welcome">Welcome {{username}}!</p>
                </li>
                <li>
                  <button @click="signOut" class="button-logout">Log out</button>
                </li>
            </ul>
          </div>
      </section>
      <section class="mobile">       
          <div class="hamburger">
              <span class="bar"></span>
              <span class="bar"></span>
              <span class="bar"></span>
          </div>
          <div class="burger-list">
              <ul class="mobile-links">
                <li>
                  <router-link to="/" class="link-router">Home</router-link>
                </li>
                <li>
                  <router-link to="/" class="link-router">Tasks </router-link>
                </li>

                <li>
                  <router-link to="/account" class="link-router">Account</router-link>
                </li>
                <li>
                  <button @click="signOut" class="button-logout">Log out</button>
                </li>
                </ul>
          </div>
      </section> 
 
  </nav>
</template>

<script setup>
// import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { ref, onMounted } from 'vue';

//constant to save a variable that will hold the use router method
const route = "/";
const buttonText = "Todo app";

// constant to save a variable that will get the user from store with a computed function imported from vue
// const getUser = computed(() => useUserStore().user);
const getUser = useUserStore().user;
const username = ref("");
// constant that calls user email from the useUSerStore
const userEmail = getUser.email;

// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();

const signOut = async () => {
  try {
    // call the user store and send the users info to backend to signOut
    await useUserStore().signOut();
    
    // then redirect user to the homeView
    redirect.push({ path: "/auth/login" });

  } catch (error) {
    errorMsg.value = error.message;
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
  return;
  errorMsg.value = "error";
};

const updateData = async () => {
  await useUserStore().fetchUser();
  //Descargamos la cadena vacía
  username.value = useUserStore().profile.username;
 }
updateData();

onMounted(() => { 
  
   // Hamburguer
  const hamburger = document.querySelector(".hamburger");
  const burgerlist = document.querySelector(".mobile-links");



  console.log(hamburger);

  hamburger.addEventListener("click", () => {
  hamburger.classList.toggle("active");
  burgerlist.classList.toggle("show-active");
   
 })

})

</script>

<style></style>
