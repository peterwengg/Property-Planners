<script setup>
  import { RouterLink } from "vue-router";
</script>

<template>
  <div class="container-fluid" id="login-fluid">
    <div class="container py-5 h-100">
      <div class="row d-flex justify-content-center align-items-center h-100">
        <div class="col-md-4 col-lg-6 col-xl-7"></div>
        <div class="col-12 col-md-8 col-lg-6 col-xl-5">
          <div
            class="card text-start"
            style="
              border-radius: 1rem;
              background-color: rgba(255, 255, 255, 0.75);
            "
          >
            <div class="card-body p-5">
              <h5>
                Welcome to <span style="color: #779341">PropertyPlanners</span>
              </h5>
              <h1 class="mb-5 fw-semibold">Sign up</h1>

              <div class="row form-outline mb-4">
                <div class="col-6">
                  <label class="form-label" for="fullName">Full Name</label>
                  <input
                    type="text"
                    id="fullName"
                    v-model="name"
                    class="form-control"
                    placeholder="Full name"
                  />
                </div>
                <div class="col-6">
                  <label class="form-label" for="role">Role</label>
                  <select id="role" v-model="type" class="form-control">
                    <option disabled>Select Role</option>
                    <option value="seeker">Property Seeker</option>
                    <option value="agent">Property Agent</option>
                  </select>
                </div>
              </div>

              <div class="form-outline mb-4">
                <label class="form-label" for="em   ailAddress"
                  >Enter your email address</label
                >
                <input
                  type="email"
                  id="emailAddress"
                  v-model="email"
                  class="form-control"
                  placeholder="Email address"
                />
              </div>

              <div class="form-outline mb-4">
                <label class="form-label" for="password"
                  >Enter your password</label
                >
                <input
                  type="password"
                  id="password"
                  v-model="password"
                  class="form-control"
                  placeholder="Password"
                />
                <p class="pt-3 d-none" id="error_message" style="color: red; font-style: italic"></p>

              </div>

              <div class="row">
                <div class="col-12 col-md-6">
                  <p>
                    Have an Account? <br /><RouterLink
                      @click="goLogin()"
                      :to="'/login'"
                      style="color: #779341"
                      >Sign in</RouterLink
                    >
                  </p>
                </div>

                <div class="col-12 col-md-6 d-flex justify-content-end">
                  <button
                    @click="signUp()"
                    class="btn btn-default btn-block btn-lg"
                    type="submit"
                    style="background-color: #779341; color: white; width: 100%"
                  >
                    Sign Up
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { auth } from "../scripts/fbauth";
import { createUserWithEmailAndPassword } from "firebase/auth";
import { spinnerOn, spinnerOff } from "../scripts/spinner";
import { fsdb } from "../scripts/fb";
import { doc, setDoc } from "firebase/firestore";

export default {
  name: "signup",
  data() {
    return {
      type: "seeker",
      email: "",
      password: "",
      user: {},
      name: "",
      boa: "https://firebasestorage.googleapis.com/v0/b/propertyplanners-93ebc.appspot.com/o/images%2Fboa.jpg?alt=media&token=e1b95b58-269d-4b9b-b23d-1309c942287f",
      luffy:
        "https://firebasestorage.googleapis.com/v0/b/propertyplanners-93ebc.appspot.com/o/images%2Fluffy.jpg?alt=media&token=c1c97139-fd1d-4f0a-8fe6-ab41af4449be",
    };
  },
  mounted(){
    this.$emit('goLogin')
  },
  methods: {
    async signUp() {
      try {
        if (
          this.type.trim().length < 1 ||
          this.email.trim().length < 1 ||
          this.password.trim().length < 1 ||
          this.name.trim().length < 1
        ) {
          document.getElementById("error_message").innerText = "Please fill up all fields";
          document.getElementById("error_message").classList.remove("d-none");
          throw "ValurError";
        }

        spinnerOn();
        var credential = await createUserWithEmailAndPassword(
          auth,
          this.email,
          this.password
        );
        this.user = credential.user;
        localStorage["uid"] = this.user.uid;
        localStorage["type"] = this.type;
        localStorage["name"] = this.name;
        localStorage["email"] = this.email;

        var data = {};
        if (this.type == "seeker") {
            var preferences = [
            {
              name: "Region",
              value: "",
            },
            {
              name: "Property Type",
              value: "",
            },
            {
              name: "Budget",
              value: "",
            },
            {
              name: "Non-Negotiables",
              value: "",
            },
            {
              name: "Good to haves",
              value: "",
            },
          ]
          data = {
            name: this.name,
            email: this.email,
            type: this.type,
            profilepic: this.luffy,
            preferences: preferences
          };
          localStorage["preferences"] = JSON.stringify(preferences)
        }else{
            data = {
            name: this.name,
            email: this.email,
            type: this.type,
            profilepic: this.boa,
            hasPlans: false,
            daysLeft: 0,
            endDate: new Date(),
            views: 0
          };
        }

        await setDoc(doc(fsdb, "users", this.user.uid), data);
        spinnerOff();
        
        this.$router.push({ path: `/${this.type}/general` });
      } catch (e) {
        console.log(e);
        spinnerOff();
      }
    },
        goLogin(){
            this.$emit('goLoginFromSignup')
        }
  }
};
</script>

<style>
#login-fluid {
  background-image: url("../assets/loginbg.jpg");
  background-size: cover;
  background-repeat: no-repeat;
  height: 100%;
  min-height: 800px;
}
</style>
