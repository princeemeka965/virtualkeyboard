<template>
  <div class="fixed left-0 bottom-0 w-full p-2 keyboard">
    <div class="flex w-full justify-center">
      <div class="w-3/5">
        <div class="text-center">
          <button
            v-for="(keyData, index) in keysElem"
            :key="index"
            class="keyboard__key text-white outline-none cursor-pointer inline-flex items-center justify-center"
            :class="setKeyClass(keyData)"
            :ref="keyData"
            @click="handleClick(keyData)"
          >
            {{ keyData }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      capsLock: false,
      shiftKey: false,
      keysElem: [
        "~",
        "1",
        "2",
        "3",
        "4",
        "5",
        "6",
        "7",
        "8",
        "9",
        "0",
        "-",
        "=",
        "backspace",
        "tab",
        "q",
        "w",
        "e",
        "r",
        "t",
        "y",
        "u",
        "i",
        "o",
        "p",
        "[",
        "]",
        "#",
        "caps",
        "a",
        "s",
        "d",
        "f",
        "g",
        "h",
        "j",
        "k",
        "l",
        ";",
        "'",
        "enter",
        "done",
        "z",
        "x",
        "c",
        "v",
        "b",
        "n",
        "m",
        ",",
        ".",
        "/",
        "question",
        "shift",
        "space",
      ],
      specialKeys: [
        "backspace",
        "caps",
        "tab",
        "enter",
        "done",
        "question",
        "shift",
        "space",
      ],
      numberKeys: ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "-", "="],
      symbolKeys: ["!", "''", "£", "$", "%", "^", "&", "*", "(", ")", "_", "+"],
    };
  },

  watch: {
    capsLock() {
      this.capsLock
        ? this.$refs.caps[0].classList.add("keyboard__key--active")
        : this.$refs.caps[0].classList.remove("keyboard__key--active");

      /**
       * Convert Lower case to upper case if capsLock is on
       * Convert Upper case to lower case when capsLock is off
       */

      const upper = this.keysElem.map((element) => {
        if (!this.specialKeys.includes(element)) {
          return element.toUpperCase();
        } else {
          return element.toLowerCase();
        }
      });

      const lower = this.keysElem.map((element) => {
        return element.toLowerCase();
      });

      this.capsLock ? (this.keysElem = upper) : (this.keysElem = lower);
    },

    shiftKey() {
      this.shiftKey
        ? this.$refs.shift[0].classList.add("keyboard__key--active")
        : this.$refs.shift[0].classList.remove("keyboard__key--active");

      /**
       * Convert Lower case to upper case if shiftKey is on
       * Convert Upper case to lower case when shiftKey is off
       */

      const upper = this.keysElem.map((element, index) => {
        if (this.numberKeys.includes(element)) {
          return this.symbolKeys[index - 1];
        } else {
          return element.toLowerCase();
        }
      });

      const lower = this.keysElem.map((element, index) => {
        if (this.symbolKeys.includes(element)) {
          return this.numberKeys[index - 1];
        } else {
          return element.toLowerCase();
        }
      });

      this.shiftKey ? (this.keysElem = upper) : (this.keysElem = lower);
      this.capsLock = this.shiftKey;
    },
  },

  methods: {
    setKeyClass(keys) {
      switch (keys) {
        case "tab":
          return "keyboard__key--wide";

        case "shift":
          return "keyboard__key--wide keyboard__key--activatable";

        case "backspace":
          return "keyboard__key--wide";

        case "caps":
          return "keyboard__key--wide keyboard__key--activatable";

        case "enter":
          return "keyboard__key--wide";

        case "space":
          return "keyboard__key--extra-wide";

        case "done":
          return "keyboard__key--wide";

        default:
          return "";
      }
    },

    handleClick(keyData) {
      switch (keyData) {
        case "tab":
          this.$emit("removeFocus", true);
          break;

        case "done":
          this.$emit("closeKey", false);
          break;

        case "caps":
          this.capsLock = !this.capsLock;
          break;

        case "shift":
          this.shiftKey = !this.shiftKey;
          break;

        default:
          this.$emit("handleKeyEvent", keyData);
          break;
      }
    },
  },

  mounted() {
    this.$refs.backspace[0].innerHTML = `<i class="material-icons">backspace</i>`;
    this.$refs.tab[0].innerHTML = `<i class="material-icons">keyboard_tab</i>`;
    this.$refs.enter[0].innerHTML = `<i class="material-icons">keyboard_return</i>`;
    this.$refs.question[0].innerHTML = `?`;
    this.$refs.shift[0].innerHTML = `<i class="material-icons" title="Shift">arrow_upward</i>`;
    this.$refs.caps[0].innerHTML = `<i class="material-icons">lock</i>`;
    this.$refs.done[0].innerHTML = `<i class="material-icons" title="Close Keyboard">close</i>`;
    this.$refs.space[0].innerHTML = `<i class="material-icons">space_bar</i>`;
  },
};
</script>

<style>
.keyboard {
  background: rgb(0, 0, 0);
  box-shadow: 0 0 50px rgba(0, 0, 0, 0.5);
  user-select: none;
  transition: bottom 0.4s;
}

.keyboard__key {
  height: 45px;
  width: 6%;
  max-width: 90px;
  margin: 3px;
  border-radius: 4px;
  border: none;
  background: rgba(255, 255, 255, 0.2);
  font-size: 1.05rem;
  vertical-align: top;
}

.keyboard__key:active {
  background: rgba(255, 255, 255, 0.12);
}

.keyboard__key--wide {
  width: 12%;
}

.keyboard__key--extra-wide {
  min-width: 70%;
}

.keyboard__key--activatable::after {
  content: "";
  width: 8px;
  height: 8px;
  margin-left: 9px;
  background: rgba(0, 0, 0, 0.4);
  border-radius: 50%;
}

.keyboard__key--active::after {
  background: #08ff00;
}

.keyboard__key--dark {
  background: rgba(0, 0, 0, 0.25);
}
</style>