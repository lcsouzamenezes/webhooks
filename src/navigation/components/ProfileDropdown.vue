<template>
  <Popover
    class="relative"
  >
    <PopoverButton class="text-gray-600 dark:text-dark-50 inline-flex items-center py-2 text-sm font-semibold focus:outline-none">
      <UserCircleIcon class="w-6 h-6" />
    </PopoverButton>

    <transition
      enter-active-class="transition duration-200 ease-out"
      enter-from-class="translate-y-1 opacity-0"
      enter-to-class="translate-y-0 opacity-100"
      leave-active-class="transition duration-150 ease-in"
      leave-from-class="translate-y-0 opacity-100"
      leave-to-class="translate-y-1 opacity-0"
    >
      <PopoverPanel class="absolute z-10 w-auto bg-white dark:bg-dark-800 right-0 mt-2 transform">
        <div class="overflow-hidden border dark:border-dark-600 rounded-sm transition duration-150 ease-in-out">
          <div class="px-3 py-4 border-b border-gray-100 dark:border-dark-600">
            <p class="text-md font-medium text-ellipsis overflow-hidden text-gray-700 dark:text-dark-50">
              {{ currentUserEmail }}
            </p>
          </div>
          <RouterLink
            v-if="false"
            :to="{ name: 'settings' }"
            class="flex items-center p-2 font-light text-gray-700 dark:text-dark-50 hover:bg-gray-50 dark:hover:bg-dark-900"
          >
            <SettingsIcon class="w-8 h-8 flex-none mr-2" />
            <span class="flex-1">Settings</span>
          </RouterLink>
          <a
            href="#"
            class="flex items-center p-2 font-light text-gray-700 dark:text-dark-50 hover:bg-gray-50 dark:hover:bg-dark-900"
            @click="signOutCurrentUser"
          >
            <LogOutIcon class="w-4 h-4 flex-none mr-2" />
            <span class="flex-1"> Sign out</span>
          </a>
        </div>
      </PopoverPanel>
    </transition>
  </Popover>
</template>

<script>
import { fetchSession, currentUser, signOutCurrentUser } from 'application/scripts/current_user'
import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'
import { LogOutIcon, UserCircleIcon, SettingsIcon } from 'icons'

export default {
  name: 'ProfileDropdown',
  components: {
    Popover,
    PopoverButton,
    PopoverPanel,
    LogOutIcon,
    UserCircleIcon,
    SettingsIcon
  },
  computed: {
    currentUserEmail: () => currentUser.email
  },
  methods: {
    signOutCurrentUser () {
      signOutCurrentUser()

      if (process.env.VSCODE || process.env.ELECTRON) {
        fetchSession().then(() => {
          this.$router.push({ name: 'home' })
        })
      } else {
        location.href = '/'
      }
    }
  }
}
</script>
