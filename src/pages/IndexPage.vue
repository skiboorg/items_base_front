<template>
  <q-page padding>
    <q-btn label="add" @click="addDialog=true"/>

    <q-list   bordered separator>
    <q-item class="bg-grey-2">
                  <q-item-section >


                  </q-item-section>
      <q-item-section>
        <q-item-label>Артикул</q-item-label>
      </q-item-section>
      <q-item-section>
        <q-item-label>Название</q-item-label>
      </q-item-section>
      <q-item-section>
        <q-item-label>Серийный номер</q-item-label>
      </q-item-section>
      <q-item-section>
        <q-item-label>Остаток</q-item-label>
      </q-item-section>

    </q-item>
      <q-item clickable v-for="item in products" :key="item.id" @click="$router.push(`/item/${item.id}`)">
        <q-item-section >
          <q-avatar rounded size="100px">
            <img :src="item.image" alt="">
          </q-avatar>

        </q-item-section>
        <q-item-section>
          <q-item-label>{{item.sku}}</q-item-label>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{item.name}}</q-item-label>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{item.serial_number}}</q-item-label>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{item.amount}}</q-item-label>
        </q-item-section>

      </q-item>
    </q-list>
    <q-dialog v-model="addDialog">
      <q-card style="width: 400px; max-width: 80vw;">
        <q-card-section class="row items-center q-pb-none">
          <div class="text-h6">Добавить</div>
          <q-space />
          <q-btn icon="close" flat round dense v-close-popup />
        </q-card-section>
        <q-item-section class="q-pa-md">

        </q-item-section>

        <q-card-actions align="center">
          <q-btn @click="add" no-caps unelevated color="primary" rounded label="Добавить"/>
          <q-btn label="Отмена" no-caps unelevated outline color="primary" rounded v-close-popup/>

        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script setup>
import {api} from "boot/axios";
import {onBeforeMount, ref} from "vue";
const products = ref(null)
const addDialog = ref(false)

const item = ref ({
  name:null,
  sku:null,
  serial_number:null,
  amount:null,
  image:null,
  category:null,
  subcategory:null,
  izdelie:null,

})

onBeforeMount(async ()=>{
  await getProducts()

})
const getProducts = async () => {
  const response = await api(`/api/data/products`)
  products.value = response.data
}

const add = async () => {

}
</script>
