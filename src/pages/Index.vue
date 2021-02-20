<template>
  <q-page class="column q-pa-lg no-scroll window-height no-wrap">
    <div class="row full-width q-col-gutter-md justify-center">
      here is the text we want to scroll
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
      id="prompt"
      ref="prompt"
      style="padding-bottom: 24px;"
      class="full-height overflow-auto text-h3 row q-mt-xl justify-center"
    >
      {{ text }}
    </div>
  </q-page>
</template>

<script>
import { gsap } from "gsap";
import ScrollToPlugin from "gsap/ScrollToPlugin";
import text from "../assets/text.js";
gsap.registerPlugin(ScrollToPlugin);

export default {
  name: "PageIndex",
  data() {
    return {
      scrollingInterval: null,
      speed: 4,
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
        this.speed += 1;
      }

      if (keyPressed === "ArrowDown") {
        this.speed = this.speed - 1 > 0 ? this.speed - 1 : 1;
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
        this.scrollingInterval = setInterval(this.scroll, 20);
      }
    },
    scroll() {
      if (
        this.$refs.prompt.scrollTop + this.$refs.prompt.clientHeight >=
        this.$refs.prompt.scrollHeight
      ) {
        this.$refs.prompt.scroll(0, 0);
      } else {
        const d = document.getElementById("prompt");
        gsap.to(d, {
          duration: 0.01,
          scrollTo: { y: this.$refs.prompt.scrollTop + this.speed, x: 0 }
        });
      }
    }
  }
};
</script>
