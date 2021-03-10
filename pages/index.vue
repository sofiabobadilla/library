<template>
  <section>
    <div
      :style="{ color: 'black', fontSize: '30px', backgroundColor: ' #faa4c3' }"
    >
      <h1>¡Bienvenid@ al sistema virtual de reservas!</h1>
    </div>
    <p></p>

    <b-field grouped>
      <back></back>
    </b-field>
    <p
      :style="{ color: 'purple', fontSize: '20px', backgroundColor: '#e0f1ef' }"
    >
      Selecciona una pestaña para realizar la acción correspondiente
    </p>
    <b-tabs>
      <b-tab-item label="Reservar">
          <manyReservations
            :books="books"
            :clients="clients"
            :reservations="reservations"
            @create="saveReservation"
          ></manyReservations>
        <reservation-data :reservations="reservations"></reservation-data>
      </b-tab-item>
      <b-tab-item label="Ingresar un nuevo libro">
        <book @create="saveBook"></book>
      </b-tab-item>
      <b-tab-item label="Crear un nuevo cliente">
        <client @create="saveNewClient"></client>
      </b-tab-item>
    </b-tabs>
  </section>
</template>

<script lang="ts">
import reservation from '@/components/reservation.vue'
import book from '@/components/book.vue'
import client from '@/components/client.vue'
import reservationData from '@/components/reservationData.vue'
import back from '@/components/back.vue'
import manyReservations from '@/components/manyReservations.vue'
import { Vue, Component } from 'vue-property-decorator'
@Component({
  components: {
    reservation,
    book,
    client,
    reservationData,
    back,
    manyReservations,
  },
})
export default class Index extends Vue {
  stops: [] = []
  clients: { [key: string]: any } = [
    {
      name: 'Sofía Bobadilla Ponce',
      birth: new Date(2019, 9, 14),
      email: 'sofia@gmail.com',
      canReserve: true,
    },
    {
      name: 'Dora La Exploradora',
      birth: new Date(1989, 9, 14),
      email: 'zorronotelolleves@gmail.com',
      canReserve: true,
    },
    {
      name: 'Billy',
      birth: new Date(2019, 9, 14),
      email: 'billy@gmail.com',
      canReserve: true,
    },
    {
      name: 'Agnes Bobadilla Reyes',
      birth: new Date(2020, 11, 20),
      email: 'agnolia@gmail.com',
      canReserve: true,
    },
  ]

  reservations: { [key: string]: any } = []
  books: { [key: string]: any } = [
    {
      title: 'Sapiens',
      author: 'Yuval Noah Harari',
      publicationDate: new Date(2011, 0, 1),
      stock: 10,
      explicit: false,
    },
    {
      title: 'Orgullo y Prejuicio',
      author: 'Jane Austen',
      publicationDate: new Date(1813, 0, 28),
      stock: 100,
      explicit: false,
    },
    {
      title: 'La bailarina de Auschwitz',
      author: 'Edith Eger',
      publicationDate: new Date(2017, 8, 5),
      stock: 10,
      explicit: true,
    },
    {
      title: 'El hombre que calculaba',
      author: 'Malba Tahan',
      publicationDate: new Date(2007, 2, 1),
      stock: 100,
      explicit: false,
    },
    {
      title: 'De Animales  Dioses',
      author: 'Yuval Noah Harari',
      publicationDate: new Date(2014, 0, 28),
      stock: 100,
      explicit: false,
    },
  ]

  saveReservation(reservation: Array<reservation>) {
    this.reservations.push(reservation)
  }

  saveBook(book: Array<book>) {
    this.books.push(book)
  }

  saveNewClient(client: Array<client>) {
    this.clients.push(client)
  }

}
</script>
