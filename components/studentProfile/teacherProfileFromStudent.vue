<template>
  <v-container v-if="!isLoading">
    <v-card max-width="1200">
      <v-img
        class="white--text align-end"
        height="100%"
        :src="require('@/assets/teacherProfile.jpg')"
      >
        <v-row align="end" class="fill-height">
          <v-col cols="12" md="4" sm="4" xs="4" class="ml-8">
            <v-avatar size="230">
              <v-img class="ma-8 profile" :src="profileImg"></v-img>
            </v-avatar>
          </v-col>

          <v-col cols="12" md="4" sm="4" xs="4">
            <v-row class="py-7 justify-center">
              <v-col cols="7">
                <v-rating
                  v-model="dataTeacher.rating"
                  class="mr-n5"
                  background-color="white lighten-3"
                  color="white"
                ></v-rating>
              </v-col>
              <v-col cols="5">
                <span class="grey--text text--lighten-2 text-caption">
                  ({{ dataTeacher.rating }})
                </span>
              </v-col>
              <v-col cols="7">
                <div>Телефон преподавателя:</div>
              </v-col>
              <v-col v-if="showPhone" cols="5">
                <div>88005553535</div>
              </v-col>
              <v-col v-if="!showPhone" cols="4">
                <v-btn x-small @click="showPhone = true"
                  >Показать телефон</v-btn
                >
              </v-col>
              <v-col cols="11">
                <div>
                  Город проживания: {{ dataTeacher.cityOfResidence.city }}
                </div>
              </v-col>
              <v-col cols="11">
                <div>Пол: {{ dataTeacher.sex }}</div>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
        <v-card-title class="ml-6">Имя преподавателя</v-card-title>
      </v-img>

      <v-card-text class="text--primary text-center">
        <v-row class="justify-center">
          <v-col cols="12" md="12">
            <v-card
              width="100%"
              outlined
              style="
                background: #79a6f2;
                border: thick double #32a1ce;
                font-size: 16px;
              "
            >
              <v-row style="text-align: left">
                <v-col cols="6">
                  Описание уроков: {{ dataTeacher.descriptionLesson }}
                </v-col>
                <v-col cols="6">
                  Контактные данные: {{ dataTeacher.contactInformation }}
                </v-col>
                <v-col cols="6">
                  Контактные данные: {{ dataTeacher.education }}
                </v-col>
                <v-col cols="6"> Обо мне: {{ dataTeacher.aboutMe }} </v-col>
              </v-row>
            </v-card>
          </v-col>
        </v-row>

        <v-row>
          <v-col
            v-for="(sub, i) in dataTeacher.subjects"
            :key="i"
            cols="12"
            md="4"
            sm="4"
            class="pr-14"
          >
            <v-card height="auto" width="auto" style="background: #a9c9ff">
              <v-text-field
                v-model="sub.name"
                clearable
                clear-icon="mdi-close-circle"
                label="Название предмета"
                disabled
              ></v-text-field>

              <v-checkbox
                v-model="sub.lessonLocation"
                label="Дистанционно"
                disabled
              ></v-checkbox>
              <v-checkbox
                v-model="sub.lessonLocation"
                label="Дома у преподавателя"
                disabled
              ></v-checkbox>

              <v-checkbox
                v-model="sub.lessonLocation"
                label="Дома у ученика"
                disabled
              ></v-checkbox>
              <v-checkbox
                v-model="sub.lessonLocation"
                label="Дома у ученика или у преподавателя"
                disabled
              ></v-checkbox>
              <v-col cols="12">
                <v-text-field
                  v-model="sub.price"
                  clearable
                  clear-icon="mdi-close-circle"
                  label="Цена"
                  disabled
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-select
                  v-model="sub.currency"
                  label="Валюта"
                  disabled
                ></v-select>
              </v-col>
            </v-card>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
    <reviews
      style="background: #e3f2fd"
      class="px-auto mx-auto pt-10"
      :data="feedbacks"
    >
    </reviews>
  </v-container>
</template>

<script>
import reviews from '@/components/teacherProfile/reviews.vue'
import { mapGetters } from 'vuex'
export default {
  components: {
    reviews
  },
  props: {
    dataStudent: {
      type: Object,
      default: null
    },
    dataTeacher: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      showPhone: false,
      feedbacks: [],
      isLoading: true
    }
  },
  computed: {
    ...mapGetters(['feedbacksGet', 'profileTeacherGet']),
    profileImg() {
      return `${process.env.LEARNING_API}/` + this.dataTeacher.photo[0]
    }
  },
  async mounted() {
    await this.getForm()
    await this.getFeedBacks()
    this.isLoading = false
  },
  methods: {
    async getForm() {
      await this.$store.dispatch('GET_FORM_TEACHER')
      this.dataTeacher = this.profileTeacherGet
    },
    async getFeedBacks() {
      await this.$store.dispatch('GET_FEEDBACKS')
      this.feedbacks = this.feedbacksGet
    }
  }
}
</script>
