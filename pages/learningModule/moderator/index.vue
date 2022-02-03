<template>
  <v-container>
    <v-row>
      <v-col
        v-for="link in $t('moder_categories')"
        :key="link.routing"
        cols="12"
        md="3"
      >
        <v-btn
          height="200"
          width="200"
          x-large
          color="primary"
          :to="link.routing"
          class="mx-auto mt-10"
          >{{ link.name }}
        </v-btn>
      </v-col>
      <v-col v-if="getValideModer.level > 1" cols="12" md="3">
        <v-btn
          height="200"
          width="200"
          x-large
          color="primary"
          :to="moderSelect.routing"
          class="mx-auto mt-10"
          >{{ moderSelect.name }}
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  name: 'Moderator',
  components: {},
  data() {
    return {
      moderSelect: {
        name: 'Модераторы',
        routing: '/ru/learningModule/moderator/moders',
        key: 'moders'
      }
    }
  },
  computed: {
    ...mapGetters(['getValideModer'])
  },
  watch: {},
  mounted() {},
  created() {},
  methods: {}
}
</script>
<style scoped></style>
