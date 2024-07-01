<template>
    <div id="app" class="App">
      <button @click="createTicket">Create Ticket</button>
      <div id="ticketDiv" v-if="ticketDetails.id">
        <h4>Ticket Created</h4>
        <p>Ticket ID: {{ ticketDetails.id }}</p>
        <p>Subject: {{ ticketDetails.subject }}</p>
        <p>Description: {{ ticketDetails.description }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      client: {
        type: Object,
        required: true
      }
    },
    data() {
      return {
        ticketDetails: {} // Initialize as an object
      };
    },
    methods: {
      async createTicket() {
        try {
          const options = {
            description: "Details about the issue...",
            subject: "Support Needed...",
            email: "tom@outerspace.com",
            priority: 1,
            status: 2,
            requester_id: 29001640418,
            phone: "4564" // Ensure phone is a string
          }; 
        
          const response = await this.client.request.invoke("oncreateTicket", { data: options });
          const ticketData = JSON.parse(response.response.response);
          console.log("Got ticket data:", ticketData);
          this.ticketDetails = ticketData.ticket;
          await this.createNotes(this.client, ticketData.ticket.id);
          let id=ticketData.ticket.id;
          try {
              await this.client.interface.trigger("openTicket", {
              id: id
              });
          } catch (error) {
              console.error(error);
          }


          try {
            let data = await this.client.interface.trigger("showNotify", {
              type: "success",
              title: "Success!!", /* The "title" should be plain text */
              message: "The ticket is created successfully" /* The "message" should be plain text */
            });
            console.log(data); // success message
          } catch (error) {
              // failure operation
              console.error(error);
          }

        } catch (error) {
          console.error('Error creating ticket:', error);
          
        }
        },
        async createNotes(client, ticketId) {
      try {
        const options = {
          "body": "hello"
        };

        const response = await client.request.invoke("oncreateNote", { data: { ticket_id: ticketId,options } });
        try {
            let data = await this.client.interface.trigger("showNotify", {
              type: "success",
              title: "Success!!", /* The "title" should be plain text */
              message: "A note has been successfully added to the ticket" /* The "message" should be plain text */
            });
            console.log(data); // success message
          } catch (error) {
              // failure operation
              console.error(error);
          }
        } catch (error) {
        console.error('Error creating note:', error);
        }
    }
      },
    created() {
      console.log('CreateTicket component created');
    }
  };
  </script>
  <style>
  button{
  height: 30px;
  width: 150px;
  border-radius: 5px;
}
</style>