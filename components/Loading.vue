<template>
  <div>
    <progress-bar :progress="percentage"></progress-bar>
    <div class="loading">
      <img src="/images/cardBack.png" alt="logo" />
      <p>{{ percentage }}%</p>
    </div>
  </div>
</template>

<script>
import ProgressBar from '~/components/ProgressBar.vue';

export default {
  components: {
    ProgressBar
  },
  data() {
    return {
      percentage: 0
    };
  },
  mounted() {
    this.timer = setInterval(() => {
      this.percentage++;
      if (this.percentage === 100) {
        clearInterval(this.timer);
      }
    }, 20);
  }
};
</script>

<style scoped>
.loading {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  font-size: 2rem;
}
</style>