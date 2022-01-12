<template>
  <header>
    <v-form ref="form" v-model="valid" lazy-validation>
      <div class="inner">
        <span style="font-size: 200%"
          >Индивидуальные онлайн-занятия с преподавателем</span
        ><br />

        <span class="main-text"> Найти преподавателя прямо сейчас: </span>
        <div class="fields">
          <v-select
            v-model="name"
            background-color="white"
            label="Предмет"
            filled
            :rules="[v => !!v || 'Заполните поле']"
            :items="itemSub"
            required
          ></v-select>
          <v-select
            v-model="city"
            :items="itemTeacher"
            background-color="white"
            label="Город преподавателя"
            filled
          ></v-select>
        </div>
        <v-btn :disabled="!valid" class="search" x-large @click="pageTeachers">
          Найти
        </v-btn>
      </div>
    </v-form>
  </header>
</template>

<script>
export default {
  components: {},
  props: {},
  data() {
    return {
      dataFilter: {
        citiesForLessons: [],
        subject: ''
      },
      itemSub: ['Алгебра', 'Русский язык', 'Физика', 'География'],
      itemTeacher: ['Москва', 'Волгоград', 'Воронеж'],
      name: '',
      city: '',
      valid: true
    }
  },
  computed: {},
  mounted() {},
  methods: {
    async sendSearchTeacher() {
      this.dataFilter.subject = this.name
      if (this.city) {
        this.dataFilter.citiesForLessons.push(this.city)
      }
      await this.$store.dispatch('POST_TEACHER_FILTER', this.dataFilter)
      this.$router.push({
        name: `learningModule-listTeachers___ru`
      })
    },
    pageTeachers() {
      if (this.$refs.form.validate()) {
        this.sendSearchTeacher()
      }
    }
  }
}
</script>
<style scoped>
header {
  position: relative;
  background: url(https://cdn.discordapp.com/attachments/914528007132180521/930445892153114664/12-frustrated-student.png)
    center no-repeat;
  -webkit-background-size: cover;
  background-size: cover;
}

header:after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  z-index: 2;
}

.main-text {
  position: relative;
  top: 100px;
  display: inline-block;
  font-size: 20px;
}

.search {
  position: relative;
  top: 150px;

  display: inline-block;
}
.fields {
  position: relative;
  top: 150px;
  width: 50%;
}

.inner {
  display: inline-block;
  position: relative;
  z-index: 3;
  right: 160px;
  bottom: 100px;

  color: #fff;
  text-align: left;
  padding: 200px;
}
</style>
