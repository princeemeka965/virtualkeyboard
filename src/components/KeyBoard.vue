<template>
  <div class="w-full flex">
    <textarea
      :class="`${className}`"
      style="margin: 0 auto"
      ref="textarea"
      @focus="openKeyBoard = true"
      v-model="textAreaVal"
      @keyup="keyEvent($event)"
    ></textarea>

    <div class="fixed left-0 bottom-0 w-full p-2 keyboard" v-if="openKeyBoard">
      <div class="flex w-full justify-center">
        <div class="w-3/5">
          <div class="text-center">
            <button
              v-for="(keyData, index) in keysElem"
              :key="index"
              class="keyboard__key text-white outline-none cursor-pointer inline-flex items-center justify-center"
              :class="setKeyClass(keyData)"
              :ref="keyData"
              :title="`${keyData}`"
              @click="handleClick(keyData)"
            >
              <i
                v-if="iconKeys.includes(keyData)"
                :class="
                  keyData === 'Windows' ? 'fa fa-windows' : 'material-icons'
                "
              >
                {{ keyData !== "Windows" ? setIcons(keyData) : "" }}
              </i>
              <span v-else>
                {{ keyData }}
              </span>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/**
 * KeysElem Array displays ALL keys in the virtual KeyBoard.
 */

/**
 * SpecialKeys Array displays keys that are not affected by any
 * action carried out on the virtual KeyBoard.
 */

/**
 * IconKeys Array displays keys that require icons to be displayed.
 */

/**
 * NumberKeys Array displays keys that will be REPLACED by SymbolKeys Array
 * when Shift is Active.
 */

