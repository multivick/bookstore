<template>
  <div class="row" id="app">
    <div class="col-md-12">
      <div class="row mt-5 mb-5">
        <!--book add form-->
        <div class="col-md-4" v-if="isAdding">
          <div class="card">
            <div class="card-header bg-success text-white">
              Add Book
            </div>
            <div class="card-body">
              <form v-on:submit.prevent="addBookSubmit()">
                <div class="form-group">
                  <label for="title">Title</label>
                  <input v-model="newBook.title" type="text" class="form-control" id="title" name="title" placeholder="Enter title" required>
                </div>
                <div class="form-group">
                  <label for="author">Author</label>
                  <input v-model="newBook.author" type="text" class="form-control" id="author" name="author" placeholder="Enter author" required>
                </div>
                <div class="form-group">
                  <label for="website">Website</label>
                  <input v-model="newBook.website" type="url" class="form-control" id="website" name="website" placeholder="Enter website" required>
                </div>
                <button type="reset" class="btn btn-danger">Reset</button>
                <button type="submit" class="btn btn-success">Add Book</button>
              </form>
            </div>
          </div>
        </div>
        <!--book edit form-->
        <div class="col-md-4" v-if="isEditing">
          <div class="card">
            <div class="card-header bg-primary text-white">
              Edit Book
            </div>
            <div class="card-body">
              <form v-on:submit.prevent="editBookSubmit()">
                <div class="form-group">
                  <label for="edit_title">Title</label>
                  <input v-model="editBook.title" type="text" class="form-control" id="edit_title" name="edit_title" placeholder="Enter title" required>
                </div>
                <div class="form-group">
                  <label for="edit_author">Author</label>
                  <input v-model="editBook.author" type="text" class="form-control" id="edit_author" name="edit_author" placeholder="Enter author" required>
                </div>
                <div class="form-group">
                  <label for="edit_website">Website</label>
                  <input v-model="editBook.website" type="url" class="form-control" id="edit_website" name="edit_website" placeholder="Enter website" required>
                </div>
                <button v-on:click="addBookForm()" type="button" class="btn btn-danger">Cancel</button>
                <button type="submit" class="btn btn-primary">Update Book</button>
              </form>
            </div>
          </div>
        </div>
        <!--book list table-->
        <div class="col-md-8">
          <div class="card">
            <div class="card-header">
              Book List
            </div>
            <div class="card-body">
              <table class="table table-striped">
                <thead>
                <tr>
                  <th>#</th>
                  <th>Name</th>
                  <th>Author</th>
                  <th>Website</th>
                  <th>Action</th>
                </tr>
                </thead>
                <tbody>
                <tr v-for="(book, index) in books" v-if="books.length !== 0">
                  <td>
                    {{ index + 1 }}
                  </td>
                  <td>
                    {{ book.title}}
                  </td>
                  <td>
                    {{ book.author }}
                  </td>
                  <td>
                    <a :href="book.website"  target="_blank">{{ book.website }}</a>
                  </td>
                  <td>
                    <a v-on:click="editBookForm(book)" href="javascript:void(0)" type="button" class="btn btn-xs btn-primary">
                      <i class="fa fa-pencil"></i>
                    </a>
                    <a v-on:click="deleteBook(book)" href="javascript:void(0)" type="button" class="btn btn-xs btn-danger">
                      <i class="fa fa-trash"></i>
                    </a>
                  </td>
                </tr>
                <tr class="text-center" v-if="books.length === 0">
                  <td colspan="5">Nothing found</td>
                </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

// import firebase
import FireBase from 'firebase'

// import toastr
import toastr from 'toastr'

// firebase configuration
let config = {
  apiKey: "AIzaSyBFz3hiFIVBJ1oXGUyJdC9Y3ItlUMKIgWI",
  authDomain: "book-store-62cb3.firebaseapp.com",
  databaseURL: "https://book-store-62cb3.firebaseio.com",
  projectId: "book-store-62cb3",
  storageBucket: "book-store-62cb3.appspot.com",
  messagingSenderId: "252200862531"
}

// initializing firebase
let app = FireBase.initializeApp(config)
let db = app.database()
let booksRef = db.ref('books')


export default {
  name: 'App',
  data () {
    return {
      newBook: {
        title: '',
        author: '',
        website: '',
      },
      editBook: {},
      isAdding: true,
      isEditing: false
    }
  },
  firebase: {
    books: booksRef
  },
  methods: {
    // edit book form
    addBookForm: function (book) {
      this.addBook = {}
      this.editBook = {}
      this.isAdding = true
      this.isEditing = false
    },
    // adding book
    addBookSubmit: function () {
      booksRef.push(this.newBook)
      toastr.success('Book Added Successfully')
      this.newBook = {}
    },
    // edit book form
    editBookForm: function (book) {
      this.editBook = book
      this.isAdding = false
      this.isEditing = true
    },
    // editing book
    editBookSubmit: function () {
      let book = this.editBook
      booksRef.child(book['.key']).update({
        title: book.title,
        author: book.author,
        website: book.website,
      })
      toastr.success('Book Updated Successfully')
      this.addBook = {}
    },
    // deleting book
    deleteBook: function (book) {
      booksRef.child(book['.key']).remove()
      toastr.success('Book Deleted Successfully')
    }
  }
}
</script>

<style>

</style>
