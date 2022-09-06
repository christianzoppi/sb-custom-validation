<template>
  <div>
    <div>
      <SbBadge v-if="!validationStatus" label="All good!" type="positive" />
      <SbBadge v-if="validationStatus" :label="validationStatus" type="negative" />
    </div>
  </div>
</template>

<script>
import SbBadge from 'storyblok-design-system/src/components/Badge'
export default {
  mixins: [window.Storyblok.plugin],
  data() {
    return {

    }
  },
  components: {
    SbBadge
  },
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: 'cz-test',
        unique_value: 1
      }
    },
    async pluginCreated() {
      window.addEventListener('message', this.processMessage, false)

    },
    processMessage(event) {
      console.log(event)
    }
  },
  watch: {
    'model': {
      handler: function (value) {
        this.$emit('changed-model', value);
      },
      deep: true
    }
  },
  computed: {
    validationStatus() {
      let errors = []
      if(!this.model.unique_value) {
        errors.push('My Field must have a unique value')
      }
      if(errors.length) {
        return `Errors: ${errors.join(', ')}`
      }
      return ''
    }
  }
}
</script>

<style>
  .sb-badge__label {
    font-size: 12px;
  }
</style>
