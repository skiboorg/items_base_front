<template>
  <q-page padding>
    <div class="container">
      <div v-if="item">
        {{item.uuid}}
        <a :href="item.qr_scan" target="_blank">QR</a>
        <br>
        <br>
        <p class="text-bold text-h6">{{item.name}}</p>
        <div class="row q-col-gutter-md q-mb-lg">
          <div class="col-12 col-md-6">
            <q-img :src="item.image"/>
          </div>
          <div class="col-12 col-md-6">

            <p class="q-mb-sm text-grey-7">Категория</p>
            <p class="text-bold text-body2">{{item.category.name}} </p>
            <p class="q-mb-sm text-grey-7">Подкатегория</p>
            <p class="text-bold text-body2">{{item.subcategory.name}}</p>
            <p class="q-mb-sm text-grey-7">Артикул</p>
            <p class="text-bold text-body2">{{item.sku}}</p>
            <p class="q-mb-sm text-grey-7">Серийный номер</p>
            <p class="text-bold text-body2">{{item.serial_number}}</p>
            <div class="bg-grey-2 q-pa-md">
              <p class="no-margin text-bold text-body2">Остаток : {{item.amount}} шт</p>
            </div>

          </div>
        </div>
        <p class="text-bold text-h6">ПРИХОД</p>

        <q-list bordered separator class="q-mb-lg">
          <q-item  class="bg-grey-2">
            <q-item-section>
              <q-item-label>Дата</q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label>Источник</q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label>Кол-во</q-item-label>
            </q-item-section>

          </q-item>
          <q-item  v-for="item in item.supplies">
            <q-item-section>
              <q-item-label>{{new Date(item.date).toLocaleDateString()}}</q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label>{{item.text}}</q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label>{{item.amount}}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
        <p class="text-bold text-h6">РАСХОД</p>

        <q-list bordered separator>
          <q-item  class="bg-grey-2">
            <q-item-section>
              <q-item-label>Дата</q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label>Причина</q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label>Кол-во</q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label>Списание?</q-item-label>
            </q-item-section>
          </q-item>

          <q-item  v-for="item in item.removes">
            <q-item-section>
              <q-item-label>{{new Date(item.date).toLocaleDateString()}}</q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label>{{item.text}}</q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label>{{item.amount}}</q-item-label>
            </q-item-section>
            <q-item-section>
              <q-item-label>{{item.is_remove}}</q-item-label>
            </q-item-section>
          </q-item>

        </q-list>
      </div>
      <div v-else class="fullscreen relative-position">
        <q-inner-loading showing>
          <q-spinner size="50px" color="primary" />
        </q-inner-loading>
      </div>
    </div>

  </q-page>


</template>
<script setup>
import {api} from "boot/axios";
import {useRoute, useRouter} from "vue-router";
import {onBeforeMount, onMounted, ref} from "vue";
const route = useRoute()
const router = useRouter()
const item = ref(null)
const is_loading = ref(false)



onMounted(async ()=>{
  await getItem()
})


const getItem = async () => {
  is_loading.value = !is_loading.value
  const response = await api(`/api/data/products/${route.params.id}`)
  item.value = response.data
  is_loading.value = !is_loading.value
}
</script>
