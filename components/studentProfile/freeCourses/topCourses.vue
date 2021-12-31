<template>
  <v-container>
    <v-row class="px-12 pt-6 justify-center">
      <!----------- PERSONAL ---------------------->
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
            v-model="timeCourses"
            :rules="numRules"
            clearable
            clear-icon="mdi-close-circle"
            label="Длительность урока"
          ></v-text-field>
        </v-col>
        <v-col cols="12" md="10">
          <v-row class="pb-12 justify-center">
            <h2 style="margin-bottom: -50px">Формат проведения</h2>
          </v-row>
          <v-row>
            <v-col cols="3">
              <v-checkbox
                v-model="selected"
                label="Дистанционно"
                value="Дистанционно"
              ></v-checkbox>
            </v-col>
            <v-col cols="3" class="ml-n10">
              <v-checkbox
                v-model="selected"
                label="У учителя дома"
                value="У учителя дома"
              ></v-checkbox>
            </v-col>
            <v-col cols="3" class="ml-n10">
              <v-checkbox
                v-model="selected"
                label="У ученика дома"
                value="У ученика дома"
              ></v-checkbox>
            </v-col>
            <v-col cols="4" class="ml-n10">
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
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'TopCourses',
  data() {
    return {
      itemCourses: '',
      selected: [],
      infoCourses: '',
      timeCourses: 0
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
  methods: {}
}
</script>
