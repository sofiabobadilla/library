<template>
  <section>
    <b-table :data="reservations">
      <b-table-column
        v-slot="reservations"
        field="client.name"
        label="Nombre del cliente"
      >
        {{ reservations.row.client.name }}
      </b-table-column>
      <b-table-column v-slot="reservations" field="book" label="Libro">
        {{ reservations.row.book.title}}
      </b-table-column>
      <b-table-column
        v-slot="reservations"
        field="email"
        label="Correo de Contacto"
      >
        {{ reservations.row.email }}
      </b-table-column>
      <b-table-column
        v-slot="reservations"
        field="deliver"
        label="Fecha de Entrega"
      >
        {{ reservations.row.deliver }}
      </b-table-column>
      <b-table-column
        v-slot="reservations"
        field="return"
        label="Fecha de Retorno"
      >
        {{ reservations.row.return }}
      </b-table-column>
      <b-table-column v-slot="reservations" field="actions" label="Acciones">
        <b-button type="is-danger" @click="DeleteReservation(reservations.row)"
          >Eliminar Reserva</b-button
        >
      </b-table-column>
    </b-table>
  </section>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'
import reservation from './reservation.vue'
@Component({ components: { reservation } })
export default class reservationData extends Vue {
  @Prop({ required: true })
  reservations!: Array<reservation>

  columns: {} = [
    {
      field: 'deliver',
      label: 'deliver',
    },
    {
      field: 'return',
      label: 'return',
    },
    { field: 'book.title', label: 'book reserved' },
    {
      field: 'email',
      label: 'email',
    },
    { field: 'client.name', label: 'client' },
    { field: 'actions', label: 'actions' },
  ]


  DeleteReservation(row: any) {
    row.book.stock++
    const index = this.reservations.indexOf(row)
    this.reservations.splice(index, 1)
  }
}
</script>
