<template>
  <div class="max-w-6xl mx-auto mt-4">

    <header class="flex items-start justify-between mb-6">
      <div>
        <h1 class="text-3xl font-bold tracking-wide">Organization Chart</h1>
      </div>

      <button
        class="px-4 py-2 text-sm rounded bg-red-500 text-white hover:bg-red-600"
        @click="resetData"
      >
        Reset
      </button>
    </header>

    <main class="flex justify-center min-h-[200px]">

      <div v-if="!tree" class="text-center">
        <p class="text-gray-600 mb-3">Your organization chart is empty.</p>
        <button
          @click="showRootModal = true"
          class="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700"
        >
          Create Root Employee
        </button>
      </div>

      <!-- Show tree -->
      <EmployeeNode v-else :node="tree" />
    </main>

    <!-- Modal for adding employee -->
    <AddEmployeeModal
      v-if="showAddModal || showRootModal"
      @close="showAddModal = false; showRootModal = false"
      @save="handleSaveNewEmployee"
    />
  </div>
</template>

<script setup>
import { ref, watch, provide } from 'vue'
import EmployeeNode from './EmployeeNode.vue'
import AddEmployeeModal from './AddEmployeeModal.vue'

const STORAGE_KEY = 'org-chart'


const saved = localStorage.getItem(STORAGE_KEY)
const tree = ref(saved ? JSON.parse(saved) : null)


const showRootModal = ref(false)


watch(
  tree,
  (val) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(val))
  },
  { deep: true }
)


const findNodeById = (node, id) => {
  if (!node) return null
  if (node.id === id) return node
  for (const child of node.children || []) {
    const found = findNodeById(child, id)
    if (found) return found
  }
  return null
}

const updateChildren = (nodeId, newChildren) => {
  const node = findNodeById(tree.value, nodeId)
  if (!node) return
  node.children = newChildren
}

const showAddModal = ref(false)
const currentParentId = ref(null)

const openAddChildModal = (parentId) => {
  currentParentId.value = parentId
  showAddModal.value = true
}

const closeAddModal = () => {
  showAddModal.value = false
  currentParentId.value = null
}

const handleSaveNewEmployee = (payload) => {
  if (!tree.value) {
  
    tree.value = {
      id: crypto.randomUUID(),
      ...payload,
      children: []
    }
    showRootModal.value = false
    closeAddModal()
    return
  }

  const parent = findNodeById(tree.value, currentParentId.value)
  if (!parent) return

  parent.children.push({
    id: crypto.randomUUID(),
    ...payload,
    children: []
  })

  closeAddModal()
}

const resetData = () => {
  tree.value = null
  localStorage.removeItem(STORAGE_KEY)
  showRootModal.value = false
}

// Provide functions for EmployeeNode.vue
provide('updateChildren', updateChildren)
provide('addChild', openAddChildModal)
</script>


