<template>
  <div></div>
</template>

<script>
export default {
  components: {},
  props: {},
  data() {
    return {
      dataFilter: {},
      name: '',
      city: '',
      subject: '',
      min: 0,
      max: 0
    }
  },
  computed: {
    numRules() {
      return [v => /^\d+$/.test(v) || this.$t('Введите число')]
    }
  },

  methods: {
    filter() {
      this.dataFilter = {}
      if (this.city) {
        this.dataFilter.citiesForLessons = [this.city]
      }

      if (this.name) {
        this.dataFilter.fullName = this.name
      }
      if (this.subject) {
        this.dataFilter.subject = this.subject
      }
      if (this.max) {
        this.dataFilter.price.max = this.max
      }
      if (this.min) {
        this.dataFilter.price.min = this.min
      }
      console.log(this.dataFilter)
      //   await this.$store.dispatch('POST_TEACHER_FILTER', this.dataFilter)
    }
  }
}
</script>
