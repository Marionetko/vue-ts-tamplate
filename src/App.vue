<template>
  <div class="component-wrapper">
    <h2>{{ props.title || 'Lifecycle Demo Component' }}</h2>
    <p>Count: {{ count }}</p>
    <p>Doubled: {{ doubled }}</p>
    <button @click="increment">Increment</button>
  </div>
</template>

<script setup lang="ts">
// -------------------------------
// Imports
// -------------------------------
import {
  ref,
  computed,
  watch,
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
  onActivated,
  onDeactivated,
  onErrorCaptured,
  onRenderTracked,
  onRenderTriggered,
} from 'vue'

// -------------------------------
// Props definition
// -------------------------------
interface Props {
  title?: string
}
const props = defineProps<Props>()

// -------------------------------
// Emits definition
// -------------------------------
const emit = defineEmits<{
  (e: 'update', value: string): void
}>()

// -------------------------------
// Reactive state
// -------------------------------
const count = ref(0) // Example reactive state

// -------------------------------
// Computed properties
// -------------------------------
const doubled = computed(() => count.value * 2) // Derived state example

// -------------------------------
// Methods
// -------------------------------
function increment() {
  count.value++
  emit('update', `Count updated to ${count.value}`)
}

// -------------------------------
// Watchers
// -------------------------------
watch(count, (newVal, oldVal) => {
  console.log(`[watch] count changed from ${oldVal} to ${newVal}`)
})

// -------------------------------
// Lifecycle hooks
// -------------------------------

// Called right before the component is mounted (template not yet rendered)
onBeforeMount(() => {
  console.log('[onBeforeMount] - Called before the component is mounted. Use this for last-minute setup.')
})

// Called after the component is mounted to the DOM
onMounted(() => {
  console.log('[onMounted] - Called after mounting. Use this for DOM access, API calls, or initializing libraries.')
})

// Called right before reactive data updates trigger a re-render
onBeforeUpdate(() => {
  console.log('[onBeforeUpdate] - Called before DOM updates. Use this for pre-update calculations or cleanup.')
})

// Called after the DOM has been updated due to reactive changes
onUpdated(() => {
  console.log('[onUpdated] - Called after DOM updates. Use this to react to layout or visual changes.')
})

// Called before the component instance is unmounted (cleanup stage begins)
onBeforeUnmount(() => {
  console.log('[onBeforeUnmount] - Called before unmounting. Use this to stop intervals, remove listeners, etc.')
})

// Called after the component instance has been unmounted
onUnmounted(() => {
  console.log('[onUnmounted] - Called after unmounting. Use this for final cleanup or logging.')
})

// Called when a kept-alive component is activated (inserted into the DOM again)
onActivated(() => {
  console.log('[onActivated] - Called when a keep-alive component is re-activated.')
})

// Called when a kept-alive component is deactivated (removed from the DOM)
onDeactivated(() => {
  console.log('[onDeactivated] - Called when a keep-alive component is deactivated.')
})

// Called when an error is captured from child components
onErrorCaptured((err, instance, info) => {
  console.error('[onErrorCaptured] - Error captured:', err, info)
  // Return false to prevent further propagation
  return false
})

// Called when a reactive dependency is tracked (for debugging reactivity)
onRenderTracked((e) => {
  console.log('[onRenderTracked] - A reactive dependency was tracked:', e)
})

// Called when a reactive dependency triggers a re-render (for debugging)
onRenderTriggered((e) => {
  console.log('[onRenderTriggered] - A reactive dependency triggered re-render:', e)
})

</script>

<style scoped>
.component-wrapper {
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 12px;
}
button {
  padding: 0.5rem 1rem;
  border: none;
  background: #4caf50;
  color: #fff;
  border-radius: 8px;
  cursor: pointer;
}
button:hover {
  background: #43a047;
}
</style>
