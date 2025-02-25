<template>
  <div class="poll">
    <h2>{{ question }}</h2>

    <div v-if="!voted" class="poll-container">
      <button
        v-for="(option, index) in options"
        :key="index"
        @click="vote(index)"
        class="poll-option"
      >
        {{ option }}
      </button>
    </div>

    <div v-else class="results">
      <p v-for="(option, index) in options" :key="index">
        {{ option }}: {{ percentages[index] }}%
      </p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

defineProps({
  question: String,
  options: Array,
});

const votes = ref(new Array(4).fill(0));
const totalVotes = ref(0);
const voted = ref(false);

const vote = (index) => {
  votes.value[index]++;
  totalVotes.value++;
  voted.value = true;
  console.log(votes.value);
};

const percentages = computed(() =>
  votes.value.map((vote) =>
    totalVotes.value ? ((vote / totalVotes.value) * 100).toFixed(1) : 0
  )
);
</script>

<style scoped>
.poll {
  max-width: 400px;
  margin: auto;
  text-align: center;
}

.poll-container {
  display: flex;
}

.poll-option {
  display: block;
  margin: 10px;
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  will-change: filter;
  transition: filter 300ms;
}
.poll-option:hover {
  background-color: #0056b3;
  filter: drop-shadow(0 0 2em #646cffaa);
}
.results {
  margin-top: 20px;
}
</style>
