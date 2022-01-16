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
                  readonly
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
              <v-col v-if="dataTeacher.subscription.status" cols="12" md="11">
                <div>Телефон преподавателя:</div>

                <div v-if="showPhone" cols="5">
                  <div>{{ dataTeacher.phone }}</div>
                </div>
                <div v-if="!showPhone" cols="4">
                  <v-btn x-small @click="showPhone = true"
                    >Показать телефон</v-btn
                  >
                </div>
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
        <v-card-title class="ml-6"
          >{{ dataTeacher.fullName }}
          <div class="mx-3">
            Возраст: {{ dataTeacher.age }}
            <span>{{ ageChange(dataTeacher.age) }}</span>
          </div>

          <v-btn small @click="nextPageChat">Написать преподавателю</v-btn>
        </v-card-title>
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
            <v-card
              height="auto"
              width="auto"
              style="background: #a9c9ff; text-align: left; padding: 1em"
            >
              <p>Название предмета</p>
              <p class="text-h6 mb-1">
                {{ sub.name }}
              </p>
              <!-- <v-text-field
                v-model="sub.name"
                clearable
                clear-icon="mdi-close-circle"
                label="Название предмета"
                disabled
              ></v-text-field> -->

              <v-checkbox
                v-if="sub.lessonLocation[0]"
                v-model="sub.lessonLocation"
                label="Дистанционно"
                value="Дистанционно"
                readonly
              ></v-checkbox>
              <v-checkbox
                v-if="sub.lessonLocation[1]"
                v-model="sub.lessonLocation"
                label="Дома у преподавателя"
                value="Дома у преподавателя"
              ></v-checkbox>

              <v-checkbox
                v-if="sub.lessonLocation[2]"
                v-model="sub.lessonLocation"
                label="Дома у ученика"
                value="Дома у ученика"
                readonly
              ></v-checkbox>
              <v-checkbox
                v-if="sub.lessonLocation[3]"
                v-model="sub.lessonLocation"
                label="Дома у ученика или у преподавателя"
                value="Дома у ученика или у преподавателя"
                readonly
              ></v-checkbox>
              <p>Цена</p>
              <p class="text-h6 mb-1">
                {{ sub.price }}
              </p>
              <p>Валюта</p>
              <p class="text-h6 mb-1">
                {{ sub.currency }}
              </p>
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
    }
  },
  data() {
    return {
      showPhone: false,
      feedbacks: [],
      isLoading: true,
      dataTeacher: {},
      text_forms: ['год', 'года', 'лет']
    }
  },
  computed: {
    ...mapGetters(['getTeacherIdData']),

    profileImg() {
      return `${process.env.LEARNING_API}/` + this.dataTeacher.photo[0]
    }
  },
  async mounted() {
    await this.getForm()
    // await this.getFeedBacks()
    this.isLoading = false
  },
  methods: {
    async getForm() {
      await this.$store.dispatch(
        'GET_TEACHER_ID',
        this.$route.params.teacherProfileFromStudent
      )
      this.dataTeacher = this.getTeacherIdData
      console.log(this.dataTeacher)
      this.feedbacks = this.getTeacherIdData.feedbacks
    },
    ageChange(n) {
      n = Math.abs(n) % 100
      const n1 = n % 10
      if (n > 10 && n < 20) {
        return this.text_forms[2]
      }
      if (n1 > 1 && n1 < 5) {
        return this.text_forms[1]
      }
      if (n1 === 1) {
        return this.text_forms[0]
      }
      return this.text_forms[2]
    },
    nextPageChat() {
      this.$router.push({
        name: `messager-dialog-id___ru`,
        params: { id: this.dataTeacher.uid }
      })
    }
    // async getFeedBacks() {
    //   await this.$store.dispatch('GET_FEEDBACKS')
    //   this.feedbacks = this.feedbacksGet
    // }
  }
}
</script>
