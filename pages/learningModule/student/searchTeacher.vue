<template>
  <header>
    <div class="inner">
      <span style="font-size: 200%"
        >Индивидуальные онлайн-занятия с преподавателем</span
      ><br />

      <span class="main-text"> Найти преподавателя прямо сейчас: </span>
      <div class="fields">
        <v-select
          background-color="white"
          label="Предмет"
          filled
          :items="itemSub"
        ></v-select>
        <v-select
          :items="itemTeacher"
          background-color="white"
          label="Город преподавателя"
          filled
        ></v-select>
      </div>
      <v-btn class="search" x-large @click="pageTeachers"> Найти </v-btn>
    </div>
  </header>
</template>

<script>
export default {
  components: {},
  props: {},
  data() {
    return {
      itemSub: ['Алгебра', 'Русский', 'Физика', 'География'],
      itemTeacher: ['Москва', 'Волгоград', 'Смоленск']
    }
  },
  computed: {},
  mounted() {},
  methods: {
    pageTeachers() {
      this.$router.push({
        name: `learningModule-listTeachers___ru`
      })
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
  left: 24%;
  display: inline-block;
}
.fields {
  position: relative;
  top: 150px;
  width: 300px;
}

.inner {
  display: inline-block;
  position: relative;
  z-index: 3;
  right: 100px;
  bottom: 100px;

  color: #fff;
  text-align: left;
  padding: 200px;
}
</style>
