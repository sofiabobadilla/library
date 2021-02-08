<template>
  <section>
    <b-table :data="reservations">
      <b-table-column
        v-slot="props"
        field="client.name"
        label="Nombre del cliente"
      >
        {{ props.row.client.name }}
      </b-table-column>
      <b-table-column v-slot="props" field="book" label="Libro">
        {{ props.row.book.title }}
      </b-table-column>
      <b-table-column v-slot="props" field="email" label="Correo de Contacto">
        {{ props.row.email }}
      </b-table-column>
      <b-table-column v-slot="props" field="deliver" label="Fecha de Entrega">
        {{ props.row.deliver }}
      </b-table-column>
      <b-table-column v-slot="props" field="return" label="Fecha de Retorno">
        {{ props.row.return }}
      </b-table-column>

      <b-table-column v-slot="props" field="actions" label="Acciones">
        <b-button type="is-danger" @click="DeleteReservation(props.row)"
          >Eliminar Reserva</b-button
        >
      </b-table-column>
    </b-table>
  </section>
</template>

<script>
export default {
  props: {
    reservations: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      columns: [
        {
          field: 'deliver',
          label: 'deliver',
        },
        {
          field: 'return',
          label: 'return',
        },
        { field: 'bookTitle', label: 'book reserved' },
        {
          field: 'email',
          label: 'email',
        },
        { field: 'client.name', label: 'client' },
        { field: 'actions', label: 'actions' },
      ],
    }
  },
  methods: {
    DeleteReservation(row) {
      row.book.stock++
      const index = this.reservations.indexOf(row)
      this.reservations.splice(index, 1)
    },
  },
}
</script>
