<template>
  <div class="container-fluid py-3 px-5">
    <!-- <p class="pp-head text-start">My Profile</p> -->
    <div class="flex-row d-flex justify-content-center">
      <div class="border-0 text-center">
        <img class="p-3 mx-auto illustration" src="../../assets/GetVerified.svg" alt="Empty Dashboard" />
        <p class="pp-subhead my-3 mx-auto" id="ill-subhead">
          Get verified and have your property agent data automatically imported
        </p>
        <!-- <button class="btn btn-success mx-auto">Find Agents</button> -->
        <button type="button" class="mx-auto btn btn-primary rounded-pill btn-lg pp-button" data-bs-toggle="modal"
          data-bs-target="#exampleModal">
          Verify
        </button>
        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
          <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
              <div class="modal-header">
                <h1 class="modal-title fs-5" id="exampleModalLabel">
                  Enter your agent registration number
                </h1>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              </div>
              <div class="modal-body">
                Agent Registration No:
                <input type="text" name="agent_no" id="agent_no" style="width: 100%; text-align: center"
                  placeholder="R045184G" v-model="agent_registration_no" v-on:keyup.enter="getAgentData()" />
                <p class="pt-3" id="error_message" style="color: red; font-style: italic"></p>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
                  Close
                </button>
                <button type="button" class="btn" v-on:click="getAgentData()"
                  style="background-color: #779341; color: white">
                  Submit
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { AgentData } from "../../scripts/agentdata";
import { doc, updateDoc } from "firebase/firestore";
import { spinnerOn } from "../../scripts/spinner";
import { fsdb } from "@/scripts/fb";

export default {
  name: "EmptyAgentProfile",
  data() {
    return {
      agent_registration_no: "R045184G",
      sales: {},
      profile: {},
    };
  },
  methods: {
    test() {
      console.log(this.$route);
    },
    async updateProfile(profile) {
      const userRef = doc(fsdb, "users", localStorage["uid"]);

      await updateDoc(userRef, {
        registration_no: profile.registration_no,
        estate_agent_license_no: profile.estate_agent_license_no,
        estate_agent_name: profile.estate_agent_name,
        registration_end_date: profile.registration_end_date,
        registration_start_date: profile.registration_start_date,
        salesperson_name: profile.salesperson_name,
      });

      localStorage["agent_registration_no"] = this.agent_registration_no

    },
    async getAgentData() {
      spinnerOn();
      var dataGetter = new AgentData(this.agent_registration_no);
      var sales = await dataGetter.getSales();
      var profile = await dataGetter.getProfile();
      console.log("Profile: ", profile, "Sales: ", sales);
      localStorage["agentDetails"] = JSON.stringify(profile);
      if (profile) {
        localStorage.agentStatus = true;
        localStorage.isAgent = true;
        localStorage.registrationNo = this.agent_registration_no;
        await this.updateProfile(profile)
        document.location.reload(true);
      } else {
        document.getElementById("error_message").innerText =
          "Invalid agent registration number, please try again!";
      }
    },
  },
};
</script>
