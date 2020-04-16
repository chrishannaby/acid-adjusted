<template>
  <div class="p-8">
    <img class="h-32" :src="require(`@/assets/${value}.svg`)" />
    <select
      class="mt-6 w-full form-select font-semibold text-lg"
      :value="value"
      @input="$emit('input', $event.target.value)"
    >
      <option v-for="(fruit, idx) in fruits" :key="idx">{{fruit}}</option>
    </select>
  </div>
</template>

<script>
export default {
  props: ["value", "fruits"]
};
</script>