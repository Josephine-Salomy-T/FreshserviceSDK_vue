<template>
  <div id="tab">
    <Requesters v-if="client" :client="client" />
    <Tickets v-if="client" :client="client" ></Tickets>
  </div>
</template>

<script>
import Requesters from "./components/Requesters.vue";
import Tickets from "./components/Tickets.vue";

export default {
  name: "App",
  components: {
    Requesters,
    Tickets,
  },
  data() {
    return {
      client: null,
    };
  },
  mounted() {
    this.initialize();
  },
  methods: {
    initialize() {
      console.log("Initializing app...");
      app.initialized()
        .then(client => {
          console.log("App initialized successfully", client);
          this.client = client;
          client.events.on("app.activated", () => {
            client.instance.resize({ height: "400px" });
            console.log("App activated event triggered");
            this.$emit("appActivated", client);
          });
        })
        .catch(error => {
          console.error('Error during app initialization:', error);
        });
    },
  },
};
</script>

<style scoped>
</style>
