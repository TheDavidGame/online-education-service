<template>
  <v-card min-height="100%" elevation="10">
    <v-toolbar flat height="200" color="blue">
      <v-row>
        <v-col class="d-flex justify-center">
          <v-icon size="100" color="white">{{ avatar }}</v-icon>
        </v-col>
        <v-col cols="12">
          <div
            class="headline font-weight-bold text-capitalize white--text text-center"
          >
            {{ name }}
          </div>
        </v-col>
      </v-row>
    </v-toolbar>
    <v-card-text>
      <v-row>
        <v-col>
          <!------------------------ CONTENT OF PANEL ----------------------->
          <slot></slot>
          <!------------------------ END CONTENT OF PANEL ----------------------->
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
import { mdiAccount } from '@mdi/js'

export default {
  props: {
    name: {
      type: String,
      default: 'Вход'
    }
  },
  data: () => ({
    avatar: mdiAccount
  })
}
</script>
