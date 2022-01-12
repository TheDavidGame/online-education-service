<template>
  <div v-if="!isLoading">
    <div v-for="(sub, i) in dataListTeachers" :key="i">
      <cardTeacher :dataCard="sub"> </cardTeacher>
    </div>
    <div v-if="dataListTeachers.length === 0">
      <h1 style="text-align: center; margin-top: 250px">
        Таких анкет не найдено
      </h1>
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
    ...mapGetters(['getTeacherFilter'])
  },
  async mounted() {
    await this.getTeachersList()
    this.isLoading = false
  },
  methods: {
    async getTeachersList() {
      await this.$store.dispatch('GET_TEACHERS')
      this.dataListTeachers = [...this.getTeacherFilter]
      console.log(this.dataListTeachers)
    }
  }
}
</script>
