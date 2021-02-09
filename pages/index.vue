<template>
  <section>
    <div :style="{ color: 'blue', fontSize: '30px' }">
      <h1>¡Bienvenid@ al sistema virtual de reservas!</h1>
    </div>
    <b-tabs>
      <b-tab-item label="Reservar">
        <reservation
          :books="books"
          :clients="clients"
          :reservations="reservations"
          @create="saveReservation"
        ></reservation>
        <reservation-data :reservations="reservations"></reservation-data>
      </b-tab-item>
      <b-tab-item label="Ingresar un nuevo libro">
        <book @create="saveBook"></book>
        <back></back>
      </b-tab-item>
      <b-tab-item label="Crear un nuevo cliente">
        <client @create="saveNewClient"></client>
      </b-tab-item>
    </b-tabs>
  </section>
</template>

<script>
import book from '@/components/book.vue'
import reservation from '@/components/reservation.vue'
import client from '@/components/client.vue'
import ReservationData from '@/components/reservationData.vue'
import back from '@/components/back.vue'
export default {
  components: { reservation, book, client, ReservationData ,back },
  data() {
    return {
      clients: [
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
      ],
      reservations: [],
      books: [
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
      ],
    }
  },
  methods: {
    saveReservation(reservation) {
      this.reservations.push(reservation)
    },
    saveBook(book) {
      this.books.push(book)
    },
    saveNewClient(client) {
      this.clients.push(client)
    },
  },
}
</script>
