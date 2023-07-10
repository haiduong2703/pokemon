<template>
  <div>
    <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)" />
    <interact-screen v-if="statusMatch === 'match'" :cardsContext="settings.cardContext" @onFinish="onGetResult" />
    <result-screen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="statusMatch = 'default'" />
  </div>
</template>
<script>
import InteractScreen from './components/InteractScreen.vue';
import MainScreen from './components/MainScreen.vue';
import { shuffled } from './utils/array';
import ResultScreen from './components/ResultScreen.vue';

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen
  },
  data() {
    return {
      settings: {
        totalOfBlock: 0,
        cardContext: [],
        startedAt: null,

      },
      statusMatch: "default",
      timer: 0
    }
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log(config.totalOfBlock);
      this.settings.totalOfBlock = config.totalOfBlock;

      const firstCard = Array.from({ length: this.settings.totalOfBlock / 2 }, (_, i) => i + 1);
      const secondCard = firstCard.slice();
      const cards = [...firstCard, ...secondCard];
      this.statusMatch = "match";
      this.settings.cardContext = shuffled(shuffled(shuffled(shuffled(cards))));

      this.settings.startedAt = new Date().getTime();
    },
    onGetResult() {
      //get timer
      this.timer = new Date().getTime() - this.settings.startedAt;

      this.statusMatch = "result";
      //switch to result component
    }
  }
};
</script>


