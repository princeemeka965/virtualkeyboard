<template>
  <div class="w-full flex justify-center p-3">
    <h1 class="text-5xl font-black">Virtual KeyBoard</h1>
  </div>
  <div class="flex w-full h-full flex-col justify-center p-10">
    <KeyBoard
      v-model="textAreaVal"
      :className="`use-keyboard-input w-1/3 h-40 border inset-11`"
    />
  </div>
</template>

<script>
import KeyBoard from "./components/KeyBoard.vue";

export default {
  components: {
    KeyBoard,
  },
  data() {
    return {
      textAreaVal: "",
    };
  },

  watch: {
    textAreaVal() {
      console.log(this.textAreaVal);
    },
  },

  methods: {},

  mounted() {},
};
</script>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
