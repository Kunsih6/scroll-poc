<template>
  <q-page class="column q-pa-lg no-scroll window-height no-wrap">
    <div class="row full-width q-col-gutter-md justify-center">
      here is the text we want to scroll {{ speed }}
      <q-input
        v-model="text"
        class="full-width full-height"
        filled
        type="textarea"
      />
    </div>
    <div class="row q-mt-xl justify-center">
      <q-btn
        color="primary"
        size="lg"
        label="start scrolling"
        @click="startScrolling"
      />
    </div>
    <div
      ref="prompt"
      style="padding-bottom: 24px;"
      class="full-height overflow-auto text-h3 row q-mt-xl justify-center"
    >
      {{ text }}
    </div>
  </q-page>
</template>

<script>
import text from "../assets/text.js";

export default {
  name: "PageIndex",
  data() {
    return {
      scrollingInterval: null,
      speed: 1,
      text
    };
  },
  mounted() {
    document.addEventListener("keydown", this.onKeyPress, false);
  },
  methods: {
    onKeyPress(e) {
      const keyPressed = e.code;
      if (keyPressed === "ArrowUp") {
        this.speed = this.speed - 5 > 0 ? this.speed - 5 : 1;
      }

      if (keyPressed === "ArrowDown") {
        this.speed += 5;
      }

      if (keyPressed === "ArrowUp" || keyPressed === "ArrowDown") {
        e.preventDefault();
        e.stopPropagation();
        clearInterval(this.scrollingInterval);
        this.scrollingInterval = setInterval(this.scroll, this.speed);
      }
    },
    startScrolling() {
      if (this.scrollingInterval) {
        clearInterval(this.scrollingInterval);
        this.scrollingInterval = null;
      } else {
        this.scrollingInterval = setInterval(this.scroll, this.speed);
      }
    },
    scroll() {
      if (
        this.$refs.prompt.scrollTop + this.$refs.prompt.clientHeight >=
        this.$refs.prompt.scrollHeight
      ) {
        this.$refs.prompt.scroll(0, 0);
      } else {
        this.$refs.prompt.scroll({
          top: this.$refs.prompt.scrollTop + 1
        });
      }
    }
  }
};
</script>
