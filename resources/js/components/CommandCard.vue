<template>
  <div class="h-full">
    <div v-if="!command.error" :class="{'shadow-2xl': hover}" class="flex h-full flex-col px-8 py-7 rounded-xl bg-white cursor-pointer transition ease-in-out duration-200" @click="onClick"
         @mouseenter="hover = true"
         @mouseleave="hover = false">

      <div :class="{'text-gray-700': !active, 'text-red-500': active}"
           class="text-xl mb-4 transition ease-in-out duration-200">
        {{ command.name }}
      </div>

      <div :class="{'mb-7': command.arguments != null || command.options != null}" class="text-gray-500">
        {{ command.description }}
      </div>

      <div class="flex items-center mt-auto justify-end">

        <badge v-if="command.arguments != null" :active="active" :count="Object.keys(command.arguments).length"
               name="Arguments"/>

        <div v-if="command.options != null" class="ml-2">
          <badge :active="active" :count="Object.keys(command.options).length" name="Options"/>
        </div>

      </div>

    </div>

    <div v-else class="h-full bg-gray-200 px-8 py-7 rounded-xl relative overflow-hidden">

      <div class="text-gray-300 w-80 h-80 mb-6 absolute -top-20 -left-20 opacity-50">
        <svg class="h-full w-full" fill="none" stroke="currentColor" viewBox="0 0 24 24"
             xmlns="http://www.w3.org/2000/svg">
          <path d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" stroke-linecap="round" stroke-linejoin="round"
                stroke-width="2"/>
        </svg>
      </div>

      <div class="text-xl mb-4 relative">

        <div class="flex items-center mb-4">
          <div class="w-2 h-2 rounded-full bg-red-500 mr-3 mt-1">
          </div>
          <div>
            {{ command.name }}
          </div>
        </div>

        <div class="text-gray-500">{{ command.error }}</div>

      </div>
    </div>

  </div>
</template>

<script>
import Badge from "./CommandCard/Badge";

export default {
  components: {Badge},
  props: ['command'],
  data() {
    return {
      hover: false,
    }
  },
  computed: {
    active() {
      return this.hover;
    }
  },
  methods: {
    onClick() {
      this.$emit('select');
    }
  }
}
</script>

<style scoped>

</style>