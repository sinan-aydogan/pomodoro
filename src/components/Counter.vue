<template>
<div class="flex flex-col items-center justify-around w-full h-screen space-y-10 bg-gray-400">
  <div class="flex flex-col items-center justify-center w-full">
    <!-- Main Content Block -->
    <div class="w-1/4 p-4 space-y-4 bg-blue-700 rounded-md bg-opacity-80">
      <!-- Header -->
      <h1 class="text-2xl font-bold text-center text-gray-200">
        Pomodoro Time Tracker
      </h1>
      <!-- Counter -->
      <div class="flex flex-row w-full space-x-4">
          <!-- Minute -->
        <div class="flex flex-col justify-center w-1/2 py-4 text-center bg-gray-300 rounded-md">
          <span class="font-semibold text-7xl">{{ minute }}</span>
          <span class="">minutes</span>
        </div>
        <!-- Second -->
        <div class="flex flex-col justify-center w-1/2 py-4 text-center bg-gray-300 rounded-md">
          <span class="font-semibold text-7xl">{{ second == 0 ? "00" : second }}</span>
          <span class="">seconds</span>
        </div>
      </div>
      <!-- Rounds -->
      <div class="flex items-center justify-center w-full p-2 font-semibold text-center text-white bg-green-500">
        Round {{ round }}
      </div>
      <!-- Buttons -->
      <div class="flex flex-row space-x-4">
        <!-- Start Button -->
        <Button @click="start" v-if="newSession && !working" label="Start" color="green" />
        <!-- Stop Button -->
        <Button @click="stop" v-if="working" label="Stop" color="red" />
        <!-- Continue -->
        <Button label="Continue" @click="start" v-if="!newSession && !working" color="yellow" />
        <!-- Reset Button -->
        <Button label="Reset" @click="reset" v-if="!newSession && !working" color="indigo" />
      </div>
    </div>
    <span class="text-sm text-gray-600">
      Spended Time: <b>{{ workingTime }}</b>
      </span>
  </div>
  <div class="flex justify-center w-full p-10 font-bold bg-gray-700 text-8xl">
        {{ turnTitle }}
    </div>
</div>
 
</template>

<script>
import Button from "./Button";
export default {
  components: {
    Button,
  },
  data() {
    return {
      timeDefault : 1500,
      restDefault : 300,
      time: 1500,
      rest: 300,
      counter: null,
      turn : true,
      round : 0,
      newSession : true,
      working : false,
    };
  },
  computed: {
    minute() {
      return Math.floor(this.time / 60);
    },
    second() {
      return Math.floor(this.time % 60);
    },
    turnTitle() {
      var title;
      if(this.newSession == true && this.turn && this.newSession == true){
        title =  'Let\'s work...';
      }
      if(this.working == true && this.turn && this.newSession == false){
        title =  'Working time...';
      }
      if(this.working == false && this.turn && this.newSession == false){
        title =  'Working time stopped, let\'s work' ;
      }
      if(this.working == true && !this.turn && this.newSession == false){
        title =  'Rest time, cool down buddy...';
      }
       if(this.working == false && !this.turn && this.newSession == false){
        title =  'You shouldn\'t rest more, let\'s press continue button';
      }
      return title;
    },
    workingTime(){
      var time = this.round*this.timeDefault;
      var title;
      if(time / 3600 > 1){
        title = Math.floor(time / 3600) +' hours ' + (Math.floor(time / 60) - Math.floor(time / 3600)*60) + ' minutes';
      }else{
        title = Math.floor(time / 60) + ' minutes';
      }
      return title;
    }
  },
  methods: {
    start() {
      var vm = this;
      this.counter = setInterval(function () {
        vm.time -= 1;
      }, 1000);
      this.working = true;      
      this.newSession = false;      
    },
    stop() {
      clearInterval(this.counter);
      this.working = false;     
    },
    reset() {
      clearInterval(this.counter);
      this.newSession = true;
      if(this.turn){
        this.time = 5;
      }else{
        this.time = 3;
      }
      
    },
  },
  watch : {
    time(){
      if(this.time < 0) {
        this.time = this.rest;
        if(this.turn){
          this.round += 1;
        }
        this.turn = !this.turn;
      }
    }
  }
};
</script>