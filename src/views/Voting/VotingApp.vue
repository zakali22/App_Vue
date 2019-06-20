<template>
    <div class="voting">
        <div class="voting__wrapper" v-for="(card, index) in sortedCards" :key="index">
            <error-message :card="card" v-if="card.votes === 25"></error-message>
            <voting-card :card="card" :incrementVote="incrementVote"/>
        </div>
    </div>
</template>

<script>
import { Submissions } from '../../submissions'
import VotingCard from './VotingCard.vue'
import ErrorMessage from './ErrorMessage.vue'
export default {
  components: {
    'voting-card': VotingCard,
    'error-message': ErrorMessage
  },
  data () {
    return {
      cards: Submissions
    }
  },
  computed: {
    sortedCards () {
      const cards = [...this.cards]
      const sorted = cards.sort((a, b) => {
        return b.votes - a.votes
      })
      return sorted
    }
  },
  methods: {
    incrementVote (index) {
      const cardElement = this.cards.find(card => {
        return card.id === index
      })
      cardElement.votes++
    }
  }
}
</script>
