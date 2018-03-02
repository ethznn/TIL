## Vue JS production with firebase

[VueJS 첫 배포](https://vue-project-33cb8.firebaseapp.com/)

주소 : https://vue-project-33cb8.firebaseapp.com/



```bash
vue init webpack project_name
...
cd project_name
...
npm install firebase vuefire vuex --save
```



project_name/config/index.js

```javascript
// Various Dev Server settings
    host: 'localhost', // can be overwritten by process.env.HOST
    port: 8080, // can be overwritten by process.env.PORT, if port is in use, a free one will be determined
    autoOpenBrowser: true, // 변경 부분
    errorOverlay: true,
    notifyOnErrors: true,
    poll: false, // https://webpack.js.org/configuration/dev-server/#devserver-watchoptions-
```



project_name/src/components/Main.vue // 초기 페이지에 들어가는 ruby 의 application.html.erb 느낌

```vue
<template>
  <div>
    <h1> Hello Component </h1>
    <h2> {{ msg }} </h2>
  </div>
</template>

<script>
export default {
  name: 'Main',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style scoped>
</style>
```



project_name/src/router/index.js

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'

import Main from '@/components/Main' // 추가

Vue.use(Router)

export default new Router({

  mode: 'history', // 추가 주소 끝에 '/' 추가 안함

  routes: [
    {
      path: '/',
      name: 'Main', // 추가
      component: Main // 추가
    }
  ]
})
```



project_name/src/App.vue

```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
    <ul>
      <li v-for="presenter in presenters">
        presenter : {{ presenter['name'] }} subject : {{ presenter['subject'] }} date : {{ presenter['date'] }}
        <button v-on:click="DeleteItem(presenter)"> delete </button>
      </li>
      <input placeholder = "presenter" v-model="name" />
      <input placeholder = "subject" v-model="subject" />
      <input placeholder = "date" v-model="date" />
      <button v-on:click="AddItem"> add </button>
    </ul>
  </div>
</template>

<script>
import Firebase from 'firebase'
let config = {
    apiKey: "Firebase_apiKey",
    authDomain: "first-prayeo.firebaseapp.com",
    databaseURL: "https://first-prayeo.firebaseio.com",
    projectId: "first-prayeo",
    storageBucket: "first-prayeo.appspot.com",
    messagingSenderId: "Firebase_messagingSenderId"
  };

let app = Firebase.initializeApp(config)
let db = app.database()
let presentersRef = db.ref('presenters')

export default {
  name: 'App',

  firebase: {
    presenters: presentersRef 
  },

  methods: {
    AddItem: function() {
      var newkey = Date.now();
      var model = {
        name: this.name,
        subject: this.subject,
        date: this.date
      };
      db.ref('presenters/' + newkey).set(model);

      this.name = this.subject = this.date = '';
    },
    DeleteItem: function(presenter) {
      this.presenters.forEach(element => {
        if( element.name == presenter['name'] &&
            element.subject == presenter['subject'] &&
            element.date == presenter['date']) {
              db.ref('presenters/' + element['.key']).remove();
            }
      });
    }
  },

  data(){
    return {
      name : "",
      subject : "",
      date: ""
    }
  },
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

```



project_name/src/main.js

```javascript
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router'

import VueFire from 'vuefire' // 추가

Vue.config.productionTip = false

Vue.use(VueFire)
/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```



firebase install

```bash
npm install -g firebase-tools
...
firebase login
```



project_name/firebase.json

```json
{
  "hosting": {
    "public": "./dist",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}

```



build

```bash
npm run build
```



deploy

```bash
firebase deploy --project firebase_project_name
```

