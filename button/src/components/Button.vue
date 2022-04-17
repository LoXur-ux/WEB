<template>
  <div v-if="!isClick" class="back">
    <div class="start" @mouseup="click()">
      <p>START!</p>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Button',
  data: () => ({
    isClick: false,
  }),
  created() {
    this.isClick = false;
  },
  methods: {
    click() {
      this.isClick = true;
    },
  },
};
</script>

<style scoped>
.back {
  position: fixed;
  top: 0;
  left: 0;
  min-height: 100%;
  width: 100%;
  background: rgba(0, 0, 0, 0.75);

  overflow-y: hidden;
  overflow-x: hidden;
}

.start {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  height: 480px;
  width: 480px;
  border-radius: 50%;

  text-align: center;

  background: linear-gradient(
    149deg,
    rgb(241, 28, 28) 0%,
    rgb(224, 158, 16) 42%,
    rgb(233, 47, 47) 72%,
    rgb(218, 31, 140) 100%
  );
  animation: move 10s infinite linear;
  background-size: 400%;
}

.start:hover {
  background: linear-gradient(
    149deg,
    rgba(24, 187, 156, 1) 0%,
    rgba(106, 57, 175, 1) 42%,
    rgba(187, 24, 148, 1) 72%,
    rgba(115, 53, 134, 1) 100%
  );
  animation: move 10s infinite linear;
  background-size: 400%;
}

.start:active {
  background: linear-gradient(
    149deg,
    rgb(241, 167, 28) 0%,
    rgb(193, 202, 64) 42%,
    rgb(88, 151, 6) 72%,
    rgb(62, 175, 122) 100%
  );
  animation: move 10s infinite linear;
  background-size: 400%;
}

.start p {
  color: white;
  margin: 50% auto;
  font: 4em consolas;
  max-width: 50%;
  transform: translateY(-50%);
  user-select: none;
}

@keyframes move {
  0% {
    background-position: 80% 0%;
  }
  50% {
    background-position: 20% 100%;
  }
  100% {
    background-position: 80% 0%;
  }
}
</style>
