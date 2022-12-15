<template>
  <Nav />

  <div class="container-account">
    <div class="account-details">

        <div class="account-name">
          <h3 class="account-title-name">User Name: </h3>
          <p class="account-user-name"> {{ useUserStore().profile ? useUserStore().profile.username : "Name" }}</p>
        </div>

        <div class="account-mail">
          <h3 class="account-title-mail">Account Email: </h3>
          <p class="account-user-mail">{{useUserStore().user.email}}</p>
        </div>

        <div class="avatar-img">
             <Avatar v-model:path="avatar_url" @upload="profiles" class="avatar"/>
          </div>   

    </div>

    <!-- New Form-->
    <div class="edit-section">
        <h3>You can change your name here:</h3>
        <form class="form-box" @submit.prevent="updateProfile">
          <div class="username-form">
            <label for="new-username">New User Name: </label>
            <input
              class="username-input"
              id="new-username"
              type="text"
              v-model="username"
            />
          </div>
          <div>
            <button type="submit" class="edit-btn">Edit</button>
          </div>
        </form>
    </div>
</div>
<Footer />
</template>

<script setup>
  import { supabase } from '../supabase';
  import { onMounted, ref, toRefs } from 'vue';
  import { useUserStore } from "../stores/user";
  import Nav from '../components/Nav.vue';
  import Footer from '../components/Footer.vue';
  import Avatar from '../components/Avatar.vue';

  const userStore = useUserStore();

  const loading = ref(false);
  const username = ref(null);
  const website = ref(null);
  const avatar_url = ref(null);
  const profile = ref("");
  const email = ref("");

  async function getProfile() {
    await userStore.fetchUser();
    console.log(userStore.profile);
    username.value = userStore.profile.username;
    email.value = userStore.profile.email;
    avatar_url.value = userStore.profile.avatar_url;
}
onMounted(() => {
    getProfile();
  });

  async function updateProfile() {
    try {
      loading.value = true
      
      let { data, error } = await supabase
        .from('profiles')
        .update({
        user_id: userStore.user.id,
        username: username.value,
        avatar_url: avatar_url.value,      
        })
        .match({ user_id: useUserStore().user.id })

      if (error) throw error
       } catch (error) {
         alert(error.message)
       } finally {
         loading.value = false
       }
  } 

 async function signOut() {
    try {
      loading.value = true
      let { error } = await supabase.auth.signOut()
      if (error) throw error
    } catch (error) {
      alert(error.message)
    } finally {
      loading.value = false
    }
  }
</script>

<style></style>