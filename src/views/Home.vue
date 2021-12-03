<template>
  <div class="home">
    <h1>{{counter}}</h1>
    <img @click="hitServer" alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App"/>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import { io, Socket } from 'socket.io-client';
import { ServerToClientEvents, ClientToServerEvents } from '@/typings/socketio';
import HelloWorld from '@/components/HelloWorld.vue'; // @ is an alias to /src

@Options({
  components: {
    HelloWorld,
  },
})
export default class Home extends Vue {
  socket: Socket<ServerToClientEvents, ClientToServerEvents> = io('http://localhost:3000');

  counter = 0;

  hitServer():void {
    console.log('sending');
    this.socket.emit('hello');
  }

  mounted() {
    this.socket.on('counter', (counter: number) => {
      this.counter = counter;
    });
  }
}
</script>
