<template>
  <main class="flex justify-between items-center px-6 md:flex-col md:justify-end md:items-end bg-[#030303] relative">
    <div class="xl:hidden flex justify-start items-start z-50 w-full">
      <button v-b-toggle.sidebar-1>
        <svg xmlns="http://www.w3.org/2000/svg" width="29" height="29" viewBox="0 0 24 24" fill="none" stroke="#FFFFFF"
          stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
          <line x1="3" y1="12" x2="21" y2="12" />
          <line x1="3" y1="6" x2="21" y2="6" />
          <line x1="3" y1="18" x2="21" y2="18" />
        </svg>
      </button>
      <b-sidebar id="sidebar-1" title="Sidebar" no-header shadow backdrop>
        <template #default="{ hide }">
          <div class="p-3 z-50 bg-[#030303] text-white">
            <div class="flex justify-between items-center text-white">
              <h4 class="text-white" id="sidebar-no-header-title text-xl pl-4">
                Visionary Gains
                <!-- <img src="@/assets/img/logo.png" alt="logo" class="w-60"> -->
                <!-- <img src="@/assets/img/Fidelitysvalues.png" alt="logo" class="h-10 w-10"> -->
              </h4>
              <button @click="hide">
                <svg xmlns="http://www.w3.org/2000/svg" width="29" height="29" viewBox="0 0 24 24" fill="none"
                  stroke="#FFFFFF" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
                  <line x1="18" y1="6" x2="6" y2="18" />
                  <line x1="6" y1="6" x2="18" y2="18" />
                </svg>
              </button>
            </div>
            <nav class="mb-3 flex justify-between flex-col">
              <ul class="mt-6 space-y-1">
                <li class="space-y-6">
                  <nuxt-link @click.prevent="v-b-toggle.sidebar-1" v-for="{ name, url, icon } in sidebarItems" :key="name" :to="url"
                    class="rounded-md space-y-2 pl-3 py-2.5 text-sm font-light text-gray-700 flex items-center space-x-3">
                    <!-- <div class="shadow-sm h-8 w-8 p-2 rounded-full">
                      <img :src="require(`~/assets/img/${icon}.png`)" alt="icons" class="">
                    </div> -->
                    <p class="text-white pb-2">
                      {{ name }}
                    </p>
                  </nuxt-link>
                </li>
              </ul>
              <!-- <button
                class="w-full flex justify-center items-center gap-x-3 py-2.5 mt-16 rounded-md bg-red-500 text-white"
                @click="handleLogout">
                Logout
              </button> -->
            </nav>
            <div
              class="absolute bottom-0 bg-[#030303] inset-x-0 border-t border-gray-100 flex items-center justify-between px-4 py-6">
              <div>
                <a href="#" class="flex items-center gap-2">
                  <img alt="Man" src="@/assets/img/user.svg" class="h-8 w-8 rounded-full border object-cover">

                  <div>
                    <p class="text-xs text-white">
                      <strong class="block font-medium text-white">{{ loggedUser?.firstName ?? "" }}
                        {{ loggedUser?.lastName ?? "" }}</strong>

                      <span> {{ loggedUser?.email ?? "" }}</span>
                    </p>
                  </div>
                </a>
              </div>
              <button class="pr-6" @click="handleLogout">
                <span class="shrink-0">
                  <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none"
                    stroke="#f25155" stroke-width="3" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M16 17l5-5-5-5M19.8 12H9M10 3H4v18h6" />
                  </svg>
                </span>
              </button>
            </div>
          </div>
        </template>
      </b-sidebar>
    </div>
    <div class="flex justify-end items-end gap-x-2.5 py-3">
      <div class="rounded-full border p-0.5">
        <img src="@/assets/img/user.svg" class="h-8 w-8 rounded-full" alt="userIcon">
      </div>
      <div class="bg-black rounded-full border p-0.5">
        <img src="@/assets/img/notification.svg" class="h-8 w-8 rounded-full" alt="notificationIcon">
      </div>
    </div>
  </main>
</template>

<script>
import Swal from 'sweetalert2/dist/sweetalert2.js'
// import 'sweetalert2/src/sweetalert2.scss'
export default {
  data() {
    return {
      loggedUser: {},
      sidebarItems: [
        {
          name: 'Dashboard',
          icon: 'analytics',
          url: '/dashboard'
        },
        {
          name: 'Profile',
          icon: 'profile',
          url: '/dashboard/profile'
        },
        {
          name: 'Deposit',
          icon: 'deposit',
          url: '/dashboard/deposit'
        },
        {
          name: 'Withdraw',
          icon: 'deposit',
          url: '/dashboard/withdrawals'
        },
        {
          name: 'Buy plans',
          icon: 'investment',
          url: '/dashboard/purchase-plans'
        },
        {
          name: 'Transaction history',
          icon: 'history',
          url: '/dashboard/transaction-history'
        }
      ]
    }
  },
  mounted() {
    const user = JSON.parse(window.localStorage.getItem('user'))
    if (!user) {
      this.$router.push('/login')
    } else {
      this.loggedUser = user
    }
  },
  methods: {
    handleLogout() {
      Swal.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        type: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, logout!'
      }).then((result) => {
        if (result.value) {
          window.localStorage.removeItem('user')
          window.localStorage.removeItem('auth')
          this.$router.push('/admin')
        } else {
          this.$swal('Cancelled', "You're still logged in!", 'info')
        }
      })
    }
  }
}
</script>

<style scoped>
.nuxt-link-active {
  font-weight: bold;
}

/* exact link will show the primary color for only the exact matching link */
.nuxt-link-exact-active {
  background-color: black;
  color: white;
}
</style>