export default {
  props: {
    className: {
      type: String,
      default: "",
    },
  },

  data() {
    return {
      textAreaVal: "",
      openKeyBoard: false,
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
        "Backspace",
        "Tab",
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
        "CapsLock",
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
        "Enter",
        "Close",
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
        "?",
        "Shift",
        "Ctrl",
        "Windows",
        "Space",
        "Alt",
        "Ctrl",
      ],
      specialKeys: [
        "Backspace",
        "CapsLock",
        "Tab",
        "Enter",
        "Close",
        "Shift",
        "Space",
        "Windows",
        "Alt",
        "Ctrl",
      ],
      iconKeys: [
        "Backspace",
        "CapsLock",
        "Tab",
        "Enter",
        "Close",
        "Shift",
        "Space",
        "Windows",
      ],
      numberKeys: [
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
        "[",
        "]",
        ";",
        "#",
        "'",
        ",",
        ".",
      ],
      symbolKeys: [
        "!",
        "''",
        "£",
        "$",
        "%",
        "^",
        "&",
        "*",
        "(",
        ")",
        "_",
        "+",
        "{",
        "}",
        ":",
        "`",
        "@",
        "<",
        ">",
      ],
    };
  },

  watch: {
    capsLock() {
      this.capsLock
        ? this.$refs.CapsLock[0].classList.add("keyboard__key--active")
        : this.$refs.CapsLock[0].classList.remove("keyboard__key--active");

      /**
       * Convert Lower case to upper case if capsLock is on
       * Convert Upper case to lower case when capsLock is off
       * S
       */

      const upper = this.keysElem.map((element) => {
        if (!this.specialKeys.includes(element)) {
          return element.toUpperCase();
        } else {
          return element;
        }
      });

      const lower = this.keysElem.map((element) => {
        if (!this.specialKeys.includes(element)) {
          return element.toLowerCase();
        } else {
          return element;
        }
      });

      this.capsLock ? (this.keysElem = upper) : (this.keysElem = lower);
    },

    shiftKey() {
      this.shiftKey
        ? this.$refs.Shift[0].classList.add("keyboard__key--active")
        : this.$refs.Shift[0].classList.remove("keyboard__key--active");

      /**
       * Convert Numbers to Symbols if shiftKey is on
       * Convert Symbols back to Numbers when shiftKey is off
       */

      const symbols = this.keysElem.map((element) => {
        if (this.numberKeys.includes(element)) {
          return this.symbolKeys[this.numberKeys.indexOf(element)];
        } else {
          return element;
        }
      });

      const numbers = this.keysElem.map((element) => {
        if (this.symbolKeys.includes(element)) {
          return this.numberKeys[this.symbolKeys.indexOf(element)];
        } else {
          return element;
        }
      });

      this.shiftKey ? (this.keysElem = symbols) : (this.keysElem = numbers);
      this.capsLock = this.shiftKey;
    },
  },

  methods: {
    setKeyClass(keys) {
      switch (keys) {
        case "Tab":
          return "keyboard__key--wide";

        case "Shift":
          return "keyboard__key--wide keyboard__key--activatable";

        case "Backspace":
          return "keyboard__key--wide";

        case "CapsLock":
          return "keyboard__key--wide keyboard__key--activatable";

        case "Enter":
          return "keyboard__key--wide";

        case "Space":
          return "keyboard__key--extra-wide";

        case "Close":
          return "keyboard__key--wide";

        default:
          return "";
      }
    },

    setIcons(keys) {
      switch (keys) {
        case "Tab":
          return "keyboard_tab";

        case "Shift":
          return "arrow_upward";

        case "Backspace":
          return "backspace";

        case "CapsLock":
          return "lock";

        case "Enter":
          return "keyboard_return";

        case "Space Bar":
          return "space_bar";

        case "Close":
          return "close";

        default:
          return "";
      }
    },

    handleClick(keyData) {
      switch (keyData) {
        case "Tab":
          this.$refs.Tab[0].blur();
          break;

        case "Close":
          // Closes virtual KeyBoard
          this.openKeyBoard = false;
          // Remove CapsLock, if on
          this.capsLock = false;
          // Remove Shift, if on
          this.shiftKey = false;
          break;

        case "Enter":
          // Enters a new line
          this.setInputData("\n");
          this.$refs.Enter[0].blur();
          break;

        case "Space":
          this.setInputData(" ");
          this.$refs.Space[0].blur();
          break;

        case "CapsLock":
          this.capsLock = !this.capsLock;
          this.$refs.CapsLock[0].blur();
          break;

        case "Shift":
          this.shiftKey = !this.shiftKey;
          this.$refs.Shift[0].blur();
          break;

        case "Backspace":
          this.deleteInputData();
          this.$refs.Backspace[0].blur();
          break;

        default:
          this.setInputData(keyData);
          break;
      }

      // Focus back to the textarea field after every action taken
      this.$refs.textarea.focus();
    },

    setInputData(data) {
      if (data !== "Windows" && data !== "Alt" && data !== "Ctrl") {
        this.textAreaVal += data;
      }
    },

    deleteInputData() {
      this.textAreaVal = this.textAreaVal.substring(
        0,
        this.textAreaVal.length - 1
      );
    },

    keyEvent(value) {
      // Captures when keys from external Keyboard (Device Key) are clicked
      // value.key === " " captures when space Bar is clicked
      const refNode = value.key === " " ? value.code : value.key;
      this.highlightKey(refNode);
    },

    highlightKey(node) {
      // Highlight the key on the virtual KeyBoard
      this.$refs[`${node}`][0].focus();

      // Remove focus on the key after 1 milliseconds
      // Place focus back to the textarea field
      setTimeout(() => {
        this.$refs[`${node}`][0].blur();
        this.$refs.textarea.focus();
      }, 100);

      node === "CapsLock" ? (this.capsLock = !this.capsLock) : "";
      node === "Shift" ? (this.shiftKey = !this.shiftKey) : "";
    },
  },

  mounted() {},
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
  margin: 0.2%;
  border-radius: 4px;
  border: none;
  background: rgba(255, 255, 255, 0.2);
  font-size: 1.05rem;
  vertical-align: top;
}

.keyboard__key:active {
  background: rgba(255, 255, 255, 0.12);
}

.keyboard__key:focus {
  background: rgba(255, 255, 255, 0.12);
}

.keyboard__key--wide {
  width: 12.5%;
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
