<script setup lang="ts">
import { ref } from 'vue'
import { TreeInjectionKey, TreeNodeInjectionKey, TreeOption } from './tree'
import TreeNode from './tree-node.vue'
import { computed } from 'vue'
import ExpandTransition from './expand-transition.vue'
import { inject } from 'vue'
import { provide } from 'vue'
import { toRefs } from 'vue'

const props = defineProps<{
  option: TreeOption
}>()

const context = inject(TreeInjectionKey)
const expanded = computed(() => {
  return !!context?.expandedKeys?.value.includes(props.option.key)
})
const canOpen = computed(() => expanded.value && !isLeaf.value)
const isLeaf = computed(
  () => !props.option.children || !props.option.children.length,
)

provide(TreeNodeInjectionKey, {
  option: props.option,
  isLeaf: isLeaf.value,
  expanded: expanded,
})
</script>

<template>
  <TreeNode :label="option.label" :node="option" />

  <ExpandTransition>
    <div v-if="canOpen" class="yk-tree__list">
      <node v-for="item in option.children" :key="item.key" :option="item" />
    </div>
  </ExpandTransition>
</template>
