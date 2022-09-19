<template>
  <div class="container-sm mt-20">
    <div class="mx-5">
      <!-- <center>
        <span class="previous_message text-white" @click.prevent="previousMessage">previous message</span>
      </center> -->
      <Message
        v-for="{ id, text, userPhotoURL, userName, userId,createdAt } in messages"
        :key="id"
        :name="userName"
        :photo-url="userPhotoURL"
        :sender="userId === user?.uid"
        :createdAt="new Date(createdAt.seconds * 1000)"
      >
        {{text}}
      </Message>
    </div>
  </div>

  <div ref="bottom" class="mt-20" />

  <div class="bottom">
    <div class="container-sm">
      <form v-if="isLogin" @submit.prevent="send">
        <input v-model="message" placeholder="Message" required />
        <button type="submit">
          <SendIcon />
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref, watch, nextTick } from 'vue'
import { useAuth, useChat } from '@/firebase'
// import { query, orderBy, startAt } from "firebase/firestore";  

import SendIcon from './SendIcon.vue'
import Message from './Message.vue'

export default {
  components: { Message, SendIcon },
  setup() {
    const { user, isLogin } = useAuth()
    let { messages, sendMessage} = useChat()
    const bottom = ref(null)
    watch(
      messages,
      () => {
        nextTick(() => {
          console.log(messages)
          bottom.value?.scrollIntoView({ behavior: 'smooth' })
        })
      },
      { deep: true }
    )

    const message = ref('')
    const send = () => {
      sendMessage(message.value)
      message.value = ''
    }
    return { user, isLogin, messages, bottom, message, send}
  }
}
</script>
