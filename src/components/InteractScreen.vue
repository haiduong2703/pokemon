<template>
  <div class="screen">
    <div class="screen__inner" :style="{
      width: `${(((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3 / 4 + 16) * Math.sqrt(cardsContext.length)}px`
    }">

      <card-flip v-for="(card, index) in cardsContext" :key="index" :imgBackFaceUrl="`images/${card}.png`"
        :cardsContexts="cardsContext" :card="{ index, value: card }" @onFlip="checkRule($event)" :ref="`card-${index}`" />
    </div>
  </div>
</template>
<script>
import CardFlip from './CardView.vue';
export default {
  props: {
    cardsContext: {
      typeof: Array,
      default: function () {
        return [];
      }
    }
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      rules: [],
    }
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);
      console.log(this.rules);

      if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
        //add class disable 
        this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode();

        //reset rules
        this.rules = [];
        const disableElements = document.querySelectorAll(".disabled");
        console.log(disableElements.length)
        console.log(this.cardsContext.length - 2)
        if (disableElements && disableElements.length === this.cardsContext.length - 2) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {

        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBack();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBack();
          this.rules = [];


        }, 800);
      }
      else {
        return false;
      }
    }
  }
}
</script>
<style scoped lang="css" >
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background: var(--dark);
  color: var(--light);
}

.screen__inner {

  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style> 