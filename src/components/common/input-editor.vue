<script>
export default {
  name: 'input-editor',
  props: {
    rules: {
      type: Array,
      default: () => {}
    },
    color: {
      type: String,
      default: 'heading'
    }
  }
}
</script>

<template lang = "pug">
q-editor(:content-style="{ color: '#84878E', fontWeight: 'normal' }"
         :color = "color"
         :rules = "rules"
         v-bind = "{...$attrs, ...$props, ...$slots}"
         v-on = "$listeners")
</template>
