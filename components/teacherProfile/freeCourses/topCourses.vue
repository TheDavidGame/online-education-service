<template>
  <v-container>
    <v-row class="px-12 pt-6 justify-center">
      <v-row class="pb-12 justify-center">
        <v-col cols="12" md="3">
          <v-text-field
            v-model="itemCourses"
            :rules="[v => !!v || 'Заполните поле']"
            clearable
            clear-icon="mdi-close-circle"
            label="Предмет"
          ></v-text-field>
        </v-col>
        <v-col cols="12" md="3">
          <v-text-field
            v-model="price"
            :rules="numRules"
            clearable
            clear-icon="mdi-close-circle"
            label="Цена урока"
          ></v-text-field>
        </v-col>
        <v-col cols="12" md="3">
          <v-select
            v-model="currency"
            :items="CurrencyItem"
            label="Валюта"
          ></v-select>
        </v-col>
        <v-col cols="12" md="10">
          <v-row class="pb-12 justify-center">
            <h2 style="margin-bottom: -50px">Формат проведения</h2>
          </v-row>
          <v-row>
            <v-col cols="6" md="6" sm="10">
              <v-checkbox
                v-model="selected"
                label="Дистанционно"
                value="Дистанционно"
              ></v-checkbox>

              <v-checkbox
                v-model="selected"
                label="У учителя дома"
                value="У учителя дома"
              ></v-checkbox>
            </v-col>
            <v-col cols="6" md="6" sm="2">
              <v-checkbox
                v-model="selected"
                label="У ученика дома"
                value="У ученика дома"
              ></v-checkbox>
              <v-checkbox
                v-model="selected"
                label="У учителя или у ученика"
                value="У учителя или у ученика"
              ></v-checkbox>
            </v-col>
          </v-row>
        </v-col>
        <v-col cols="12" md="4">
          <v-textarea
            v-model="infoCourses"
            clearable
            auto-grow
            rows="2"
            row-height="10"
            clear-icon="mdi-close-circle"
            label="Комментарий к уроку"
          ></v-textarea>
        </v-col>
      </v-row>
      <v-row class="pb-12 justify-center">
        <v-col cols="12" md="4">
          <v-btn @click="addSubject"> Добавить пробный урок </v-btn>
        </v-col>
      </v-row>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'TopCourses',
  data() {
    return {
      subject: {
        name: '',
        lessonLocation: [],
        price: 0,
        currency: '',
        comment: ''
      },
      itemCourses: '',
      selected: [],
      infoCourses: '',
      price: 0,
      currency: '',
      CurrencyItem: ['USD', 'RUB', 'EUR']
    }
  },
  computed: {
    numRules() {
      return [
        v => !!v || this.$t('teacherProfile.fieldRules'),
        v => /^\d+$/.test(v) || this.$t('teacherProfile.rulesPattern')
      ]
    }
  },
  mounted() {},
  methods: {
    addSubject() {
      this.subject.name = this.itemCourses
      this.subject.lessonLocation = [...this.selected]
      this.subject.price = this.price
      this.subject.currency = this.currency
      this.subject.comment = this.infoCourses
      this.sendTestSubject()
    },
    async sendTestSubject() {
      await this.$store.dispatch('POST_TEST_SUBJECT', this.subject)
      this.reloadPage()
    },
    reloadPage() {
      window.location.reload()
    }
  }
}
</script>
