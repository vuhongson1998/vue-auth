<template>
  {{ message }}
</template>
<script>
import { onMounted, ref } from 'vue'
import { useStore } from "vuex"
export default {
  name: "HomePage",
  setup() {
    const message = ref('')
    const store = useStore()

    onMounted(async () => {
      try {
        const response = await fetch(process.env.VUE_APP_BASE_URL + '/user', {
          headers: { 'Content-Type': 'application/json' },
          credentials: 'include'
        })

        const content = await response.json()

        message.value = `Hi ${content.name}`
        
        await store.dispatch('setAuth', true)
      } catch (error) {
        message.value = 'You are not logged in!'
        await store.dispatch('setAuth', false)
      }
    })

    return {
      message
    }
  }
}
</script>