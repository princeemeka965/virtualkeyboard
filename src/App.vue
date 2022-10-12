<template>
  <div class="w-full flex justify-center p-3">
    <h1 class="text-5xl font-black">Virtual KeyBoard</h1>
  </div>
  <div class="flex w-full h-full flex-col justify-center p-10">
    <textarea
      class="use-keyboard-input w-1/3 h-40 border inset-11"
      style="margin: 0 auto"
      ref="textarea"
      @focus="openKeyBoard = true"
      v-model="textAreaVal"
      @keyup="keyEvent($event)"
    ></textarea>

    <KeyBoard
      v-if="openKeyBoard"
      :input-props="this.externalKey"
      @closeKey="closeKeyBoard($event)"
      @handleKeyEvent="displayData($event)"
      @removeFocus="removeInputFocus($event)"
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
      openKeyBoard: false,
      textAreaVal: "",
      externalKey: "",
    };
  },

  methods: {
    removeInputFocus(value) {
      if (value) {
        this.$refs.textarea[0].blur();
      }
    },

    closeKeyBoard(data) {
      this.openKeyBoard = data;
    },

    displayData(data) {
      this.textAreaVal += data;
    },

    keyEvent(value) {
      if (value.key === " ") {
        this.externalKey = value.code.toLowerCase();
      } else {
        this.externalKey = value.key;
      }
    },
  },

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
