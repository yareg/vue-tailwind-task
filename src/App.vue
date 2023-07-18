<template>
  <div class="w-screen h-screen bg-gray-50 flex flex-col">
    <div class="bg-gray-800 flex justify-center p-4">
        <span class="text-white font-bold">BuildFast Labs</span>
    </div>

    <!-- Main content div -->
    <div class="flex flex-grow overflow-hidden">
      
      <SideBar :chatsList="chatsList" @sideBarClick="onSideBarClick" />

      <!-- Content -->
      <div class="w-full max-w-screen-lg flex-1 m-auto pt-10 px-10 pb-44 my-4 overflow-auto">
        <div class="flex flex-col">
          <div
            v-for="message in messages"
            :key="message.id"
            class="message rounded-lg py-2 px-6 mb-4"
            :class="message.role === 'assistant' ? 'assistant bg-blue-100 border-blue-100 self-start' : 'user bg-green-200 border-green-200 self-end'"
          >
            <span v-if="message.type === 'text'">{{message.body}}</span>
            <img
              v-if="message.type === 'image'"
              :src="message.url"
              alt="Display Picture"
              class="w-1/2 h-1/2"
            />
            <audio
              v-if="message.type === 'audio'"
              :src="message.url"
              controls
            ></audio>
            <video
              v-if="message.type === 'video'"
              :src="message.url"
              controls
              class="w-full h-auto"
            ></video>

            <span
              v-if="message.beingTyped"
              class="w-2.5 bg-gray-600 h-4 inline-block -mb-0.5 align-baseline animate-blink"
            ></span>
          </div>
          <div
            v-if="showTyping"
            class="message assistant rounded-lg py-2.5 px-6 mb-4 bg-blue-100 border-blue-100 self-start"
          >
            <div class="type-indicator">
              <span>.</span><span>.</span><span>.</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="fixed inset-x-0 bottom-0 bg-gray-200">
      <form
          class="max-w-screen-lg m-auto w-full p-4 flex space-x-4 justify-center items-center"
          @submit.prevent="sendMessage"
        >
          <input
            id="message"
            type="text"
            autocomplete="off"
            class="border rounded-md p-2 flex-1 border-gray-300"
            v-model="newMessage"
            placeholder="Your message..."
          />
          <button
            class="bg-gray-800 text-white px-4 py-2 rounded-md"
            :class="{'opacity-50' : waitingOnResponse}"
          >
            Send
          </button>
        </form>
    </div>
  </div>
</template>

<script>

let element = document.querySelector("#personaltwitter");
if (element) {
  element.classList.remove("bottom-0");
  element.classList.add("top-0");
}

import SideBar  from './components/Sidebar.vue';

export default {
  name: 'App',
  components: { SideBar },
  data() {
    return {
      icenseKey: "",
      openaiKey: "",
      newMessage: "",
      showTyping: false,
      waitingOnResponse: false,
      messages: [],
      chatsList: [],
      mockTypingAfter: 1500,
      mockResponseAfter: 3000,
      mockOpeningMessage:
        "Hello there. I am BuildFastGPT. Created by pwang_szn.",
      mockResponsePrefix: "I am ready to serve",
    }
  },
  methods: {
      sendMessage() {
        // if (this.waitingOnResponse) return;
        this.showTyping = true;

        const message = {
          role: "user",
          body: this.newMessage,
          id: Date.now(),
          type: "text",
        };
        this.messages.push(message);
        //this.messages.push({ role: "assistant", url: 'https://i.ytimg.com/vi/zocutif0cQY/maxresdefault.jpg', type: 'image', id: Date.now() })
        //this.messages.push({ role: "assistant", url: 'https://dl.dropboxusercontent.com/scl/fi/mw10wjgrtnaqdwmvhm38b/Prompts-Concept.wav?dl=0&rlkey=jh3t4banv86o5pal7dgghl4mg', type: 'audio', id: Date.now() })
        //this.messages.push({ role: "assistant",url: 'https://www.dropbox.com/scl/fi/umce1m5xq2ycrc7na7i27/How-To-Create-a-Personal-Chatbot.mp4?dl=1&rlkey=082i3ggai33fctlm8txnhhef8', type: 'video', id: Date.now() })
        this.waitingOnResponse = true;
        // axios({
        //   method: "post",
        //   url: "/api/generate_img",
        //   data: {
        //     res: this.newMessage,
        //   },
        //   headers: {
        //     "X-CSRFToken": "{{ csrf_token }}",
        //   },
        // })
        //   .then((response) => {
        //     console.log(response.data["img_url"]);
        //     image_url = response.data["img_url"];
        //     this.typeOutResponse(image_url, "image");
        //     this.showTyping = false;
        //     this.waitingOnResponse = false;

        //   })
        //   .catch((error) => {
        //     console.error("Error:", error);
        //                 this.showTyping = false;
        //     this.waitingOnResponse = false;
        //     this.typeOutResponse("SORRY THERE WAS AN ERROR", "text");
        //   });

        let currentChatlist = this.chatsList.slice(-1).pop() || [];
        currentChatlist.push(message);
        this.chatsList[(this.chatsList.length || 1) - 1] = currentChatlist;

        this.newMessage = "";
        window.scrollTo(0, document.body.scrollHeight);
      },
      typeOutResponse(message, type = "text") {
        if (type === "image") {
          this.messages.push({
            role: "assistant",
            url: message,
            type: type,
            id: Date.now(),
          });
          this.waitingOnResponse = false;
        } else {
          let responseMessage = {
            role: "assistant",
            body: "",
            beingTyped: true,
            id: Date.now(),
            type: type,
          };
          this.messages.push(responseMessage);
          let i = 0;
          let interval = setInterval(() => {
            responseMessage.body += message.charAt(i);
            i++;
            if (i > message.length - 1) {
              this.waitingOnResponse = false;
              responseMessage.beingTyped = false;
              clearInterval(interval);
            }
          }, 30);
        }
      },
      onSideBarClick({type, index}) {
        switch (type) {
          case 'newChat': 
            this.newMessage = "";
            this.messages = [];
            this.chatsList.push([]);
            break;
          case 'selectChat': 
            this.newMessage = ""; 
            this.messages = this.chatsList[index];
            break;
        }
        
      }
    },
}
</script>

