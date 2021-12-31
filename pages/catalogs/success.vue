<template>
  <v-row justify="center" style="margin-top: 200px">
    <p class="description">Вы успешно оплатили товар</p>
  </v-row>
</template>

<script>
// import payPal from '@/components/paypal/paypal.vue'
import { mapGetters } from 'vuex'

export default {
  name: 'HelloWorld',
  components: {
    // payPal
  },
  data() {
    return {}
  },

  computed: {
    ...mapGetters(['getProducts'])
  },
  methods: {}
}
</script>

<style lang="scss" scoped>
.btn_pay {
  margin-top: 5rem;
}
.product {
  width: 400px;
  background: #c2eee1;
  border-radius: 5px;
  padding: 1rem;
  margin: 1rem;
  box-shadow: 1px 1px 5px 0px rgb(50 50 50 / 75%);
}
.container {
  display: flex;
  justify-content: center;
}
.description {
  background: #69f06a82;
  padding: 1rem;
  font-size: 24px;
  border-radius: 5px;
}
</style>
