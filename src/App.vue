<template>
  
  <div id="app" class="container">
  <SourceSelection v-on:sourceChanged="sourceChanged"></SourceSelection>
    <Newslist v-bind:source="source" v-if="source !=''"></Newslist>
    <div class="page-header">

      <h1> Vue.js 2 and Firebase Sample Application</h1>
    </div>

     <div class="panel panel-default">
       <div class="panel-heading">

            <h3>Add Books</h3>
        </div>
        <div class="panel-body">
          <form id="form" class="form-inline" v-on:submit.prevent="addBook">
            <div class="form-group">
              <label for="bookTitle">Title:</label>
              <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">

            </div>
            <div class="form-group">
              <label for="bookAuthor">Author:</label>
              <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">

            </div>

            <div class="form-group">
              <label for="bookUrl">Url:</label>
              <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">

            </div>

            <input type="submit" class ="btn btn-primary" value="Add Book">


          </form>


        </div>
      </div>  
     <div class="panel panel-default">
       <div class="panel-heading">
            <h3> Books Lists</h3>
        </div>
        <div class="panel-body">
            <table class="table table-striped">
                <thead>
                  <tr>
                    <th> Title</th>
                    <th> Author</th>
                    <th> Delete</th>

                  </tr>
                </thead>
                <tbody>
                  <tr v-for="book in books">
                    <td>
                      <a v-bind:href="book.url">{{book.title}}</a>
                    </td>
                    <td> {{book.author}}</td>
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
import Hello from './components/Hello'
import SourceSelection from './components/SourceSelection'
import Newslist from './components/Newslist'
import Firebase from 'firebase'
import toastr from 'toastr'
let config={
    apiKey: "AIzaSyCGeZPv6iGp-Bgh9gmv0BJ8PAsOPmO84u8",
    authDomain: "vuejs-firebase-33d24.firebaseapp.com",
    databaseURL: "https://vuejs-firebase-33d24.firebaseio.com",
    projectId: "vuejs-firebase-33d24",
    storageBucket: "vuejs-firebase-33d24.appspot.com",
    messagingSenderId: "758014411725"
}
// if (!Firebase.apps.length) {
//     let app = ;
// }
    let app = !Firebase.apps.length ? Firebase.initializeApp(config) : Firebase.apps[0]
    let db = app.database();
    let bookRef = db.ref('books');


export default {
  name: 'app',
  firebase: {
    books:bookRef
  },
  data(){
    return{
        newBook : {
        source: '',
        title: '',
        author: '',
        url:''
      },
      source:''
    }
  },
  methods:{
  sourceChanged:function(source) {
    this.source = source;
  },
  OnSourceLoaded(source) {
    this.source = source 
  },

  addBook:function(){
  bookRef.push(this.newBook);
  this.newBook.title = '';
  this.newBook.author = '';
  this.newBook.url = '' ;
  },
  removeBook:function(book){
    bookRef.child(book['.key']).remove();
    toastr.success("Book removed");

  }
  },
  components: {
    Hello,
    SourceSelection,
    Newslist
    
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  margin-top: 60px;
}
</style>
