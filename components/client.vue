<template>
  <section>
    <form action="" @submit="saveNewClient">
      <b-field label="Escribe tu nombre completo">
        <b-input v-model="currentClient.name" icon="text" required></b-input>
      </b-field>

      <b-field label="Correo de contacto">
        <b-input
          v-model="currentClient.email"
          icon="email"
          type="email"
          maxlength="30"
          required
        >
        </b-input>
      </b-field>

      <b-field label="Año de Nacimiento">
        <b-datepicker
          v-model="currentClient.birth"
          icon="calendar-today"
          editable
          required
        >
        </b-datepicker>
      </b-field>
      <b-button type="is-info" native-type="submit">Save</b-button>
    </form>
  </section>
</template>

<script lang="ts">
import { Vue, Component, Emit } from 'vue-property-decorator'
@Component
export default class Client extends Vue {
  currentClient: { [key: string]: any } = {
    name: '',
    birth: new Date(),
    email: '',
    canReserve: true,
  }

  @Emit('create')
  saveNewClient(event: Event) {
    event.preventDefault()
    const newClient = {
      name: this.currentClient.name,
      birth: this.currentClient.birth,
      email: this.currentClient.email,
      canReserve: this.currentClient.canReserve,
    }
    this.currentClient = {
      name: '',
      birth: new Date(),
      email: '',
      canReserve: true,
    }
    return newClient
  }
}
</script>

<style></style>
