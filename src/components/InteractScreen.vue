<template>
  <div class="screen">
    <h1>Interact here</h1>
    <card-flip
      v-for="(card, index) in cardsContext"
      :key="index"
      :ref="`card-${index}`"
      :imgBackFaceUrl="`images/${card}.png`"
      :card="{ index, value: card }"
      @onFlip="checkRule($event, cardsContext)"
    />
  </div>
</template>

<script>
import CardFlip from "./Card.vue";

export default {
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      rules: [],
      count: 0,
    };
  },
  components: {
    CardFlip,
  },
  methods: {
    checkRule(card, cardsContext) {
      //console.log(card);
      if (this.rules.length === 2) return false;

      this.rules.push(card);
      //console.log(this.rules);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        //console.log(this.count);
        if (this.count == cardsContext.length / 2 - 1) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 2000);
        }
        //add class disable to component card
        this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableMode();
        this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableMode();
        //reset rules
        this.rules = [];

        this.count += 1;
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          console.log("Wrong");
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          this.rules = [];
        }, 800);
      } else {
        return false;
      }
    },
  },
};
</script>
