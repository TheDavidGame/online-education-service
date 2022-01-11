<template>
  <div>
    <v-card class="mx-auto my-7" max-width="800" outlined>
      <v-list-item three-line>
        <v-list-item-content>
          <v-row>
            <v-col cols="7" class="text-h5 mb-4">
              {{ dataCard.fullName }}
            </v-col>
            <v-col cols="4">
              <v-rating
                v-model="dataCard.rating"
                background-color="yellow lighten-3"
                color="yellow"
              ></v-rating>
            </v-col>
          </v-row>
          <v-list-item-title class="text-h6 mb-1">
            {{ dataCard.aboutMe }}
            <v-divider class="my-2"></v-divider>
            <p class="text-overline mb-4">Города где преподает:</p>
            <div v-for="(city, j) in dataCard.citiesForLessons" :key="j">
              <p class="mr-1">{{ city }}</p>
            </div>
            <v-divider class="my-2"></v-divider>
            <p class="text-overline mb-4">Предметы:</p>
            <div v-for="(subjects, m) in dataCard.subjects" :key="m">
              <v-row>
                <v-col cols="3">
                  <p class="mr-1">- {{ subjects.name }}</p>
                </v-col>
                <v-col cols="6">
                  <div>Цена: {{ subjects.price }}</div>
                </v-col>
              </v-row>
            </div>
          </v-list-item-title>
          <v-list-item-subtitle>
            Кол-во отзывов {{ dataCard.feedbacks.length }}</v-list-item-subtitle
          >
        </v-list-item-content>

        <v-list-item-avatar v-if="dataCard.photo[0]" size="150">
          <v-img class="card-img" :src="profileImg"></v-img
        ></v-list-item-avatar>

        <v-list-item-avatar
          v-else
          tile
          size="150"
          color="grey"
        ></v-list-item-avatar>
      </v-list-item>

      <v-card-actions>
        <v-btn outlined rounded text> Страница преподавателя </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
export default {
  components: {},
  props: {
    dataCard: {
      type: Array,
      required: true
    }
  },
  data() {
    return {}
  },
  computed: {
    profileImg() {
      return `${process.env.LEARNING_API}/` + this.dataCard.photo[0]
    }
  },
  mounted() {},
  methods: {}
}
</script>
