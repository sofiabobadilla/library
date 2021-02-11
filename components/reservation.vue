<template>
  <section>
    <form action="" @submit="saveReservation">
      <div>
        <b-field grouped>
          <b-field label="Selecciona al cliente ">
            <b-select
              v-model="currentReservation.client"
              icon="account"
              required
            >
              <option
                v-for="(item, index) in clients"
                :key="index"
                :value="item"
              >
                {{ item.name }}
              </option>
            </b-select>
          </b-field>
          <b-field label="Selecciona un libro de la lista">
            <b-select
              v-model="currentReservation.booksList"
              multiple
              icon="book"
              required
            >
              <option v-for="(item, index) in books" :key="index" :value="item">
                {{ item.title }}
              </option>
            </b-select>
          </b-field>
          <b-field label="Selecciona las fechas a reservar">
            <b-datepicker
              v-model="currentReservation.dates"
              range
              icon="calendar"
              required
            >
            </b-datepicker>
            <p>{{ currentReservation.book }}</p>
          </b-field>
        </b-field>
        <b-button type="is-info" native-type="submit">Guardar Reserva</b-button>
      </div>
    </form>
  </section>
</template>

<script lang="ts">
// Define the props by using Vue's canonical way.
import { Vue, Component, Emit, Prop } from 'vue-property-decorator'
import book from '@/components/book.vue'
import client from '@/components/client.vue'
@Component({ components: { book, client } })
export default class Reservation extends Vue {
  @Prop({ required: true }) books
  @Prop({ required: true }) clients
  @Prop({ required: true }) reservations
  currentReservation: { [key: string]: any } = {
    dates: [],
    booksList: [],
    email: '',
    client: null,
  }

  calculateAge(birthday: Date) {
    // Debe estar en formato YYYY-MM-DD
    const age = new Date().getFullYear() - birthday.getFullYear()
    const m = new Date().getMonth() - birthday.getMonth()
    if (m < 0 || (m === 0 && new Date().getDate() < birthday.getDate())) {
      return age - 1
    }
    return age
  }

  saveReservation(event: Event) {
    event.preventDefault()
    const clientAge = this.calculateAge(this.currentReservation.client.birth)
    this.checkOtherReservations()
    const booksExplicity = this.currentReservation.booksList.forEach(
      (element: book) => {
        this.checkIfExplicit(element)
      }
    )
    const booksStock = this.currentReservation.booksList.forEach(
      (element: book) => {
        this.checkStock(element)
      }
    )
    if (clientAge < 18 && booksExplicity === true) {
      alert('No puedes reservar este libro por ser menor de edad')
    } else if (booksStock) {
      alert('Libro sin stock')
    } else if (this.currentReservation.client.canReserve === false) {
      alert('No puedes reservar porque tienes libros pendientes a entregar')
    } else {
      this.pushReservation()
    }
  }

  checkStock(book: book) {
    if (book.stock < 1) {
      alert('El libro ' + book.title + ' se encuentra sin stock')
      return false
    }
  }

  checkIfExplicit(book: book) {
    if (book.explicit === true) {
      alert('El libro ' + book.title + ' solo lo puede reservar una persona mayor de edad')
      return true
    }
  }

  @Emit('create')
  pushReservation() {
    const newReservation = {
      deliver: Intl.DateTimeFormat(undefined, { timeZone: 'UTC' }).format(
        this.currentReservation.dates[0]
      ),
      return: Intl.DateTimeFormat(undefined, { timeZone: 'UTC' }).format(
        this.currentReservation.dates[1]
      ),
      booksList: this.currentReservation.booksList,
      email: this.currentReservation.client.email,
      client: this.currentReservation.client,
    }
    this.currentReservation.booksList.forEach((element: book) => {
      element.stock--
    })
    return newReservation
  }

  checkOtherReservations() {
    this.currentReservation.client.canReserve = true
    this.reservations.forEach(this.Reserve)
  }

  Reserve(item: any) {
    const today = Intl.DateTimeFormat(undefined, { timeZone: 'UTC' }).format(
      new Date()
    )
    if (
      item.client.name === this.currentReservation.client.name &&
      item.return < today
    ) {
      this.currentReservation.client.canReserve = false
    }
  }
}
</script>

<style></style>
