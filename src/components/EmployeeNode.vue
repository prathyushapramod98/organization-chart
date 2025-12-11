<template>
  <div class="flex flex-col items-center">

    <div class="relative bg-white rounded-lg shadow px-4 pt-3 pb-5 min-w-[220px]">
      <div class="border-l-4 border-blue-600 pl-2">
        <div class="text-xs font-semibold text-gray-700">{{ node.title }}</div>
        <div class="text-base font-bold">{{ node.name }}</div>
        <div class="text-[11px] text-gray-500">{{ node.phone }}</div>
        <div class="text-[11px] text-blue-600">{{ node.email }}</div>
      </div>

      <button
        class="absolute -bottom-3 left-1/2 -translate-x-1/2 text-[10px] px-2 py-1 rounded bg-emerald-500 text-white"
        @click="addChild(node.id)"
      >
        + Add Child
      </button>
    </div>

    <div v-if="node.children?.length" class="w-px h-6 bg-gray-400"></div>

    <draggable
      v-if="node.children?.length"
      v-model="childrenProxy"
      group="org"
      item-key="id"
      class="flex gap-6"
    >
      <template #item="{ element }">
        <EmployeeNode :node="element" />
      </template>
    </draggable>

  </div>
</template>

<script setup>
import { computed, inject } from 'vue'
import draggable from 'vuedraggable'
import EmployeeNode from './EmployeeNode.vue'

const props = defineProps({
  node: Object
})

const updateChildren = inject('updateChildren')
const addChild = inject('addChild')

const childrenProxy = computed({
  get: () => props.node.children,
  set: (val) => updateChildren(props.node.id, val)
})
</script>
