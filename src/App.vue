<template>
  <div id="app" class="container">
  	<div class="page-header">
		<h1>Vuejs and Firebase Sample application</h1>
	</div>

	<div class="panel panel-default">
		<div class="panel-heading">
			<h3>Add Books</h3>
		</div>
		<div class="panel-body">
			<form id="form" class="form-inline" v-on:submit.prevent="addBook">
			<div class="form-group">
				<label for="bookTitle">Title:</label>
				<input type="text" id="bookTitle" class="form" v-model="newBook.title">
			</div>
			<div class="form-group">
				<label for="bookAuthor">Author:</label>
				<input type="text" id="bookAuthor" class="form" v-model="newBook.author">
			</div>
			<div class="form-group">
				<label for="bookUrl">URL:</label>
				<input type="text" id="bookUrl" class="form" v-model="newBook.url">
			</div>
			<br>
			<br>
			<input type="submit" class="btn btn-primary" value="Add Book">
			</form>
		</div>
	</div>

	<div class="panel panel-default">
		<div class="panel-heading">
			<h3>Books Lists</h3>
		</div>
		<div class="panel-body">
			<table class="table table-striped">
				<thead>
					<tr>
						<th class="text-left">
							Title
						</th>
						<th class="text-left">
							Author
						</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="book in books">
						<td class="text-left">
							<a :href="book.url" target='_blankpage'>{{ book.title }}</a>
						</td>
						<td class="text-left">
							{{ book.author }}
						</td>
						<td>
							<span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span>
						</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

import Firebase from "firebase";

import toastr from "toastr"

let config = {
  apiKey: "AIzaSyDFHcr4HBYkid-dcRelDCUZFevI4fwba5k",
  authDomain: "vuejs-firebase-01-63044.firebaseapp.com",
  databaseURL: "https://vuejs-firebase-01-63044.firebaseio.com",
  projectId: "vuejs-firebase-01-63044",
  storageBucket: "vuejs-firebase-01-63044.appspot.com",
  messagingSenderId: "66750260875"
};

let app = Firebase.initializeApp(config);

//establishing connection to firebase

let db = app.database();

//access to data stored in books

let booksRef = db.ref("books");

export default {
  name: "App",

  firebase: {
    books: booksRef
  },
  data() {
    return {
      newBook: {
        title: "",
        author: "",
        url: ""
      }
    }
  },
  methods:{
	  addBook: function(){
			booksRef.push(this.newBook);
			this.newBook.title='';
			this.newBook.author='';
			this.newBook.url='';
			toastr.success('Book Added Successfully');
		},
		removeBook: function(book){
			booksRef.child(book['.key']).remove();
			toastr.success('Book Removed');
		}
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
