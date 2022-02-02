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
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'Moderator',
  components: {},
  data() {
    return {
      selectArray: [
        {
          name: 'catalog',
          router: ''
        },
        {
          name: '',
          router: ''
        },
        {
          name: '',
          router: ''
        }
      ]
    }
  },
  computed: {},
  watch: {},
  mounted() {},
  created() {},
  methods: {}
}
</script>
<style scoped></style>
