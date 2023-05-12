<template>

  <q-page padding>
    <div class="container">
      <div v-if="item">
        <q-img :src="item.image" alt=""/>
        <div class="text-center">


          <p class="q-mb-sm text-grey-7">Категория</p>
<!--          <p class="text-bold text-body2">{{item.category.name}} </p>-->
          <p class="q-mb-sm text-grey-7">Подкатегория</p>
<!--          <p class="text-bold text-body2">{{item.subcategory.name}}</p>-->
          <p class="q-mb-sm text-grey-7">Артикул</p>
          <p class="text-bold text-body2">{{item.sku}}</p>
          <p class="q-mb-sm text-grey-7">Серийный номер</p>
          <p class="text-bold text-body2">{{item.serial_number}}</p>
          <div class="bg-grey-2 q-pa-md">
            <p class="no-margin text-bold text-body2">Остаток : {{item.amount}} шт</p>
          </div>
        </div>


        <div class="row q-col-gutter-md">
          <div class="col-12 col-md-6">
            <q-btn label="Приход" @click="addDialog = true" no-caps unelevated color="positive" class="full-width"/>
          </div>
          <div class="col-12 col-md-6">
            <q-btn label="Расход" @click="remDialog = true" no-caps unelevated color="negative" class="full-width"/>
          </div>
        </div>
      </div>
      <div v-else class="fullscreen relative-position">
        <q-inner-loading showing>
          <q-spinner size="50px" color="primary" />
        </q-inner-loading>
      </div>
    </div>

  </q-page>
  <q-dialog
    v-model="addDialog"
  >
    <q-card style="width: 300px">
      <q-card-section>
        <div class="text-h6">Приход</div>
      </q-card-section>
      <q-card-section class="q-pt-none">
          <p>Источник</p>
          <q-input v-model="add_data.text" outlined dense />
          <p>Кол-во</p>
          <q-input v-model="add_data.amount" outlined dense />



      </q-card-section>

      <q-card-actions align="right"  class="q-gutter-md">
        <q-btn no-caps unelevated color="positive" @click="add_supply" label="OK" v-close-popup />
        <q-btn no-caps unelevated color="dark"  label="Отмена" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
  <q-dialog
    v-model="remDialog"
  >
    <q-card style="width: 300px">
      <q-card-section>
        <div class="text-h6">Расход</div>
      </q-card-section>
      <q-card-section class="q-pt-none">
        <p>Источник</p>
        <q-input v-model="remove_data.text" outlined dense />
        <p>Кол-во</p>
        <q-input v-model="remove_data.amount" outlined dense />
        <p>Списание</p>
        <q-checkbox v-model="remove_data.is_remove" outlined dense />



      </q-card-section>

      <q-card-actions align="right"  class="q-gutter-md">
        <q-btn no-caps unelevated color="positive" @click="add_remove" label="OK" v-close-popup />
        <q-btn no-caps unelevated color="dark"  label="Отмена" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>
<script setup>
import {api} from "boot/axios";
import {useRoute, useRouter} from "vue-router";
import {onBeforeMount, onMounted, ref} from "vue";
const route = useRoute()
const router = useRouter()
const item = ref({})
const is_loading = ref(false)
const addDialog = ref(false)
const remDialog = ref(false)

const remove_data = ref({
  text:null,
  amount:null,
  is_remove:false
})

const add_data = ref({
  text:null,
  amount:null
})

onMounted(async ()=>{
  await getItem()
})

const add_remove =  async () => {
  is_loading.value = !is_loading.value

  const response = await api.post(`/api/data/product/add_remove`,{data:remove_data.value, id:item.value.id})
  await getItem()
  remDialog.value = false
  remove_data.value.amount = null
  remove_data.value.text = null
  is_loading.value = !is_loading.value
}

const add_supply =  async () => {
  is_loading.value = !is_loading.value
  const response = await api.post(`/api/data/product/add_supply`,{data:add_data.value,id:item.value.id})
  await getItem()
  addDialog.value = false
  add_data.value.amount = null
  add_data.value.text = null
  is_loading.value = !is_loading.value
}

const getItem = async () => {
  is_loading.value = !is_loading.value
  const response = await api(`/api/data/product/by_uuid?uuid=${route.params.uuid}`)
  item.value = response.data
  is_loading.value = !is_loading.value
}
</script>
