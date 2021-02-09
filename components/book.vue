<template>
  <div>
    <form action="" @submit="saveBook">
      <b-field grouped>
        <b-field label="Título">
          <b-input v-model="currentBook.title" icon="text" required></b-input>
        </b-field>
        <b-field label="Autor">
          <b-input v-model="currentBook.author" icon="text" required></b-input>
        </b-field>

        <b-field label="Fecha de Publicación">
          <b-datepicker
            v-model="currentBook.publicationDate"
            placeholder="Type or select the publication date"
            icon="calendar-today"
            editable
            required
          >
          </b-datepicker>
        </b-field>

        <b-field label="Stock">
          <b-numberinput v-model="currentBook.stock" required></b-numberinput>
        </b-field>
      </b-field>
      <b-field label="¿Es contenido para mayores de edad?">
        <b-checkbox v-model="currentBook.explicit"> YES </b-checkbox>
      </b-field>
      <b-button type="is-info" native-type="submit">Save</b-button>
    </form>
  </div>
</template>

<script>
import { Vue, Component, Emit } from 'vue-property-decorator'
@Component
export default class Client extends Vue {
  data() {
    return {
      currentBook: {
        title: '',
        author: '',
        publicationDate: new Date(),
        stock: 10,
        explicit: true,
      },
    }
  }

  @Emit('create')
  saveBook(event) {
    event.preventDefault()
    const newBook = {
      title: this.currentBook.title,
      author: this.currentBook.author,
      publicationDate: this.currentBook.publicationDate,
      stock: this.currentBook.stock,
      explicit: this.currentBook.explicit,
    }
    this.currentBook = {
      title: '',
      author: '',
      publicationDate: null,
      stock: 10,
      explicit: false,
    }
    return newBook
  }
}
</script>

<style></style>
