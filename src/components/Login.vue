<script setup>
  import { RouterLink } from "vue-router";
</script>
<template>
    <div class="container-fluid" id="login-fluid">
        <div class="container py-5 h-100">
            <div class="row d-flex justify-content-center align-items-center h-100">
                <div class="col-md-4 col-lg-6 col-xl-7"></div>
                <div class="col-12 col-md-8 col-lg-6 col-xl-5">
                    <div class="card text-start"
                        style="border-radius: 1rem; background-color: rgba(255, 255, 255, 0.75);">
                        <div class="card-body p-5">
                            <h5>Welcome to <span style="color: #779341;">PropertyPlanners</span></h5>
                            <h1 class="mb-5 fw-semibold">Sign In</h1>

                            <!-- <div class="row form-outline mb-4">
                            <div class="col-6">
                                <label class="form-label" for="fullName">Full Name</label>
                            <input type="text" id="fullName" class="form-control" placeholder="Full name"/>
                            </div>
                            <div class="col-6">
                                <label class="form-label" for="role">Role</label>
                                <select id="role" class="form-control">
                                    <option selected disabled>Select Role</option>
                                    <option value="seeker">Property Seeker</option>
                                    <option value="agent">Property Agents</option>
                                </select>
                            </div>
                        </div> -->

                            <div class="form-outline mb-4">
                                <label class="form-label" for="emailAddress">Enter your email address</label>
                                <input type="email" id="emailAddress" v-model="email" class="form-control"
                                    placeholder="Email address" @keyup.enter="logIn()" />
                            </div>

                            <div class="form-outline mb-4">
                                <label class="form-label" for="password">Enter your password</label>
                                <input type="password" id="password" v-model="password" class="form-control"
                                    placeholder="Password" @keyup.enter="logIn()" />
                                <p class="pt-3 d-none" id="error_message" style="color: red; font-style: italic"></p>
                            </div>


                            <div class="row">
                                <div class="col-12 col-md-6">
                                    <p>New User? <br><RouterLink @click="goSignup()" :to="'/signup'" style="color: #779341;">Sign up</RouterLink></p>
                                </div>

                                <div class="col-12 col-md-6 d-flex justify-content-end">
                                    <button class="btn btn-default btn-block btn-lg" @click="logIn()" type="submit"
                                        style="background-color: #779341; color: white; width: 100%;">Sign In</button>
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
import { auth } from '../scripts/fbauth'
import { signInWithEmailAndPassword } from 'firebase/auth'
import { spinnerOn, spinnerOff } from '../scripts/spinner'
import { fsdb } from '../scripts/fb'
import { doc, getDoc } from "firebase/firestore";

export default {
    name: 'login',
    data() {
        return {
            email: "",
            password: "",
            user: "",
            type: "",
        }
    },
    methods: {
        async logIn() {
            try {

                if (this.email.trim().length < 1 ||
                    this.password.trim().length < 1) {
                    document.getElementById("error_message").innerText = "Please fill up all fields";
                    document.getElementById("error_message").classList.remove("d-none");
                    throw 'ValurError'
                }
                document.getElementById("error_message").innerText = ""
                spinnerOn()
                var credential = await signInWithEmailAndPassword(auth, this.email, this.password)
                spinnerOff()
                this.user = credential.user
                localStorage['uid'] = this.user.uid

                const docRef = doc(fsdb, "users", this.user.uid);
                const docSnap = await getDoc(docRef);

                if (docSnap.exists()) {
                    var data = docSnap.data()

                    this.type = data.type
                    localStorage['type'] = this.type
                    localStorage['name'] = data.name
                    localStorage['email'] = data.email
                    localStorage['aboutme'] = data.name
                    localStorage['profilepic'] = data.profilepic
                    localStorage['preferences'] = JSON.stringify(data.preferences)

                    this.$router.push({ path: `/${this.type}/general` })
                } else {
                    // doc.data() will be undefined in this case
                    throw 'User dont exist'
                }


            }
            catch (e) {
                console.log(e)
                console.log(e.code)
                console.log(e.message);
                if(document.getElementById("error_message").innerText != "Please fill up all fields"){
                    document.getElementById("error_message").innerText = "Invalid Email Address or Password!";
                }
                spinnerOff()
            }
        },
        goSignup(){
            this.$emit('goSignupFromLogin')
        }
    }

}

</script>

<style>
#login-fluid {
    background-image: url('../assets/loginbg.jpg');
    background-size: cover;
    background-repeat: no-repeat;
    height: 100%;
    min-height: 800px;
}
</style>