<template>
  <section>
    <div>
      <b-field grouped>
        <b-field label="Selecciona al cliente ">
          <b-select v-model="current.client" icon="account" required>
            <option v-for="(item, index) in clients" :key="index" :value="item">
              {{ item.name }}
            </option>
          </b-select>
        </b-field>
        <b-field label="Selecciona los libros que quieras de la lista">
          <div v-for="(book, index) in current.books" :key="index">
            <b-select v-model="current.books[index]" icon="book" required>
              <option v-for="(item, i) in books" :key="i" :value="item">
                {{ item.title }}
              </option>
            </b-select>
          </div>
          <b-button type="is-text" size="is-small" @click="addBook"
            >Añadir</b-button
          >
        </b-field>
        <b-field label="Selecciona las fechas a reservar">
          <b-datepicker v-model="current.dates" range icon="calendar" required>
          </b-datepicker>
        </b-field>
      </b-field>
      <b-button type="is-info" @click="remove">Eliminar Reserva</b-button>
    </div>
  </section>
</template>

<script lang="ts">
// Define the props by using Vue's canonical way.
import { Vue, Component, Prop, Emit } from 'vue-property-decorator'
import book from '@/components/book.vue'
import client from '@/components/client.vue'
@Component({ components: { book, client } })
export default class Reservation extends Vue {
  @Prop({ required: true }) current
  @Prop({ required: true }) books
  @Prop({ required: true }) clients

  addBook() {
    this.current.books.push(null)
  }

  @Emit('remove')
  remove() {}
}
</script>

<style></style>
