<template>
  <div style="text-align: center; margin: 50px;" class="agenda-container">
        <div class="agenda-header">
          <h1>
              <strong>Agenda</strong>
          </h1>
          <div class="search-container">
            <label for="searchName" style="margin-bottom: 15px;"><strong>Pesquisar Cliente</strong></label>
            <input type="text" class="form-control" v-model="searchTerm" id="searchName" placeholder="Digite aqui">
          </div>
        </div>

      <ul class="list-group">
          <li class="list-group-item" v-for="(event, index) in filteredEvents" :key="index" style="background-color: white;">
             <div class="event-item">
               <div class="event-info">
                 <p><strong>Nome:</strong> {{ event.name }}</p>
                  <p><strong>Procedimento:</strong> {{ event.title }}</p>
                  <p><strong>Valor:</strong> {{ event.value }}</p>
                  <p><strong>Telefone:</strong> {{ event.phoneNumber }}</p>
                  <p><strong>Data:</strong> {{ formatDate(event.date) }}</p>
               </div>
             </div>
              <button type="button" class="btn btn-danger" @click="deleteEvent(index)">Excluir</button>
          </li>
      </ul>
  </div>
</template>

<script>


export default {
  name: 'EventList' ,
  props: {
      events: {
          type: Array, 
          default: () => []
      }
  },
  data() {
    return {
      searchTerm: ''
    };
  },

  methods: {
    deleteEvent(index) {
      const copiedEvents = JSON.parse(JSON.stringify(this.events));
      copiedEvents.splice(index, 1);
      localStorage.setItem('events', JSON.stringify(copiedEvents));
      this.$emit('eventsUpdated', copiedEvents);
    },

    formatDate(date) {
      const eventDate = new Date(date);
      const day = String(eventDate.getDate()).padStart(2, '0');
      const month = String(eventDate.getMonth() + 1).padStart(2, '0');
      const year = eventDate.getFullYear();
      return `${day}/${month}/${year}`;
    },
  },

  computed: {
    filteredEvents() {
      const search = this.searchTerm.trim().toLowerCase();
      if (!search) {
        return this.events;
      } else {
        return this.events.filter(event => {
          return event &&
          event.name &&
          typeof event.name === 'string' &&
          event.name.toLowerCase().includes(search);
        });
      }
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700&display=swap');

.agenda-container {
  text-align: center;
  margin: 50px;
  padding-top: 0px;
  padding-bottom: 0px;
  width: 90%;
  max-width: 600px;
}

h1 {
  font-family: 'Montserrat', sans-serif;
}

.agenda-header {
  background-color: #f9f9f9;
  padding: 10px;
  border-radius: 5px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
  padding-top: 1px;
  padding-bottom: 1px;
}

.search-container {
  margin: auto;
  margin-bottom: 15px;
}

.event-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  background-color: white;
  padding: 10px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.event-info {
  background-color: white;
  flex-grow: 1;
}

.event-info p {
  margin: 5px 0;
}

.search-container {
  margin-top: 30px;
  padding-bottom: 30px;
}

.list-group {
  background-color: white;
}


.btn-danger {
    background-color: #f44336;
    color: white;
    border: none;
  }
</style>