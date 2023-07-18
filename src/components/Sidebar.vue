<template>
  <div class="w-64 bg-white p-4 shadow-md overflow-auto relative">
    <div @click="newChat" class="ring-2 ring-slate-400 py-2 px-4 mb-2 rounded hover:ring-slate-600">+ New chat</div>

    <ul>
      <li v-for="(message, key) in chatsList" :key="key" class="mb-1">
        <a @click="selectChat(key)" href="#"  class="block p-2 text-gray-900 rounded hover:bg-blue-100 hover:text-white"
          >{{ message[0]?.body?.substring(0, 10) }}</a
        >
      </li>
    </ul>

    <LicenseBlock />
  </div>
</template>

<script>

import LicenseBlock from './LicenseBlock.vue';

export default {
  name: 'SideBar',
  components: {
    LicenseBlock
  },
  props: {
    message: {
      type: String,
      required: false,
      default: ''
    },
    chatsList: {
      type: Array,
      required: false,
      default: () => []
    }
  },
  data() {
    return {
      chats: []
    }
  },
  methods: {
    newChat() {
      this.$emit('sideBarClick', {type: 'newChat'});
    },
    selectChat(index) {
      this.$emit('sideBarClick', {type: 'selectChat', index});
    }
  }
}
</script>