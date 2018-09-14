<template>
  <div id="app" class="container">
    <h1>Vue and Firebase</h1>

    <div class="card">
      <div class="card-header">
        <h3>Add A Link</h3>
      </div>

      <div class="card-body">
        <form v-on:submit.prevent="addLink" class="form-inline">
          <div class="form-group">
            <label for="">Title</label>
            <input 
              v-model="newLink.title"
              class="form-control"
              type="text"
              placeholder="text">
          </div>

          <div class="form-group">
            <label for="">Author</label>
            <input 
              v-model="newLink.author"
              class="form-control"
              type="text"
              placeholder="author">
          </div>

          <div class="form-group">
            <label for="">Url</label>
            <input 
              v-model="newLink.url"
              class="form-control"
              type="text"
              placeholder="url">
          </div>

          <input type="submit" class="btn btn-success" value="Add A Link">
        </form>
      </div>
    </div>
    
    <hr>

    <div class="card">
      <div class="card-header">
        <h3 class="card-title">Links List</h3>
      </div>

      <div class="card-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>User</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="link in links">
              <td>
                <a target="_blank" v-bind:href="link.url">{{ link.title }}</a>
              </td>
              <td>
                {{ link.author }}
              </td>
              <td>
                <button
                  v-on:click="deleteLink(link)" 
                  class="btn btn-danger">
                  <i class="fa fa-trash-o" aria-hidden="true"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
  apiKey: "AIzaSyA5nopYKBSMDGA8eygrFbc4HAes9WjGycA",
  authDomain: "vuejsfirebase-786e0.firebaseapp.com",
  databaseURL: "https://vuejsfirebase-786e0.firebaseio.com",
  projectId: "vuejsfirebase-786e0",
  storageBucket: "vuejsfirebase-786e0.appspot.com",
  messagingSenderId: "243065310635"
};

let app = Firebase.initializeApp(config);
let db = app.database();

let linksRef = db.ref('links');

export default {
  name: 'App',
  firebase: {
    links: linksRef
  },
  data() {
    return {
      newLink: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addLink: function() {
      linksRef.push(this.newLink);
      this.newLink.title = '';
      this.newLink.author = '';
      this.newLink.url = '';
    },
    deleteLink: function(link) {
      linksRef.child(link['.key']).remove();
      toastr.success('Link removed');
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
