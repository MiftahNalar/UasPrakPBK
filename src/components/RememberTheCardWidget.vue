<template>
  <div class="remember-the-card-widget">
    <h2>Remember the Card</h2>
    <div class="cards">
      <div v-for="(card, index) in cards" :key="index" @click="flipCard(index)">
        <div class="card-front" v-if="!card.flipped"></div>
        <div class="card-back" v-if="card.flipped">{{ card.value }}</div>
      </div>
    </div>
    <button @click="resetGame">Reset</button>
    <p>{{ status }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cards: [
        { value: 'A', flipped: false, matched: false },
        { value: 'A', flipped: false, matched: false },
        { value: 'B', flipped: false, matched: false },
        { value: 'B', flipped: false, matched: false },
        { value: 'C', flipped: false, matched: false },
        { value: 'C', flipped: false, matched: false },
        { value: 'D', flipped: false, matched: false },
        { value: 'D', flipped: false, matched: false },
        { value: 'E', flipped: false, matched: false },
        { value: 'E', flipped: false, matched: false },
        { value: 'F', flipped: false, matched: false },
        { value: 'F', flipped: false, matched: false },
      ],
      isGameEnded: false,
      flippedCards: [],
    };
  },
  computed: {
    status() {
      if (this.isGameEnded) {
        return 'Game Over!';
      }
      return 'Remember the Card';
    },
  },
  methods: {
    flipCard(index) {
      if (!this.isGameEnded && !this.cards[index].flipped && this.flippedCards.length < 2) {
        this.cards[index].flipped = true;
        this.flippedCards.push(index);
        if (this.flippedCards.length === 2) {
          const [cardIndex1, cardIndex2] = this.flippedCards;
          if (this.cards[cardIndex1].value === this.cards[cardIndex2].value) {
            this.cards[cardIndex1].matched = true;
            this.cards[cardIndex2].matched = true;
            this.flippedCards = [];
            this.checkGameEnd();
          } else {
            setTimeout(() => {
              this.cards[cardIndex1].flipped = false;
              this.cards[cardIndex2].flipped = false;
              this.flippedCards = [];
            }, 1000);
          }
        }
      }
    },
    checkGameEnd() {
      const allCardsMatched = this.cards.every(card => card.matched);
      if (allCardsMatched) {
        this.isGameEnded = true;
      }
    },
    resetGame() {
      this.shuffleCards();
      this.cards.forEach(card => {
        card.flipped = false;
        card.matched = false;
      });
      this.isGameEnded = false;
      this.flippedCards = [];
    },
    shuffleCards() {
      // Fisher-Yates Shuffle Algorithm
      for (let i = this.cards.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [this.cards[i], this.cards[j]] = [this.cards[j], this.cards[i]];
      }
    },
  },
};
</script>

<style scoped>
.remember-the-card-widget {
  background-color: grey;
  border: 1px solid #ccc;
  padding: 20px;
  margin-bottom: 20px;
}

.remember-the-card-widget h2, p {
  color: whitesmoke;
}

.cards {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 10px;
  margin-bottom: 10px;
}

.cards div {
  border: 1px solid #ccc;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.card-front {
  background-color: #eee;
}

.card-back {
  background-color: #ccc;
  color: #fff;
  font-size: 24px;
}

button {
  margin-top: 10px;
}
</style>
