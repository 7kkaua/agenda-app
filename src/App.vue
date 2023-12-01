<template>
  <div id="app">
     <div class="container mt-4">
       <AddEvent @eventAdded="handleEventAdded"/>
       <hr>
       <EventList :events="eventsList" @eventsUpdated="updateEventsList"/>
     </div>
  </div>
 </template>
 
 <script>
 import 'bootstrap/dist/css/bootstrap.css';
 import 'bootstrap';
 import EventList from './components/EventList.vue';
 import AddEvent from './components/AddEvent.vue';
 

 export default {
   name: 'App',
   components: {
     EventList,
     AddEvent,
   },
   data() {
     return {
      eventsList: []
     }
   },
   created() {
    const savedEvents = localStorage.getItem('events');
    if (savedEvents) {
      this.eventsList = JSON.parse(savedEvents);
    }
   },
   methods: {
    handleEventAdded(newEvent) {
      this.eventsList.push(newEvent);
      localStorage.setItem('events', JSON.stringify(this.eventsList));
    },
    updateEventsList(updatedEvents) {
      this.eventsList = updatedEvents;
      localStorage.setItem('events', JSON.stringify(this.eventsList));
    },
   }
 }
 </script>
 
 <style scoped>
    .container  {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 20px;
    }

    

    

 </style>