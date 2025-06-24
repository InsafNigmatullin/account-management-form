<template>
  <div class="max-w-4xl mx-auto bg-white rounded-xl shadow-sm overflow-hidden">
    <div class="px-6 py-4 border-b flex justify-between items-center">
      <h2 class="text-xl font-semibold text-gray-800">Учетные записи</h2>
      <button
          @click="addData"
          class="p-2 rounded-full bg-blue-100 text-blue-600 hover:bg-blue-200 transition-colors"
          aria-label="Добавить запись"
      >
        <PlusCircle class="w-7 h-7" />
      </button>
    </div>
    <div class="px-6 py-3 bg-blue-50 text-blue-800 text-sm flex items-start gap-2">
      <ShieldQuestion class="w-4 h-4 mt-0.5 flex-shrink-0" />
      <p>Для указания нескольких меток для одной пары логин/пароль используйте разделитель ;</p>
    </div>
    <div class="divide-y divide-gray-200">
      <div class="grid grid-cols-12 gap-4 px-6 py-3 bg-gray-50 text-xs font-medium text-gray-500 uppercase tracking-wider">
        <div class="col-span-3">Метки</div>
        <div class="col-span-3 hidden sm:block">Тип записи</div>
        <div class="col-span-3 sm:hidden">Тип</div>
        <div class="col-span-3">Логин</div>
        <div class="col-span-3">Пароль</div>
      </div>
      <FormRow v-for="(obj, key) in data" :key="obj.id" v-model="data[key]" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { PlusCircle, ShieldQuestion } from 'lucide-vue-next';
import FormRow from './AccountFormRow.vue'
import { useAccountStore } from '../stores/store.ts'
import { storeToRefs } from 'pinia'

const accountStore = useAccountStore()
const { data } = storeToRefs(accountStore)
const addData = () => accountStore.addData()
</script>
