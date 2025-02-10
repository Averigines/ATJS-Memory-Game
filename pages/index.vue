<template>
  <div id = "wrapper">
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
      <div v-if="win" class="win-message">You finished in {{ turnCount }} turns! {{ winMessage }}</div>
      <button id = "btnRestart" @click="resetGame()">Restart</button>
    </div>
  </div>


</template>

<script>
export default {
  data() {
    return {
      cards: [],
      selectedCards: [],
      totalCards: 20,
      cardValues: [`/images/discus1.png`, `/images/discus1.png`, `/images/discus2.png`, `/images/discus2.png`, `/images/discus3.png`, `/images/discus3.png`, `/images/discus4.png`, `/images/discus4.png`, `/images/discus5.png`, `/images/discus5.png`, `/images/discus6.png`, `/images/discus6.png`, `/images/discus7.png`, `/images/discus7.png`, `/images/discus8.png`, `/images/discus8.png`, `/images/discus9.png`, `/images/discus9.png`, `/images/discus10.png`, `/images/discus10.png`],
      cardBackValue: `/images/cardBack.png`,
      flippedCards: 0,
      turnCount: 0,
      win: false,
      winMessage: "",
      winTable: [[15, "You cheated!"],[25, "Very well done!"],[35, "Not bad!"],[45, "You can do better. Try again!"]]
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
            for(let i=0; i<this.winTable.length; i++) {
              if (this.turnCount <= this.winTable[i][0]) {
                this.winMessage = this.winTable[i][1];
                break;
              }
            }
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
      this.winMessage = "";
      this.initCards()
    }
  }
};
</script>

<style>

#wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
}

h1 {
  font-family: Lato,serif;
  font-size: 40px;
  text-align: center;
  margin-bottom: 4vh;
  color: #005ea6;
}

h2 {
  font-family: Lato,serif;
  font-size: 24px;
  text-align: center;
  color: #005ea6;
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

.card img {
  width: 100%;
  height: 100%;
  object-fit: contain;
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
  margin-top: 40px;
  margin-bottom: 40px;
}

#btnRestart:hover {
  background-color: #005ea6;
}

@media only screen and (max-device-height: 740px) {
  .card {
    width: 80px;
    height: 80px;
  }
}

@media only screen and (max-device-width: 580px) {
  .card {
    width: 60px;
    height: 60px;
  }
}

</style>