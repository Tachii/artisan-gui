<template>
  <div>

    <div v-if="option['accept_value']">

      <label :for="id" class="block text-gray-500 mb-1 text-sm">
        {{ option.title }}
        <span v-if="option.required" class="text-red-500">*</span>
        <span v-if="option.array" class="px-1 py-px rounded text-xs bg-gray-200">array</span>
      </label>

      <div v-if="option['array']">

        <div
            class="flex flex-wrap items-center cursor-text px-2 py-px border bg-white rounded-lg focus-within:border-primary-500 transition ease-in-out duration-200"
            @click="$refs.arrayInput.focus()">

          <input v-for="item in items" :name="option.name + '[]'" :value="item" type="hidden">

          <div v-for="item in items"
               class="cursor-pointer px-2 py-1 bg-gray-100 text-gray-800 hover:bg-gray-200 mr-1 rounded my-1 transition ease-in-out duration-200"
               @click="deleteItem(item)">
            {{ item }}
          </div>

          <input :id="id"
                 ref="arrayInput"
                 v-model="arrayInput"
                 :required="option.required && items.length === 0"
                 class="focus:outline-none bg-transparent flex-1 px-2 py-1 my-1"
                 placeholder="Add item..."
                 type="text"
                 @focus="errorMessage = ''"
                 @keydown.tab.prevent="addItem"
                 @keydown.enter.prevent="addItem"
                 @keydown.backspace="removeChar"
          >
        </div>

      </div>

      <div v-else>
        <input :id="id"
               v-model="input"
               :name="option.name"
               :placeholder="'Enter ' + option.title.toLowerCase() + '...'"
               :required="option.required && false"
               class="px-4 py-2 rounded-lg border w-full focus:outline-none focus:border-primary-500 transition ease-in-out duration-200"
               type="text"
               @focus="errorMessage = ''"
        >
      </div>

    </div>

    <div v-else>

      <input :id="id" :checked="checked" :name="option.name" :required="option.required" hidden type="checkbox"
             value="1">
      <div class="flex items-center cursor-pointer" @change="errorMessage = ''" @click="checked = !checked">

        <div :class="{'border-primary-300': checked}"
             class="h-5 w-5 border bg-white rounded mr-2 flex items-center justify-center">
          <svg v-if="checked" class="text-primary-500 w-4 h-4" fill="none" stroke="currentColor"
               viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path d="M5 13l4 4L19 7" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"/>
          </svg>
        </div>

        <div class="block text-gray-700">
          {{ option.title }}
          <span v-if="option.required" class="text-red-500">*</span>
        </div>

      </div>

    </div>

    <div class="text-xs mt-1">
      <div v-if="option.description != null && !errorMessage" class="text-gray-400">
        {{ option.description }}
      </div>

      <div v-else-if="errorMessage" class="text-red-800 opacity-80">
        {{ errorMessage }}
      </div>
    </div>

  </div>
</template>

<script>
export default {
  props: ['command', 'option', 'error', 'old'],
  mounted() {

    if (this.error)
      this.errorMessage = this.error

    if (this.old) {
      if (this.option['accept_value']) {

        if (this.option['array'])
          this.items = this.old
        else
          this.input = this.old

      } else {
        this.checked = true
      }
    }

  },
  data() {
    return {
      checked: false,
      items: [],
      arrayInput: '',
      errorMessage: '',
      input: '',
    }
  },
  computed: {
    id() {
      return this.command.name + this.option.name;
    }
  },
  methods: {
    addItem() {

      if (this.arrayInput === '')
        return;

      if (!this.items.includes(this.arrayInput))
        this.items.push(this.arrayInput);

      this.arrayInput = '';
    },
    deleteItem(item) {
      const index = this.items.indexOf(item);

      if (index > -1)
        this.items.splice(index, 1);

    },
    removeChar(e) {

      if (this.arrayInput === '' && this.items.length > 0) {
        e.preventDefault();
        let last = this.items[this.items.length - 1];
        console.log(last);
        this.deleteItem(last);
        this.arrayInput = last;
      }

    }
  }
}
</script>