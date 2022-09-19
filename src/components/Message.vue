<template>
  <div class="message">
    <span v-if="!sender" class="text-white">{{ name }} </span>
    <div class="flex" :class="sender ? 'flex-row-reverse' : ''">
      <Avatar class="mt-1" :src="photoUrl" />
      <div class="text w-3/4" :class="sender ? 'bg-green-800' : 'bg-gray-700'">
        <slot />
      </div>
    </div>
    
    <span class="text-white">{{createdAt.toLocaleString()}}</span>
  </div>
</template>

<script>
import Avatar from './Avatar.vue'
export default {
  components: { Avatar },
  props: {
    name: { type: String, default: '' },
    photoUrl: { type: String, default: '' },
    sender: { type: Boolean, default: false },
    createdAt: {type:Date, default: new Date()}
  }
}
</script>
