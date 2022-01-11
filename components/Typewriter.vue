<template>

  <div class="pl-10">
      <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
    <span class="text-4xl text-black">
      {{ title }}
    </span>
    <span class="text-4xl text-bold text-red-500">
        {{ displayText.join("") }}
    </span>
  </div>
</template>

<script>
export default {
  props: {
    title: String,
    speed: {
      type: Number,
      default: 500
    },
    deleteSpeed: {
      type: Number,
      default: 166
    },
    words: {
      type: Array,
      default: []
    }
  },
  data() {
    return {
      displayText: [],
      currentWord: "",
      wordIdx: 0,
    };
  },
  computed: {
    // can adjust speeds based on other factors if needed
    TYPE_SPEED() { return this.speed } ,
    DELETE_SPEED(){ return this.deleteSpeed },
    timeoutSpeed() { return this.TYPE_SPEED * 1 },
  },
  mounted() {
    this.start();
  },
  methods: {
    start() {
      if (this.words && this.words.length > 0) {
        this.currentWord = this.words[this.wordIdx].split("");
        this.wordIdx++;
        this.animate = setTimeout(this.type, this.timeoutSpeed);
      }
    },
    type(word) {
      // if typing...
      if (this.currentWord.length > 0) {
        this.displayText.push(this.currentWord.shift());
        // if done typing, then delete
      } else if (this.currentWord.length === 0 && this.displayText.length > 0) {
        this.timeoutSpeed = this.DELETE_SPEED;
        this.displayText.pop();
        // if done typing & deleting
      } else if (
        this.currentWord.length === 0 &&
        this.displayText.length === 0
      ) {
        // change words
        if (this.wordIdx < this.words.length) {
          this.currentWord = this.words[this.wordIdx].split("");
          this.wordIdx++;
          this.timeoutSpeed = this.TYPE_SPEED;
          this.displayText.push(this.currentWord.shift());
        } else {
          // reset
          this.wordIdx = 0;
          this.currentWord = this.words[this.wordIdx].split("");
          this.displayText.push(this.currentWord.shift());
        }
      }
      setTimeout(this.type, this.timeoutSpeed);
    }
  }
}
</script>