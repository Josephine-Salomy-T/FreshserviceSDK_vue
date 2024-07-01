///needed

<template>
  <div id="app" class="App">
    <div id="tab">
      <button @click="fetchRequestorData">Get Requestor</button>
    </div>
    <table id="tableDiv">
      <tr v-if="requesters.length">
        <th>Name</th>
        <th>ID</th>
        <th>Email</th>
      </tr> 
      <tr v-for="requester in requesters" :key="requester.id">
        <td>{{ requester.first_name }}</td>
        <td>{{ requester.id }}</td>
        <td>{{ requester.primary_email }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    client: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      requesters: [],
    };
  },
  methods: {
    async fetchRequestorData() {
      try{
          if (!this.client) {
            console.error('Client not initialized');
            return;
          }
          let response = await this.client.request.invoke("onlistAllRequesters",{})
          let requestersData = JSON.parse(response.response.response);
          this.requesters=requestersData.requesters;
      }
      catch(error) {
          console.error('Error fetching requesters:', error);
      };
    },
  },
  created() {
    console.log('Sidebar component created');
  },
};
</script>

<style scoped>
.vue_logo {
  height: 40vmin;
  color: red;
}
td,th{
  margin-top: 20px;
  padding: 10px;
  border: 1px solid silver;
  border-collapse: collapse;
  font-size: 15px;

}
button{
  height: 30px;
  width: 150px;
  border-radius: 5px;
}
#tableDiv{
  margin-top: 20px;
  border: 1px solid silver;
  border-collapse: collapse;
}
</style>
