<template>
  <v-container>
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-row class="px-12 pt-6 justify-center">
        <v-row class="pb-12 justify-center">
          <v-col cols="12" md="3">
            <v-text-field
              v-model="itemCourses"
              :rules="[v => !!v || 'Заполните поле']"
              clearable
              clear-icon="mdi-close-circle"
              label="Предмет"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-text-field
              v-model="price"
              :rules="numRules"
              clearable
              clear-icon="mdi-close-circle"
              label="Цена урока"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="3">
            <v-select
              v-model="currency"
              :items="CurrencyItem"
              label="Валюта"
              :rules="[v => !!v || 'заполните поле']"
              required
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
                  :rules="checkboxRules"
                  required
                ></v-checkbox>

                <v-checkbox
                  v-model="selected"
                  label="У учителя дома"
                  value="У учителя дома"
                  :rules="checkboxRules"
                  required
                ></v-checkbox>
              </v-col>
              <v-col cols="6" md="6" sm="2">
                <v-checkbox
                  v-model="selected"
                  label="У ученика дома"
                  value="У ученика дома"
                  :rules="checkboxRules"
                  required
                ></v-checkbox>
                <v-checkbox
                  v-model="selected"
                  label="У учителя или у ученика"
                  value="У учителя или у ученика"
                  :rules="checkboxRules"
                  required
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
              label="Описание урока"
              required
            ></v-textarea>
          </v-col>
        </v-row>
        <v-row class="pb-12 justify-center">
          <v-col cols="12" md="4">
            <v-btn :disabled="!valid" @click="addSubject">
              Добавить пробный урок
            </v-btn>
          </v-col>
        </v-row>
      </v-row>
    </v-form>
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
      CurrencyItem: ['USD', 'RUB', 'EUR'],
      valid: true
    }
  },
  computed: {
    numRules() {
      return [
        v => !!v || this.$t('teacherProfile.fieldRules'),
        v => /^\d+$/.test(v) || this.$t('teacherProfile.rulesPattern')
      ]
    },
    checkboxRules() {
      return [this.selected.length > 0 || this.$t('learningModule.rulesField')]
    }
  },
  mounted() {},
  methods: {
    addSubject() {
      if (this.$refs.form.validate()) {
        this.subject.name = this.itemCourses
        this.subject.lessonLocation = [...this.selected]
        this.subject.price = this.price
        this.subject.currency = this.currency
        this.subject.comment = this.infoCourses
        this.sendTestSubject()
      }
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
