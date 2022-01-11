<template>
  <div v-if="!isLoading">
    <div v-for="(sub, i) in dataListTeachers" :key="i">
      <cardTeacher :dataCard="sub"> </cardTeacher>
    </div>
  </div>
</template>

<script>
import cardTeacher from '@/components/listTeachers/cardTeacher.vue'
import { mapGetters } from 'vuex'
export default {
  components: {
    cardTeacher
  },
  props: {},
  data() {
    return {
      dataListTeachers: [],
      isLoading: true
    }
  },
  computed: {
    ...mapGetters(['getTeachers'])
  },
  async mounted() {
    await this.getTeachersList()
    this.isLoading = false
  },
  methods: {
    async getTeachersList() {
      await this.$store.dispatch('GET_TEACHERS')
      this.dataListTeachers = [...this.getTeachers]
      console.log(this.dataListTeachers)
    }
  }
}
</script>
