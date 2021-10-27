<template>
  <div class="grid grid-cols-12 gap-4 p-8 min-h-screen max-w-7xl mx-auto">
    <h1>{{ matched }}</h1>
    <div v-for="card of cards" :key="card.code">
      <div @click="setCards(card)">
        <Card
          :card="card"
          :selected="checkSelected(card)"
          :has-matched="matched"
          :set-matched="setMatched"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cards: [],
      first: { code: '' },
      second: { code: '' },
      matched: false,
      both: 1,
    }
  },
  async fetch() {
    async function getData(url) {
      const res = await fetch(url)
      const data = await res.json()
      return data
    }
    const deck = await getData(
      'https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1'
    )
    const cards = await getData(
      `https://deckofcardsapi.com/api/deck/${deck.deck_id}/draw/?count=52`
    )
    this.cards = cards.cards
  },
  methods: {
    setCards(card) {
      if (!this.first.code) this.first = card
      else if (!this.second.code && card.code !== this.first.code) {
        this.second = card
        if (
          this.first.value === this.second.value &&
          this.getColor(this.first) === this.getColor(this.second)
        ) {
          this.matched = true
        }
      } else {
        this.first = { code: '' }
        this.second = { code: '' }
      }
    },
    checkSelected(card) {
      if (card.code === this.first.code || card.code === this.second.code) {
        return true
      }
      return false
    },
    getColor(card) {
      if (card.suit === 'HEARTS' || card.suit === 'DIAMONDS') return 'red'
      return 'black'
    },
    setMatched(event) {
      if (this.both === 1) this.both++
      else {
        this.first = { code: '' }
        this.second = { code: '' }
        this.matched = false
        this.both = 1
      }
    },
  },
}
</script>
