<template>
  <v-container class="mt-5">
    <v-row>
      <v-col
        v-for="link in $t('moder_categories')"
        :key="link.routing"
        cols="12"
        md="4"
      >
        <!-- <v-btn
          height="80"
          width="200"
          x-large
          color="primary"
          :to="link.routing"
          class="mx-auto mt-10"
          >{{ link.name }}
        </v-btn> -->
        <nuxt-link :to="link.routing">
          <v-banner color="#0d4456" rounded shaped>
            <v-icon large color="white"> mdi-square-edit-outline </v-icon>
            <span class="white--text">{{ link.name }}</span>
          </v-banner>
        </nuxt-link>
      </v-col>
      <v-col v-if="getValideModer.level > 1" cols="12" md="4">
        <nuxt-link :to="moderSelect.routing">
          <v-banner color="#0d4456" rounded shaped>
            <v-icon large color="white"> mdi-cog </v-icon>
            <span class="white--text">{{ moderSelect.name }}</span>
          </v-banner>
        </nuxt-link>
        <!-- <v-btn
          height="80"
          width="200"
          x-large
          color="primary"
          :to="moderSelect.routing"
          class="mx-auto mt-10"
          >{{ moderSelect.name }}
        </v-btn> -->
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
