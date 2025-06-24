<template>
  <div class="grid grid-cols-12 gap-3 items-center p-3 hover:bg-gray-50 rounded-lg transition-colors group">
    <div class="col-span-3">
      <input
          v-model="tags"
          @blur="handleBlur"
          maxlength="50"
          class="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all text-gray-800 text-sm"
      />
    </div>
    <div class="col-span-2">
      <select
          v-model="obj.type"
          class="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all text-gray-800 text-sm bg-white"
      >
        <option value="">Тип записи</option>
        <option
            v-for="option in options"
            :key="option.text"
            :value="option.text"
            @click="setNullPass(obj.type, obj.id)"
        >
          {{ option.text }}
        </option>
      </select>
    </div>
    <div :class="hidePasswordField ? 'col-span-6' : 'col-span-4'">
      <input
          v-model="obj.login"
          @blur="handleBlur"
          maxlength="100"
          required
          placeholder="Логин (обязательно)"
          class="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all text-gray-800 text-sm"
          :class="{ 'border-red-500': !obj.login?.trim() }"
      />
    </div>
    <div v-if="!hidePasswordField" class="col-span-2 relative">
      <input
          v-model="obj.pass"
          @blur="handleBlur"
          :type="showPassword ? 'text' : 'password'"
          maxlength="100"
          :required="!hidePasswordField"
          placeholder="Пароль"
          class="w-full px-3 py-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500 outline-none transition-all text-gray-800 text-sm pr-9"
          :class="{ 'border-red-500': !hidePasswordField && !obj.pass?.trim() }"
      />
      <button
          type="button"
          @click="showPassword = !showPassword"
          class="absolute right-2 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-blue-500 focus:outline-none"
          :title="showPassword ? 'Скрыть пароль' : 'Показать пароль'"
      >
        <EyeOff v-if="showPassword" class="w-4 h-4" />
        <Eye v-else class="w-4 h-4" />
      </button>
    </div>
    <div class="col-span-1 flex justify-end">
      <button
          @click="deleteData(obj.id)"
          class="p-1 text-gray-400 hover:text-red-500 rounded-full hover:bg-red-50 transition-colors opacity-0 group-hover:opacity-100 focus:opacity-100"
      >
        <Trash2 class="w-4 h-4" />
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import { Eye, EyeOff, Trash2 } from 'lucide-vue-next'
import { useAccountStore } from '../stores/store.ts'
import { storeToRefs } from 'pinia'

const accountStore = useAccountStore()
const deleteData = (id: number) => accountStore.deleteData(id)
const setNullPass = (type: string, id: number) => accountStore.setNullPass(type, id)
const { options } = storeToRefs(accountStore)
const obj = defineModel({ type: Object })
const showPassword = ref(false)
const tags = computed({
  get() {
    return Array.isArray(obj.value.tags) ? obj.value.tags.join(';') : ''
  },
  set(value) {
    obj.value.tags = value.length > 0 ? value.split(';') : []
  },
})
const validateField = (input: HTMLInputElement) => {
  if (input.required) {
    input.classList.toggle('border-red-500', !input.value.trim())
  }
}
const handleBlur = (event: Event) => {
  validateField(event.target as HTMLInputElement)
}
const hidePasswordField = computed(() => obj.value.type === 'Local')
</script>

