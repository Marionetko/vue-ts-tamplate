<template>
  <!-- Component Root element -->
  <div class="component-root">

    <!-- Title from props -->
    <h2>{{ title }}</h2>

    <!-- Counter value -->
    <p>Counter: {{ counter }}</p>

    <!-- Button that use increment method -->
    <button @click="increment">Increase</button>

    <!-- Toggle state -->
    <button @click="toggleActive">Toggle active state</button>

    <!-- Additional content slot -->
    <slot name="extra">
      <em>Additional content</em>
    </slot>
  </div>
</template>

<script lang="ts">

// IMPORTS
import {
  defineComponent,
  ref,
  toRefs,
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
} from 'vue';

//  defineComponent
export default defineComponent({
  name: 'FullLifecycleComponent',
  // Props if needed
  props: {
    initial: {
      type: Number,
      required: false,
      default: 0,
    },
    // Prop for Page title
    titleProp: {
      type: String,
      required: false,
      default: 'Template component',
    }
  },
  // Announced events that can be emitted
  emits: ['updated', 'error'],

  // Setup function
  setup(props, { emit }) {

    // Reactive counter from props
    const counter = ref<number>(props.initial);
    // Reactive boolean state
    const active = ref<boolean>(true);
    // Reactive title from props
    const title = ref<string>(props.titleProp);

    // Example of watch for counter with immediate call
    watch(
      // Tracking the counter value
      () => counter.value,
      (newVal, oldVal) => {
        // Throw an 'updated' event every time the counter changes

        // emit an event with information
        emit('updated', { newVal, oldVal });
      },
      // Settings for watch
      { immediate: false }
    );

    // Increment counter function
    const increment = () => {
      // increase the counter
      counter.value += 1;
      // log the value
      console.log('counter', counter.value);
    };

    // Toggle active state function
    const toggleActive = () => {
      // toggle active state
      active.value = !active.value;
      // log the value
      console.log('active', active.value);
    };

    // An example of an exception hook for intercepting errors in child components
    onErrorCaptured((err, instance, info) => {
      // Emit an error event and return false so as not to absorb the error.

      // inform parents components about the error
      emit('error', { err, info });
      // By returning false, we allow the error to be raised further
      return false;
    });

    // Life cycle hooks (Composition API)
    onBeforeMount(() => {
      // Executed immediately before mounting the component
      // It is good to initialize external subscriptions here, but do not manipulate the DOM
      console.log('Before mount');
    });

    onMounted(() => {
      // Executed after the component is mounted in the DOM
      // It is safe to access DOM elements here
      console.log('Component mounted');
    });

    onBeforeUpdate(() => {
      // Called before updating reactive dependencies in the template
      // Here you can capture the state before updating
    });

    onUpdated(() => {
      // Called after DOM update
      // Useful for actions that require an updated DOM
    });

    onBeforeUnmount(() => {
      // Executed before component removal
      // It is recommended to unsubscribe from external subscriptions here
      console.log('Before unmount');
    });

    onUnmounted(() => {
      // Executed after the component has already been unmounted
      // Cleaning timers and resources — a good place
      console.log('Component unmounted');
    });

    // Additional hooks that are applicable in the keep-alive context
    onActivated(() => {
      // Called when the component is activated inside <keep-alive>
      // You can restore state or make network requests
    });

    onDeactivated(() => {
      // Called when the component is deactivated inside <keep-alive>
      // You can pause animations or save state
    });

    // Expose parts of the interface to the template via return
    // Return only what is needed in the template or to external consumers
    return {
      // Reactive refs
      counter,
      active,
      title,

      // Functions
      increment,
      toggleActive,

      // props

      // use props as reactive refs if necessary
      ...toRefs(props),
    };
  }
});
</script>

<style scoped>
  .component-root {
    padding: 16px;
    border: 1px solid #e5e7eb;
    border-radius: 8px;
  }

  button {
    margin-right: 8px;
    padding: 8px 12px;
    cursor: pointer;
  }
</style>
