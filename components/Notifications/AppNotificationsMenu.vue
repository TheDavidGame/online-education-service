<template>
  <div class="text-center">
    <v-menu
      v-model="menu"
      :close-on-content-click="false"
      :nudge-width="200"
      offset-x
    >
      <!--------------------- NOTIFICATION BUTTON ----------------------->
      <template v-slot:activator="{ on }">
        <v-tooltip bottom>
          <template v-slot:activator="{ on: tooltip }">
            <v-btn
              icon
              color="primary"
              dark
              v-on="{ ...on, ...tooltip }"
              @click="openNotifications"
            >
              <v-icon>{{ svg.bell }}</v-icon>
              <div v-if="unreadNotifications" class="badge-notification"></div>
            </v-btn>
          </template>
          <span>{{ $t('notifications') }}</span>
        </v-tooltip>
      </template>
      <!--------------------- END NOTIFICATION BUTTON ----------------------->

      <!--------------------- NOTIFICATION LIST ----------------------->
      <v-card>
        <v-list rounded>
          <v-list-item v-if="!unreadNotifications">
            {{ $t('noNotifications') }}
          </v-list-item>
          <!--------------------- NOTIFICATION ITEM ----------------------->
          <AppNotificationItem
            v-for="(notification, i) in userNotifications"
            :key="i"
            :data="notification"
          ></AppNotificationItem>
          <!--------------------- END NOTIFICATION ITEM ----------------------->
        </v-list>
        <v-divider></v-divider>
      </v-card>
      <!--------------------- END NOTIFICATION LIST ----------------------->
    </v-menu>
  </div>
</template>

<script>
// COMPONENTS
import AppNotificationItem from '@/components/Notifications/AppNotificationItem.vue'

// VUEX
import { mapGetters } from 'vuex'

// SVG ICONS
import { mdiBellOutline } from '@mdi/js'

export default {
  components: {
    AppNotificationItem
  },
  data: () => ({
    menu: false,
    svg: {
      bell: mdiBellOutline
    }
  }),
  computed: {
    ...mapGetters([
      'userNotifications',
      'unreadNotifications',
      'isAuthenticated'
    ])
  },
  methods: {
    openNotifications() {
      // TODO i18n routes

      if (window.location.pathname === '/') {
        this.menu = true
      } else {
        this.$router.push({ name: `notifications___${this.$i18n.locale}` })
      }
    }
  }
}
</script>

<style>
.badge-notification {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: red;
  position: absolute;
  top: 1px;
  right: 5px;
}
</style>
