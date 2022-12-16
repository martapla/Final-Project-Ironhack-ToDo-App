<!-- COMPONENTE BOILERPLATE -->
 
  <template>

  <div class="main-container">
    <div class="header">
    <h3 class="header-title-in">My To-do List</h3>
        <p class="header-subtitle-in">
          Let's Organize your Daily Tasks !
        </p>
    </div>    

      <form @submit.prevent="signIn" class="form-sign-in">
          <div class="form-sign">
              <div class="form-input">
                  <label class="input-field-label">E-mail</label>
                  <input
                    type="email"
                    class="input-field"
                    placeholder="example@gmail.com"
                    id="email"
                    v-model="email"
                    required
                  />
              </div>
              <div class="form-input">
                  <label class="input-field-label">Password</label>
                  <input
                    type="password"
                    class="input-field"
                    placeholder="**********"
                    id="password"
                    v-model="password"
                    required
                  />
              

                  <button class="button" type="submit">Sign In</button>
              </div>

              <div class="account-box">
                  <p class="text-signup">Don't have an account? 
                    <PersonalRouter :route="route" :buttonText="buttonText" class="sign-up-link"/>
                  </p>
              </div>
          </div>  
      </form>
   
      <div v-show="errorMsg">{{errorMsg}}</div>
  </div>

</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";


// Route Variables
const route = "/auth/signup";
const buttonText = "Sign Up";

// Input Fields
const email = ref("");
const password = ref("");

// Error Message
const errorMsg = ref("");

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Arrow function to Signin user to supaBase
const signIn = async () => {
  try {
    await useUserStore().signIn(email.value, password.value);
    redirect.push({ path: "/" });
  } catch (error) {
    errorMsg.value = error.message;
    setTimeout(() => {
      errorMsg.value = null;
    }, 5000);
  }
  return;
  errorMsg.value = "error";
};
</script>

<style></style>
