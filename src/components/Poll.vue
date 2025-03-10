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

    <transition name="fade-slide">
      <div v-if="voted" class="results">
        <p v-for="(option, index) in options" :key="index">
          {{ option }}: {{ percentages[index] }}%
        </p>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

defineProps({
  question: { type: String, required: true },
  options: { type: Array, required: true },
});

const votes = ref(new Array(4).fill(0));
const totalVotes = ref(0);
const voted = ref(false);

const vote = (index) => {
  if (index < 0 || index >= 4) {
    console.error("Invalid vote index:", index);
    return;
  } else {
    votes.value[index]++;
    totalVotes.value++;
    voted.value = true;
  }
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
  transition: all 300ms ease;
}

.poll-option:hover {
  background-color: #0056b3;
  filter: drop-shadow(0 0 2em #646cffaa);
}

.results {
  margin-top: 20px;
}

.fade-slide-enter-active {
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.fade-slide-enter-from {
  opacity: 0;
  transform: translateY(20px);
}

.fade-slide-enter-to {
  opacity: 1;
  transform: translateY(0);
}
</style>
