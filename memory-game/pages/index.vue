<template>
  <h1>Discus-Memory Game</h1>
  <h2>Turns: {{ turnCount }}</h2>
  <div class="container">
    <div class="card-grid">
      <div
          v-for="card in cards"
          :key="card.id"
          :class="['card', {'flipped': card.flipped}]"
          @click="flipCard(card)"
      >
        <div class="back">
          <img :src="cardBackValue" :width="100" :height="100"/>
        </div>
        <div class="front">
          <img :src= "card.value" :width="100" :height="100"/>
        </div>
      </div>
    </div>
    <div v-if="win" class="win-message">You finished in {{ turnCount }} turns! Good job!</div>
    <button id = "btnRestart" @click="resetGame()">Restart</button>
  </div>

</template>

<script>
export default {
  data() {
    return {
      cards: [],
      selectedCards: [],
      totalCards: 4,
      cardValues: [`/images/discus1.png`, `/images/discus1.png`, `/images/discus2.png`, `/images/discus2.png`],
      cardBackValue: `/images/cardBack.png`,
      flippedCards: 0,
      turnCount: 0,
      win: false
    };
  },

  mounted() {
    this.initCards();
  },
  methods: {
    initCards() {
      const shuffledCardValues = this.cardValues.sort(() => Math.random() - 0.5);
      for (let i = 0; i < this.totalCards; i++) {
        this.cards.push({
          id: i,
          value: shuffledCardValues[i],
          flipped: false
        });
      }
    },
    flipCard(card) {
      if (this.selectedCards.length < 2 && !card.flipped) {
        card.flipped = true;
        this.selectedCards.push(card);
        this.checkMatch();
      }
    },
    checkMatch() {
      if (this.selectedCards.length === 2) {
        this.turnCount++
        if (this.selectedCards[0].value === this.selectedCards[1].value) {
          this.flippedCards += 2;
          this.selectedCards = [];
          if (this.flippedCards === this.totalCards) {
            this.win = true;
          }
        } else {
          setTimeout(() => {
            this.selectedCards[0].flipped = false;
            this.selectedCards[1].flipped = false;
            this.selectedCards = [];
          }, 1000);
        }
      }
    },
    resetGame() {
      this.cards.forEach(card => card.flipped = false);
      this.cards.length = 0;
      this.turnCount = 0;
      this.flippedCards = 0;
      this.selectedCards.length = 0;
      this.win = false;
      this.initCards()
    }
  }
};
</script>

<style>

h1 {
  font-family: Lato,serif;
  font-size: 40px;
  text-align: center;
  margin-bottom: 12vh;
}

h2 {
  font-family: Lato,serif;
  font-size: 24px;
  text-align: center;
}

.win-message {
  font-family: Lato,serif;
}

.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  height: auto;

}

.card-grid {
  border-bottom: #4caf50;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-gap: 10px;
  padding-bottom: 40px;
}

.card {
  image-rendering: pixelated;
  width: 100px;
  height: 100px;
  position: relative;
  transform-style: preserve-3d;
  transition: all 0.5s;
  cursor: pointer;
}

.card.flipped {
  transform: rotateY(180deg);
  pointer-events: none;
}

.front, .back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 36px;
}

.front {
  transform: rotateY(180deg);
  background-color: lightblue;
}

.win-message {
  font-size: 2rem;
  color: green;
  text-align: center;
  margin-top: 20px;
}

#btnRestart {
  font-family: Lato,serif;
  font-size: 24px;
  background-color: #0077be;
  border: none;
  color: #fff;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  border-radius: 5px;
  cursor: pointer;
}

#btnRestart:hover {
  background-color: #005ea6;
}

</style>