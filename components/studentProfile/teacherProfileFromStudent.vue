<template>
  <v-container>
    <v-card max-width="1200">
      <v-img
        class="white--text align-end"
        height="300px"
        src="https://avatars.mds.yandex.net/get-zen_doc/4303740/pub_605fa616b1f696367c469a53_605fa9db188a9f73594cefdc/scale_1200"
      >
        <v-row>
          <v-col cols="6" md="4" class="ml-8">
            <v-avatar size="200">
              <v-img class="relative ma-8 card-img" :src="profileImg"></v-img>
            </v-avatar>
          </v-col>

          <v-col cols="6" md="4">
            <v-row class="py-7 justify-center">
              <v-col cols="12">
                <v-rating
                  v-model="rating"
                  class="mx-n2"
                  background-color="white lighten-3"
                  color="white"
                ></v-rating>
              </v-col>
              <v-col cols="12">
                <div>Телефон преподавателя</div>
              </v-col>
              <v-col cols="12">
                <div>Город преподавателя</div>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
        <v-card-title class="ml-6">Имя преподавателя</v-card-title>
      </v-img>

      <v-card-text class="text--primary">
        <v-row>
          <v-col
            v-for="(sub, i) in dataTeacher.subjects"
            :key="i"
            cols="12"
            class="pr-14"
            sm="4"
          >
            <v-card
              height="450"
              class="mx-auto pa-5"
              style="background: #a9c9ff"
            >
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
  </v-container>
</template>

<script>
export default {
  components: {},
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
      rating: 4
    }
  },
  computed: {
    profileImg() {
      return `${process.env.LEARNING_API}/` + this.dataStudent.photo[0]
    }
  },
  mounted() {
    console.log(this.dataTeacher)
  },
  methods: {}
}
</script>
