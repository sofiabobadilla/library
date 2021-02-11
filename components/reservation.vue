<template>
  <section>
    <form action="" @submit="saveReservations">
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
            <b-select v-model="booksList" multiple icon="book" required>
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
    book: null,
    email: '',
    client: null,
  }

  booksList: [] = []

  calculateAge(birthday: Date) {
    // Debe estar en formato YYYY-MM-DD
    const age = new Date().getFullYear() - birthday.getFullYear()
    const m = new Date().getMonth() - birthday.getMonth()
    if (m < 0 || (m === 0 && new Date().getDate() < birthday.getDate())) {
      return age - 1
    }
    return age
  }

  saveReservations(event: Event) {
    event.preventDefault()
    this.booksList.forEach((element: book) => this.saveReservation(element))
  }

  saveReservation(book: any) {
    const clientAge = this.calculateAge(this.currentReservation.client.birth)
    this.checkOtherReservations()
    if (clientAge < 18 && book.explicit === true) {
      alert(
        'El libro ' +
          book.title +
          ' solo lo puede reservar una persona mayor de edad'
      )
    } else if (book.stock < 1) {
      alert('El libro ' + book.title + ' se encuentra sin stock')
    } else if (this.currentReservation.client.canReserve === false) {
      alert('No puedes reservar porque tienes libros pendientes a entregar')
    } else {
      this.currentReservation.book = book
      this.pushReservation()
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
      book: this.currentReservation.book,
      email: this.currentReservation.client.email,
      client: this.currentReservation.client,
    }
    this.currentReservation.book.stock--

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
