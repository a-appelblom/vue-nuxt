<template>
  <div>
    <img v-if="selected || locked" :src="card.image" alt="" />
    <img
      v-if="!selected && !locked"
      src="https://deckofcardsapi.com/static/img/X1.png"
      alt=""
    />
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: Object,
      default: () => {},
    },
    hasMatched: {
      type: Boolean,
      default: false,
    },
    selected: {
      type: Boolean,
      default: false,
    },
    setMatched: {
      type: Function,
      default: () => {
        console.log('hello')
      },
    },
  },
  data() {
    return {
      locked: false,
    }
  },
  watch: {
    hasMatched(curr) {
      console.log('watcher')
      if (curr && this.selected) {
        this.setMatched(false)
        this.locked = true
      }
    },
  },
}
</script>
