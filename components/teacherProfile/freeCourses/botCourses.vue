<template>
  <v-container v-if="!isLoading">
    <h1 class="ma-5" style="text-align: center">Мои предметы</h1>
    <v-row class="px-12 pt-6 justify-center">
      <v-row class="pb-12 justify-center">
        <v-col v-for="(sub, i) in dataTestSubject" :key="i">
          <v-row>
            <v-col cols="7" class="text-h5 mb-4">
              {{ sub.name }}
              <v-divider class="my-2"></v-divider>
            </v-col>

            <v-col cols="7" class="text-h5 mb-4">
              {{ sub.price }}
              <v-divider class="my-2"></v-divider>
            </v-col>

            <v-col cols="7" class="text-h5 mb-4">
              {{ sub.currency }}
              <v-divider class="my-2"></v-divider>
            </v-col>

            <v-col
              v-for="(city, j) in dataTestSubject.lessonLocation"
              :key="j"
              cols="7"
              class="text-h5 ma-4"
            >
              {{ city }}
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-row>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  name: 'BotCourses',
  data() {
    return {
      dataTestSubject: {},
      isLoading: true
    }
  },
  computed: {
    ...mapGetters(['getTestSubject'])
  },
  async mounted() {
    await this.getDataTestSubject()
    this.isLoading = false
  },
  methods: {
    async getDataTestSubject() {
      await this.$store.dispatch('GET_TEST_SUBJECT')
      this.dataTestSubject = this.getTestSubject
      console.log(this.dataTestSubject)
    }
  }
}
</script>
