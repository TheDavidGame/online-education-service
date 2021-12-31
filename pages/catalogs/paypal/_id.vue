<template>
  <v-row justify="center" style="margin-top: 10px">
    <v-card v-if="product" class="mx-auto">
      <v-toolbar v-if="product.discountBool == false" color="#5a88b0" dark
        ><h2>
          {{ product.title }}
        </h2>
      </v-toolbar>
      <v-toolbar v-else color="#2fde55" dark
        ><h2>{{ product.title }}</h2>
      </v-toolbar>

      <v-card-text>
        <h1 v-if="product.discountBool == true" class="card_text">
          <p class="oldPrice">
            Старая цена: {{ product.price.value }}
            {{ product.price.currency }}
          </p>
          <p style="display: inline-block" class="price">
            Новая цена:
            {{
              product.price.value -
              (product.price.value / 100) * product.discount
            }}
            {{ product.price.currency }}
          </p>
        </h1>

        <h1 v-else class="price">
          <p>
            Цена: {{ product.price ? product.price.value : '' }}
            {{ product.price ? product.price.currency : '' }}
          </p>
        </h1>
        <h2 style="padding: 1rem 0">{{ product.time }}</h2>

        <h4 class="description">
          {{ product.description }}
        </h4>
      </v-card-text>
      <div ref="paypal" class="btn_pay"></div>
    </v-card>
    <v-card v-else class="mx-auto">
      <h1>Error</h1>
    </v-card>
  </v-row>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  name: 'HelloWorld',
  data() {
    return {
      loaded: false,
      paidFor: false,
      product: {},
      products: []
    }
  },
  computed: {
    ...mapGetters(['productsGet'])
  },
  mounted() {
    this.getProduct()
  },
  methods: {
    async getProduct() {
      await this.$store.dispatch('GET_PRODUCTS')
      this.products = this.productsGet

      const id = this.$route.params.id
      if (id) {
        const product = this.products.find(x => Number(id) === x.id)
        this.product = product
        this.product.price = this.product.price.en // указывать текущий язык для валюты
        console.log(this.product)

        const script = document.createElement('script')
        script.src =
          'https://www.paypal.com/sdk/js?client-id=ARHn0pYUardbaEFAeo7uia3Jqd6HKsQCDfJUdUKNVbUWhJeItAmM8grqflYl9Xa6MYdWsQlF0bZRCqIQ'
        script.addEventListener('load', this.setLoaded)
        document.body.appendChild(script)
      }
    },

    setLoaded() {
      this.loaded = true
      window.paypal
        .Buttons({
          createOrder: (data, actions) => {
            return actions.order.create({
              purchase_units: [
                {
                  custom_id: this.product.id,
                  description: this.product.description,
                  amount: {
                    currency_code: this.product.price.currency,
                    value: this.product.price.value
                  }
                }
              ]
            })
          },
          onApprove: async (data, actions) => {
            const order = await actions.order.capture()
            this.paidFor = true
            this.opa(order)
            console.log(order)

            this.$router.push({
              name: `catalogs-success___ru`
            })
          },
          onError: err => {
            console.log(err)
          }
        })
        .render(this.$refs.paypal)
    },
    async opa(order) {
      await this.$store.dispatch('PAYMENT_SUCCESS', { data: order })
    }
  }
}
</script>

<style lang="scss" scoped>
.btn_pay {
  margin-top: 5rem;
}
.oldPrice {
  text-decoration: line-through;
  font-size: 20px;
}
.product {
  width: 400px;
  background: #c2eee1;
  border-radius: 5px;
  padding: 1rem;
}
.container {
  display: flex;
  justify-content: center;
}
.price {
  margin-top: 1rem;
  color: #f00;
  font-size: 20px;
}
.description {
  font-size: 20px;
}
</style>
