<template>
  <q-layout view="lHh Lpr lFf">


<!--    <Header/>-->
    <q-header>
      <q-toolbar class="bg-purple text-white shadow-2 rounded-borders">
        <q-btn flat label="Homepage" />
        <q-space />

        <!--
          notice shrink property since we are placing it
          as child of QToolbar
        -->
        <q-tabs v-model="tab" shrink stretch>
          <q-tab name="tab1" label="Изделия" />
          <q-tab name="tab2" label="Приход" />
          <q-tab name="tab3" label="Расход" />
        </q-tabs>
      </q-toolbar>
    </q-header>
    <q-page-container>
      <div class="container">
        <router-view />
      </div>

    </q-page-container>
  </q-layout>
</template>
<script>
import { useAuthStore } from 'stores/auth'
//import Header from "components/Header";
export default {
  //components: {Header},
  preFetch ({ store, redirect }) {
    const auth_store = useAuthStore(store)
    if (!auth_store.loggedIn) {
      redirect({ path: '/auth' })
    }
  },
}
</script>

