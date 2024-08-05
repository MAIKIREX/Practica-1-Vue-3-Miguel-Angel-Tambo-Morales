<template>
  <div>
    <h1>{{ message }}</h1>
    <div class="filter-container">
      <div class="filter-input">
        <h2>Filter</h2>
        <input type="text" v-model="filterQuery" placeholder="Filter by name or email">
      </div>
    </div>
    <table class="styled-table">
      <thead>
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Email</th>
          <th>Address</th>
          <th>Phone</th>
          <th>Country</th>
          <th>City</th>
          <th></th>
        </tr>
        <tr>
          <th><input type="text" v-model="contacNewObj.id"></th>
          <th><input type="text" v-model="contacNewObj.name"></th>
          <th><input type="text" v-model="contacNewObj.email"></th>
          <th><input type="text" v-model="contacNewObj.address"></th>
          <th><input type="text" v-model="contacNewObj.phone"></th>
          <th><input type="text" v-model="contacNewObj.country"></th>
          <th><input type="text" v-model="contacNewObj.city"></th>
          <th><button @click="saveNew()">New</button></th>
        </tr>
        <tr v-if="indexToEdit !== null">
          <th><input type="text" v-model="contactEditObject.id"></th>
          <th><input type="text" v-model="contactEditObject.name"></th>
          <th><input type="text" v-model="contactEditObject.email"></th>
          <th><input type="text" v-model="contactEditObject.address"></th>
          <th><input type="text" v-model="contactEditObject.phone"></th>
          <th><input type="text" v-model="contactEditObject.country"></th>
          <th><input type="text" v-model="contactEditObject.city"></th>
          <th><button @click="update()">Save</button></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(contact, index) in filteredContacts" :key="contact.id">
          <td>{{contact.id}}</td>
          <td>{{contact.name}}</td>
          <td>{{contact.email}}</td>
          <td>{{contact.address}}</td>
          <td>{{contact.phone}}</td>
          <td>{{contact.country}}</td>
          <td>{{contact.city}}</td>
          <td>
            <button @click="delateContac(index)">Delete</button>
            <button @click="editContact(contact, index)">Edit</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "MiComponente",
  data() {
    return {
      message: "Agenda de contactos",
      filterQuery: "",
      indexToEdit: null,
      contactEditObject: {},
      contacNewObj: {
          id: "",
          name: "",
          email: "",
          address: "",
          phone: "",
          country: "",
          city: "",
      },
      contactos: [
        {
          id: 1,
          name: "Alice Johnson",
          email: "alice. johnson@example.com",
          address: "123 Maple Street",
          phone: "123-456-7890",
          country: "USA",
          city: "New York",
        },
        {
          id: 2,
          name: "Bob Smith",
          email: "bob.smith@example.com",
          address: "456 Oak Avenue",
          phone: "987-654-3210",
          country: "Canada",
          city: "Toronto",
        },
        {
          id: 3,
          name: "Carol White",
          email: "carol.white@example.com",
          address: "789 Pine Road",
          phone: "555-123-4567",
          country: "UK",
          city: "London",
        },
        {
          id: 4,
          name: "David Brown",
          email: "david.brown@example.com",
          address: "321 Elm Street",
          phone: "444-555-6666",
          country: "Australia",
          city: "Sydney",
        },
        {
          id: 5,
          name: "Emily Davis",
          email: "emily. davis@example.com",
          address: "654 Spruce Lane",
          phone: "333-444-5555",
          country: "USA",
          city: "Los Angeles",
        },
      ],
    };
  },
  components: {
    // Registro de componentes que se utilizaran.
  },
  methods: {
    saveNew(){
      this.contactos.push(Object.assign({}, this.contacNewObj))
    },
    delateContac(index){
      this.contactos.splice(index,1)
    },
    update(){
      this.contactos[this.indexToEdit] = Object.assign({}, this.contactEditObject)
    },
    editContact(contact, index){
      this.indexToEdit = index
      this.contactEditObject = Object.assign({},contact)
    }
    // métodos que se pueden llamar desde la plantilla o desde otras partes del componente.
  },
  computed: {
    filteredContacts() {
    if (!this.filterQuery) {
      // Si el filtro está vacío, retorna todos los contactos
      return this.contactos;
    }
    // Filtra contactos según el filtro aplicado
    return this.contactos.filter(contact => {
      const lowerCaseQuery = this.filterQuery.toLowerCase();
      return (
        contact.name.toLowerCase().includes(lowerCaseQuery) ||
        contact.email.toLowerCase().includes(lowerCaseQuery)
      );
    });
  },
    // propiedades computadas que dependen de otras propiedades reactivas
  },
  props: {
    // propiedades que el componente puede recibir.
  },
  emits: [], // los eventos personalizados que el componente puede emitir.
};
</script>

<style>
h1 {
  color: #42b983;
}

.filter-container {
  margin-right: 20px;
}

.filter-input {
  display: flex;
  align-items: center;
}

.filter-input h2 {
  margin: 0;
  margin-right: 10px;
}

.styled-table {
  width: 100%;
  border-collapse: collapse;
  margin: 25px 0;
  font-size: 0.9em;
  font-family: 'Arial', sans-serif;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}

.styled-table thead tr {
  background-color: #009879;
  color: #ffffff;
  text-align: left;
  font-weight: bold;
}

.styled-table th, .styled-table td {
  padding: 12px 15px;
}

.styled-table tbody tr {
  border-bottom: 1px solid #dddddd;
}

.styled-table tbody tr:nth-of-type(even) {
  background-color: #f3f3f3;
}

.styled-table tbody tr:last-of-type {
  border-bottom: 2px solid #009879;
}

.styled-table tbody tr:hover {
  background-color: #f5f5f5;
  cursor: pointer;
  transform: scale(1.01);
  transition: all 0.2s ease-in-out;
}

.styled-table tbody td {
  color: #333333;
}

.styled-table tbody td.actions {
  text-align: center;
}

.styled-table tbody td.actions button {
  background-color: #009879;
  color: #ffffff;
  border: none;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
}

.styled-table tbody td.actions button:hover {
  background-color: #00795a;
}
</style>