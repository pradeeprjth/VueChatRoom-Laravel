<template>
  <app-layout>
    <template #header>
      <h2 class="font-semibold text-xl text-gray-800 leading-tight">
        <chat-room-selection
          v-if="currentRoom.id"
          :rooms="chatRooms"
          :currentRoom="currentRoom"
          v-on:roomchanged="setRoom($event)"
        />
      </h2>
    </template>

    <div class="py-12">
      <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
        <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
          <message-container :messages="messages" />
          <input-messge
           :room="currentRoom" 
           v-on:messagesent="getMessages()" />
        </div>
      </div>
    </div>
  </app-layout>
</template>

<script>
import AppLayout from "@/Layouts/AppLayout";
import MessageContainer from "./MessageContainer.vue";
import InputMessge from "./InputMessge.vue";
import ChatRoomSelection from "./ChatRoomSelection.vue";

export default {
  components: {
    AppLayout,
    MessageContainer,
    InputMessge,
    ChatRoomSelection
  },
  data: function () {
    return {
      chatRooms: [],
      currentRoom: [],
      messages: [],
    };
  },
  methods: {
    getRooms() {
      axios.get('/chat/rooms')
        .then(response => {
          this.ChatRooms = response.data;
          this.setRoom(response.data[0]);
        })
        .catch(error => {
          console.log(error);
        });
    },
    setRoom(room) {
      this.currentRoom = room;
      this.getMessages();
    },
    getMessages() {
      axios.get('/chat/rooms/' + this.currentRoom.id + '/messages')
        .then(response => {
          this.messages = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    },
  },
  created() {
    this.getRooms();
  }
}
</script>
