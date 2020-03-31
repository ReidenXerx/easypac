<template lang="html">
  <div class="application-body flex a-i-center j-c-center">
    <router-view ></router-view>
  </div>
</template>

<script>

import Vue from 'vue';
import VueRouter from 'vue-router';
import Login from './login.vue';
import Workspace from './workspace.vue';

Vue.use(VueRouter);

const routes = [
  { path: '/', component: Login },
  { path: '/workspace/:user', component: Workspace, props: true }
];

export default {
  name: 'app',
  router:
    new VueRouter({
        mode: 'history',
        routes: routes
    }),
  data() {
    return {
    }
  }
}
</script>
