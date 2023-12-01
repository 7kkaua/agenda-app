<template>
  <div class="container">
    <h1><strong>Agendar atendimento</strong></h1>
    <form @submit.prevent="addEvent" class="form-container">
      <div class="form-group">
        <label for="inome"><strong>Cliente:</strong></label>
        <input type="text" class="form-control" id="inome" placeholder="Nome" v-model="newfilteredEvent.name" required autocomplete="name">
      </div>
      <div class="form-group">
        <label for="title"><strong>Procedimento:</strong></label>
        <select class="form-control" id="title" v-model="newfilteredEvent.title" required autocomplete="procedure">
          <option value="">Selecione um Procedimento</option>
          <option value="A">A</option>
          <option value="B">B</option>
          <option value="C">C</option>
          <!-- Adicione outras opções conforme necessário -->
        </select>
      </div>
      <div class="form-group">
        <label for="value"><strong>Valor:</strong></label>
        <input type="text" class="form-control" id="value" v-model.lazy="newfilteredEvent.value" @input="formatCurrency" placeholder="Digite aqui" required autocomplete="off">
      </div>
      <div class="form-group">
        <label for="date"><strong>Data:</strong></label>
        <input type="date" class="form-control" id="date" v-model="newfilteredEvent.date" required autocomplete="off">
      </div>
      <div style="margin: auto;">
        <label for="phone"><strong>Telefone:</strong></label>
        <input type="text" id="phone" v-model="newfilteredEvent.phoneNumber" placeholder="(DDD) _____-____" @input="formatPhoneNumber" required autocomplete="off">
      </div>
      <div class="button-container">
        <button type="submit" class="btn btn-success" @click="validateFields" >
          Agendar
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import numeral from 'numeral';

export default {
  name: 'AddEvent',
  data() {
    return {
      newfilteredEvent: {
        title: '',
        date: '',
        value: '',
        phoneNumber: ''
      }
    };
  },
  methods: {
    addEvent() {
  if (
    this.newfilteredEvent.title &&
    this.newfilteredEvent.date &&
    this.newfilteredEvent.value &&
    this.newfilteredEvent.phoneNumber
  ) {
    this.$emit('eventAdded', this.newfilteredEvent);
    this.clearFields();
  } 
},

    formatCurrency(event) {
      let input = event.target.value;
      const apenasDigitos = input.replace(/\D/g, '');
      const valorNumerico = Number(apenasDigitos) / 100;
      const valorFormatado = numeral(valorNumerico).format('0,0.00');
      event.target.value = valorFormatado;
      this.newfilteredEvent.value = numeral(this.newfilteredEvent.value).format('0,0.00');
    },
    clearFields() {
      this.newfilteredEvent = {
        title: '',
        date: '',
        value: '',
        phoneNumber: ''
      };
    },
    formatPhoneNumber() {
      let cleaned = this.newfilteredEvent.phoneNumber.replace(/\D/g, '');
      let formattedNumber = '';

      if (cleaned.length > 0) {
        formattedNumber = `(${cleaned.slice(0, 2)})`;

        if (cleaned.length > 2 && cleaned.length <= 7) {
          formattedNumber += ` ${cleaned.slice(2)}`;
        } else if (cleaned.length > 7 && cleaned.length <= 11) {
          formattedNumber += ` ${cleaned.slice(2, 7)}-${cleaned.slice(7)}`;
        } else if (cleaned.length > 11) {
          formattedNumber += ` ${cleaned.slice(2, 7)}-${cleaned.slice(7, 11)}`;
        }
      }

      this.newfilteredEvent.phoneNumber = formattedNumber;
    },
    validateFields(event) {
      event.preventDefault();

      if (
        this.newfilteredEvent.title &&
        this.newfilteredEvent.date &&
        this.newfilteredEvent.value &&
        this.newfilteredEvent.phoneNumber
      ) {
        this.addEvent();
        alert('Agendamento realizado com sucesso!')
      } else {
        alert('Por favor, preencha todos os campos obrigatórios!');
      }
     
    }
  }
};
</script>

  
  <style scoped>
  /* Importa a fonte Montserrat do Google Fonts com diferentes pesos */
  @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500;600;700&display=swap');
  @import '~bootstrap/dist/css/bootstrap.css';
  @import url('https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css');
  
  /* Estilos para o contêiner principal */
  .container {
    background-color: #f9f9f9;
    border-radius: 10px;
    padding: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    align-items: center;
    width: 90%;
    max-width: 600px;
    margin: 20px auto; /*margin: 0px auto;*/
  }
  
  /* Estilos para o título h1 */
  h1 {
    font-family: 'Montserrat', sans-serif;
    color: #333;
    text-align: center;
    margin-bottom: 20px;
    margin: 0;
  }
  
  /* Estilos para o formulário (.form-container) */
  .form-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  /* Estilos para os grupos de formulário (.form-group) */
  .form-group {
    flex: 1 1 50%; 
    padding: 5px;
    box-sizing: border-box;
    max-width: 300px;
  }
  
  /* Estilos para o contêiner do botão (.button-container) */
  .button-container {
    margin: 15px;
    text-align: center;
    width: 100%; /* Garante que o botão ocupe toda a largura */
  }
  
  /* Ajustes de estilo (opcional) para labels, inputs e selects */
  label {
    display: block;
    margin-bottom: 5px;
    text-align: center;
  }
  
  input[type="text"],
  select,
  input[type="date"] {
    width: 100%;
    padding: 8px;
    border-radius: 5px;
    border: 1px solid #ccc;
    box-sizing: border-box;
    margin-top: 3px;
  }
  
  .btn {
    padding: 8px 20px;
    border-radius: 5px;
    cursor: pointer;
    display: block;
    margin: 0 auto;
  }

  .btn-success {
    background-color: #4caf50;
    color: white;
    border: none;
  }

  
  </style>
  