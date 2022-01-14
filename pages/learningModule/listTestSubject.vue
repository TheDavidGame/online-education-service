<template>
  <div v-if="!isLoading">
    <v-row>
      <v-col cols="8">
        <div v-for="(sub, i) in dataListTeachers" :key="i">
          <cardTeacher :dataCard="sub" class="ml-4"> </cardTeacher>
        </div>
        <div v-if="dataListTeachers.length === 0">
          <h1 style="text-align: center; margin-top: 250px">
            Таких анкет не найдено
          </h1>
        </div>
      </v-col>
      <v-col cols="4">
        <v-card class="ma-10">
          <v-row class="ma-2">
            <v-col cols="6">
              <v-text-field
                v-model="city"
                clearable
                label="Город"
              ></v-text-field>
            </v-col>
            <v-col cols="6">
              <v-select
                v-model="subject"
                clearable
                :items="itemSubject"
                label="Предмет"
              ></v-select>
            </v-col>
            <v-col cols="12">
              <v-btn @click="filter"> Фильтровать </v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </div>
  <div v-else>
    <v-progress-circular
      style="margin-left: 45%; margin-top: 20%"
      :size="120"
      color="primary"
      indeterminate
    ></v-progress-circular>
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
      isLoading: true,
      dataFilter: {},
      city: '',
      itemSubject: ['Русский язык', 'Математмка', 'Физика']
    }
  },
  computed: {
    ...mapGetters(['getTestSubjectFilter'])
  },
  async mounted() {
    await this.getTeachersList()
    this.isLoading = false
  },
  methods: {
    async getTeachersList() {
      await this.$store.dispatch('GET_TEACHERS')
      this.dataListTeachers = [...this.getTestSubjectFilter]
      console.log(this.dataListTeachers)
    },
    async filter() {
      this.isLoading = true
      this.dataFilter = {}
      if (this.city) {
        this.dataFilter.citiesForLessons = [this.city]
      }
      if (this.subject) {
        this.dataFilter.subject = this.subject
      }

      await this.$store.dispatch('POST_TEST_SUBJECT_FILTER', this.dataFilter)
      this.dataListTeachers = [...this.getTestSubjectFilter]
      this.isLoading = false
    }
  }
}
</script>
