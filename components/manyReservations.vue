<template>
  <section>
    <form action="" @submit="saveReservations">
      <reservation
        v-for="(item, index) in reservationItems"
        :key="index"
        :books="books"
        :clients="clients"
        :current="item"
        @remove="removeItem(index)"
      ></reservation>
      <b-button type="is-info" @click="Add"> agregar otra reserva</b-button>
      <b-button type="is-info" native-type="submit">Guardar Reserva</b-button>
    </form>
  </section>
</template>

<script lang="ts">
// Define the props by using Vue's canonical way.
import { Vue, Component, Emit, Prop } from 'vue-property-decorator'
import reservation from '@/components/reservation.vue'
import book from '@/components/book.vue'
import client from '@/components/client.vue'
@Component({ components: { reservation, book, client } })
export default class ManyReservations extends Vue {
  @Prop({ required: true }) books
  @Prop({ required: true }) clients
  @Prop({ required: true }) reservations
  booksList: [] = []
  reservationItems: [] = []
  current: { [key: string]: any } = {
    dates: [],
    booksList: [],
    email: '',
    client: null,
  }

  Add() {
    this.reservationItems.push({
      dates: [],
      books: [],
      email: '',
      client: null,
    })
  }

  removeItem(index: number) {
    this.reservationItems.splice(index, 1)
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

  saveReservations(event: Event) {
    event.preventDefault()
    this.reservationItems.forEach((element: book) =>
      this.saveReservationPerBook(element)
    )
  }

  saveReservationPerBook(reserva: any) {
    reserva.books.forEach((element: book) =>
      this.saveReservation(element, reserva)
    )
  }

  saveReservation(book: any, reserva: any) {
    const clientAge = this.calculateAge(reserva.client.birth)
    this.checkOtherReservations(reserva)

    if (clientAge < 18 && book.explicit === true) {
      alert(
        'El libro ' +
          book.title +
          ' solo lo puede reservar una persona mayor de edad'
      )
    } else if (book.stock < 1) {
      alert('El libro ' + book.title + ' se encuentra sin stock')
    } else if (reserva.client.canReserve === false) {
      alert('No puedes reservar porque tienes libros pendientes a entregar')
    } else {
      this.pushReservation(book, reserva)
    }
  }

  @Emit('create')
  pushReservation(bookIntheList: book, reserva: reservation) {
    const newReservation = {
      book: bookIntheList,
      email: reserva.client.email,
      client: reserva.client,
      deliver: Intl.DateTimeFormat(undefined, { timeZone: 'UTC' }).format(
        reserva.dates[0]
      ),
      return: Intl.DateTimeFormat(undefined, { timeZone: 'UTC' }).format(
        reserva.dates[1]
      ),
    }
    bookIntheList.stock--

    return newReservation
  }

  checkOtherReservations(reserva: any) {
    reserva.client.canReserve = true
    this.reservations.forEach((element) => this.Reserve(element, reserva))
  }

  Reserve(item: any, reserva: reservation) {
    const today = Intl.DateTimeFormat(undefined, { timeZone: 'UTC' }).format(
      new Date()
    )
    if (item.client.name === reserva.client.name && item.return < today) {
      reserva.client.canReserve = false
    }
  }
}
</script>

<style></style>
