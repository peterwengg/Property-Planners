<template>
    <div class="container-fluid py-3 px-5">
    <div class="flex-row d-flex justify-content-center">
      <div class="col-12">
        <p class="pp-head text-start">Dashboard</p>
        <div class="border-0 text-center">
          <img
            class="p-3 mx-auto illustration"
            src="../../assets/Empty_Dashboard.svg"
            alt="Empty Dashboard"
          />
          <p class="pp-subhead my-3 mx-auto" id="ill-subhead">
            Start adding components to your dashboard to view them in one look!
          </p>
          <button
          type="button"
          class="mx-auto btn btn-primary rounded-pill btn-lg pp-button"
          @click="getStarted()"
        >
          Start
        </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "EmptyAgentProfile",
  data() {
    return {
      agent_registration_no: "R045184G",
    };
  },
  methods: {
    getStarted(){
      localStorage.hasComponents = true
      document.location.reload(true);
    }
  }
}
</script>