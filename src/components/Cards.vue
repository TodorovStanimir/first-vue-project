<template>
  <div>
    <div class="counter">
      <h1>{{counter}}</h1>
    </div>
    <section class="memory-game">
      <div
        class="memory-card"
        v-for="(card, index) in cards"
        :key="index"
        :class="{'flip': card.isFlip, 'disable-card': card.isDisabled}"
        :data-framework="card.framework"
        @click="flipCard(index)"
      >
        <img class="front-face" :src="card.type" :alt="card.alt" />
        <img class="back-face" src="../assets/img/js-badge.svg" alt="JS Badge" />
      </div>
    </section>
  </div>
</template>

<script>
import aurelia from "../assets/img/aurelia.svg";
import vue from "../assets/img/vue.svg";
import angular from "../assets/img/angular.svg";
import backbone from "../assets/img/backbone.svg";
import ember from "../assets/img/ember.svg";
import react from "../assets/img/react.svg";

export default {
  name: "Cards",
  data: function() {
    return {
      cards: [],
      indexCard1: "",
      counter: 60,
      interval: null
    };
  },
  methods: {
    shuffle(array) {
      var source = array.concat([]);
      while (source.length) {
        let index = Math.floor(Math.random() * source.length);
        this.cards.push(source.splice(index, 1)[0]);
      }
    },
    flipCard(index) {
      this.cards[index].isFlip = true;
      this.cards[index].isDisabled = true;
      const counterOpenedCards = this.cards.filter(el => el.isDisabled).length;
      if (this.counter === 60) {
        this.startTime();
      }
      if (counterOpenedCards % 2 !== 0) {
        this.indexCard1 = index;
      } else {
        if (
          this.cards[this.indexCard1].framework !== this.cards[index].framework
        ) {
          this.cards[index].isFlip = false;
          this.cards[index].isDisabled = false;
          this.cards[this.indexCard1].isFlip = false;
          this.cards[this.indexCard1].isDisabled = false;
        }
      }
      if (counterOpenedCards === 12) {
        setTimeout(() => {
          const result = confirm("Game is over! Start a new game?");
          if (result) {
            clearInterval(this.interval);
            this.counter = 60;
            this.indexCard1 = "";
            this.startNewGame();
          } else {
            clearInterval(this.interval);
          }
        }, 500);
      }
    },
    startTime() {
      this.interval = setInterval(() => {
        this.counter--;
      }, 500);
    },
    startNewGame() {
      this.cards.map(card => {
        card.isDisabled = false;
        card.isFlip = false;
        return card;
      });
      const array = [].concat(this.cards);
      this.cards = [];
      this.shuffle(array);
    }
  },
  watch: {
    counter: function(newValue) {
      if (newValue === 0) {
        const result = confirm("Time is up! New game?");
        clearInterval(this.interval);
        this.counter = 60;
        if (result) {
          this.startNewGame();
        } else {
          this.cards.map(card => {
            return (card.isDisabled = true);
          });
        }
      }
    }
  },
  created() {
    this.shuffle([
      {
        type: aurelia,
        framework: "aurelia",
        alt: "Aurelia",
        isFlip: false,
        isDisabled: false
      },
      {
        type: aurelia,
        framework: "aurelia",
        alt: "Aurelia",
        isFlip: false,
        isDisabled: false
      },
      {
        type: vue,
        framework: "vue",
        alt: "Vue",
        isFlip: false,
        isDisabled: false
      },
      {
        type: vue,
        framework: "vue",
        alt: "Vue",
        isFlip: false,
        isDisabled: false
      },
      {
        type: angular,
        framework: "angular",
        isFlip: false,
        isDisabled: false
      },
      {
        type: angular,
        framework: "angular",
        alt: "Angular",
        isFlip: false,
        isDisabled: false
      },
      {
        type: ember,
        framework: "ember",
        alt: "Ember",
        isFlip: false,
        isDisabled: false
      },
      {
        type: ember,
        framework: "ember",
        alt: "Ember",
        isFlip: false,
        isDisabled: false
      },
      {
        type: backbone,
        framework: "backbone",
        alt: "Backbone",
        isFlip: false,
        isDisabled: false
      },
      {
        type: backbone,
        framework: "backbone",
        alt: "Backbone",
        isFlip: false,
        isDisabled: false
      },
      {
        type: react,
        framework: "react",
        alt: "React",
        isFlip: false,
        isDisabled: false
      },
      {
        type: react,
        framework: "react",
        alt: "React",
        isFlip: false,
        isDisabled: false
      }
    ]);
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.memory-game {
  width: 640px;
  height: 640px;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
  perspective: 1000px;
}

.memory-card {
  width: calc(25% - 10px);
  height: calc(33.333% - 10px);
  margin: 5px;
  position: relative;
  transform: scale(1);
  transform-style: preserve-3d;
  transition: transform 0.5s;
  box-shadow: 1px 1px 1px rgba(0, 0, 0, 0.3);
}

.disable-card {
  pointer-events: none;
}

.memory-card:active {
  transform: scale(0.97);
  transition: transform 0.2s;
}

.memory-card.flip {
  transform: rotateY(180deg);
}

.front-face,
.back-face {
  width: 100%;
  height: 100%;
  padding: 20px;
  position: absolute;
  border-radius: 5px;
  background: #fff29e;
  backface-visibility: hidden;
}

.front-face {
  transform: rotateY(180deg);
}

.counter {
  position: absolute;
  top: 0px;
  right: 0px;
}
</style>
