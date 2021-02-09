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
            <b-select v-model="currentReservation.book" icon="book" required>
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

<script>
// Define the props by using Vue's canonical way.
import { Vue, Component, Emit, Prop } from 'vue-property-decorator'
@Component
export default class Reservation extends Vue {
  @Prop({ default: '() => []' }) books: Array
  @Prop({ default: '() => []' }) clients: Array
  @Prop({ default: '() => []' }) reservations: Array
  data() {
    return {
      currentReservation: {
        dates: [],
        book: null,
        email: '',
        client: null,
      },
    }
  }

  calculateAge(birthday) {
    // Debe estar en formato YYYY-MM-DD
    const age = new Date().getFullYear() - birthday.getFullYear()
    const m = new Date().getMonth() - birthday.getMonth()
    if (m < 0 || (m === 0 && new Date().getDate() < birthday.getDate())) {
      return age - 1
    }
    return age
  }

  @Emit('create')
  saveReservation(event) {
    event.preventDefault()
    const clientAge = this.calculateAge(this.currentReservation.client.birth)
    this.checkedOtherReservations()
    if (clientAge < 18 && this.currentReservation.book.explicit === true) {
      this.$buefy.notification.open(
        'No puedes reservar este libro por ser menor de edad'
      )
    } else if (this.currentReservation.book.stock < 1) {
      this.$buefy.notification.open('Libro sin stock')
    } else if (this.currentReservation.client.canReserve === false) {
      this.$buefy.notification.open(
        'No puedes reservar porque tienes libros pendientes a entregar'
      )
    } else {
      const newReservation = {
        deliver: Intl.DateTimeFormat(undefined, { timezome: 'UTC' }).format(
          this.currentReservation.dates[0]
        ),
        return: Intl.DateTimeFormat(undefined, { timezome: 'UTC' }).format(
          this.currentReservation.dates[1]
        ),
        book: this.currentReservation.book,
        email: this.currentReservation.client.email,
        client: this.currentReservation.client,
      }
      this.currentReservation.book.stock--
      this.currentReservation = {
        deliver: new Date(),
        return: new Date(),
        book: null,
        email: '',
        client: null,
      }
      this.$buefy.notification.open('La reserva se realizó correctamente')
      return newReservation
    }
  }

  checkedOtherReservations() {
    this.currentReservation.client.canReserve = true
    this.reservations.forEach(this.Reserve)
    // this.currentReservation.client.canReserve=false
  }

  Reserve(item) {
    const today = Intl.DateTimeFormat(undefined, { timezome: 'UTC' }).format(
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
