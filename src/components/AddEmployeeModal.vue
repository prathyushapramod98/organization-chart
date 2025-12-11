<template>
  <div class="fixed inset-0 z-40 flex items-center justify-center bg-black/40">
    <div class="bg-white rounded-lg shadow-lg w-full max-w-md p-6 relative">

      <h2 class="text-lg font-semibold mb-4">Add Employee</h2>

      <form @submit.prevent="onSubmit" class="space-y-3">
        
        <!-- Name -->
        <div>
          <label class="block text-xs font-medium text-gray-600 mb-1">
            Name <span class="text-red-500">*</span>
          </label>
          <input 
            v-model="name" 
            required
            class="w-full border rounded px-2 py-1.5 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
          />
        </div>

        <!-- Title -->
        <div>
          <label class="block text-xs font-medium text-gray-600 mb-1">Title</label>
          <input 
            v-model="title"
            class="w-full border rounded px-2 py-1.5 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="Employee"
          />
        </div>

        <!-- Email -->
        <div>
          <label class="block text-xs font-medium text-gray-600 mb-1">Email</label>
          <input 
            v-model="email"
            type="email"
            class="w-full border rounded px-2 py-1.5 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="name@example.com"
          />
        </div>

        <!-- Phone -->
        <div>
          <label class="block text-xs font-medium text-gray-600 mb-1">Phone</label>
          <input 
            v-model="phone"
            class="w-full border rounded px-2 py-1.5 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500"
            placeholder="N/A"
          />
        </div>

        <!-- Footer Buttons -->
        <div class="flex justify-end gap-2 pt-3">
          <button 
            type="button"
            @click="$emit('close')"
            class="px-3 py-1.5 text-xs rounded border border-gray-300 text-gray-700 hover:bg-gray-100"
          >
            Cancel
          </button>

          <button 
            type="submit"
            class="px-3 py-1.5 text-xs rounded bg-blue-600 text-white hover:bg-blue-700"
          >
            Save
          </button>
        </div>

      </form>

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['save', 'close'])

const name = ref('')
const title = ref('')
const email = ref('')
const phone = ref('')

const onSubmit = () => {
  emit('save', {
    name: name.value.trim(),
    title: title.value.trim() || 'Employee',
    email: email.value.trim() || 'unknown@example.com',
    phone: phone.value.trim() || 'N/A'
  })
}
</script>
