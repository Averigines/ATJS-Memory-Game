<template>
  <div class="progress">
    <div class="progress-bar" :style="{ width: progress + '%' }"></div>
  </div>
</template>

<script>
export default {
  props: {
    progress: {
      type: Number,
      required: true
    }
  }
};
</script>

<style>
.progress {
  height: 5px;
  width: 100%;
  background-color: #f2f2f2;
  margin-bottom: 10px;
}

.progress-bar {
  height: 100%;
  background-color: #4caf50;
}
</style>