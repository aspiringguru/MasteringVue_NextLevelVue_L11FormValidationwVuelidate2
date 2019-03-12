<template>
  <div>
    <label v-if="label">{{ label }}</label>
    <input :value="value" @input="updateValue" v-bind="$attrs" v-on="listeners">
    <!-- these are in conflict :  @input="updateValue" v-on="$listeners" -->
    <!-- remove the $ from $listeners when computed property added below. -->
  </div>
</template>

<script>
export default {
  inheritAttrs: false,
  props: {
    label: {
      type: String,
      default: ''
    },
    value: [String, Number]
  },
  computed: {
    listeners() {
      return {
        ...this.$listeners,
        input: this.updateValue
        //this fixes the conflict between @input="updateValue" v-on="$listeners" above.
      }
    }
  },
  methods: {
    updateValue(event) {
      this.$emit('input', event.target.value)
    }
  }
}
</script>
