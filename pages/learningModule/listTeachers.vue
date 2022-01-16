<template>
  <div v-if="!isLoading">
    <v-row>
      <v-col cols="8">
        <div v-for="(sub, i) in dataListTeachers" :key="i">
          <cardTeacher :dataCard="sub"> </cardTeacher>
        </div>
        <div v-if="dataListTeachers.length === 0">
          <h1 style="text-align: center; margin-top: 250px">
            {{ $t('studentProfile.noProfiles') }}
          </h1>
        </div>
      </v-col>
      <v-col cols="4">
        <v-card class="ma-10">
          <v-row class="ma-2">
            <v-col cols="12" md="5">
              <v-select
                v-model="sex"
                clearable
                :items="itemSex"
                :label="$t('studentProfile.city')"
              ></v-select>
            </v-col>
            <v-col cols="12" md="5" class="mx-6">
              <v-text-field
                v-model="city"
                clearable
                :label="$t('studentProfile.city')"
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-select
                v-model="subject"
                clearable
                :items="itemSubject"
                :label="$t('studentProfile.subName')"
              ></v-select>
            </v-col>
            <v-col cols="12">
              <v-slider
                v-model="rating"
                max="5"
                :label="$t('studentProfile.rating')"
                thumb-label
              ></v-slider>
            </v-col>
            <v-col cols="12">
              <h3>{{ $t('studentProfile.subprice') }}</h3>
            </v-col>
            <v-col class="px-4">
              <v-range-slider
                v-model="range"
                :max="max"
                :min="min"
                hide-details
                class="align-center"
              >
                <template v-slot:prepend>
                  <v-text-field
                    :value="range[0]"
                    class="mt-0 pt-0"
                    hide-details
                    single-line
                    type="number"
                    style="width: 60px"
                    @change="$set(range, 0, $event)"
                  ></v-text-field>
                </template>
                <template v-slot:append>
                  <v-text-field
                    :value="range[1]"
                    class="mt-0 pt-0"
                    hide-details
                    single-line
                    type="number"
                    style="width: 60px"
                    @change="$set(range, 1, $event)"
                  ></v-text-field>
                </template>
              </v-range-slider>
            </v-col>
            <!-- <v-col cols="12" md="4">
              <v-text-field
                v-model="priceMin"
                :rules="numRules"
                clearable
                label="Минимум"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="4">
              <v-text-field
                v-model="priceMax"
                :rules="numRules"
                clearable
                label="Максимум"
              ></v-text-field>
            </v-col> -->
            <v-col cols="12">
              <h3>{{ $t('studentProfile.subprice') }}</h3>
            </v-col>
            <v-col cols="12" md="4">
              <v-text-field
                v-model="ageMax"
                :rules="numRules"
                clearable
                :label="$t('studentProfile.max')"
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="4">
              <v-text-field
                v-model="ageMin"
                :rules="numRules"
                clearable
                :label="$t('studentProfile.min')"
              ></v-text-field>
            </v-col>

            <v-col cols="12">
              <v-btn @click="filter"> {{ $t('studentProfile.filter') }} </v-btn>
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
      subject: '',
      sex: '',
      rating: 0,
      ageMax: 0,
      ageMin: 0,
      itemSubject: [
        this.$t('studentProfile.language'),
        this.$t('studentProfile.math'),
        this.$t('studentProfile.geography'),
        this.$t('studentProfile.physics')
      ],
      itemSex: [
        this.$t('studentProfile.sexItemMan'),
        this.$t('studentProfile.sexItemWoman')
      ],
      min: 0,
      max: 10000,
      range: [0, 10000]
    }
  },
  computed: {
    ...mapGetters(['getTeacherFilter']),
    numRules() {
      return [v => /^\d+$/.test(v) || this.$t('studentProfile.rulesPattern')]
    }
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
    },
    async filter() {
      this.isLoading = true
      this.dataFilter = {}
      if (this.sex) {
        this.dataFilter.sex = this.sex
      }
      this.dataFilter.price = { ...this.dataFilter.price, min: this.range[0] }

      this.dataFilter.price = { ...this.dataFilter.price, max: this.range[1] }

      if (this.ageMin) {
        this.dataFilter.age = { ...this.dataFilter.age, min: this.ageMin }
      }
      if (this.ageMax) {
        this.dataFilter.age = { ...this.dataFilter.age, max: this.ageMax }
      }
      if (this.city) {
        this.dataFilter.citiesForLessons = [this.city]
      }
      if (this.subject) {
        this.dataFilter.subject = this.subject
      }
      if (this.rating > 0) {
        this.dataFilter.rating = this.rating
      }

      this.dataFilter.price.min = this.min

      await this.$store.dispatch('POST_TEACHER_FILTER', this.dataFilter)
      this.dataListTeachers = [...this.getTeacherFilter]
      this.isLoading = false
    }
  }
}
</script>
