<template>
  <div id="app" class="relative bg-black">
    <video class="w-screen h-screen" autoplay controls muted loop>
      <source
        src="http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4"
        type="video/mp4"
      />
    </video>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { gsap } from "gsap";

// constants
import { SPEED_BASE, SIZES_BASE, COLOR_LIST } from "@/constants";

// types
interface Message {
  id: number;
  message: string;
}

@Component
export default class App extends Vue {
  mounted(): void {
    new Array(20).fill(0).forEach((item, index) => {
      this.createText({
        id: index,
        message: `test${index}`,
      });
    });
  }

  async createText(item: Message): Promise<void> {
    const { clientWidth, clientHeight } = document.documentElement;
    const textEl = document.createElement("div");

    textEl.id = `text${item.id}`;
    textEl.appendChild(document.createTextNode(item.message));

    document.body.appendChild(textEl);

    textEl.style.position = "fixed";
    textEl.style.left = `${clientWidth}px`;
    textEl.style.top = `${Math.random() *
      (clientHeight - textEl.clientHeight)}px`;
    textEl.style.color = this.getRandomColor();
    textEl.style.fontSize = `${this.getRandomSize()}px`;
    textEl.style.fontWeight = "900";
    textEl.style.whiteSpace = "nowrap";

    await gsap.to(`#${textEl.id}`, {
      duration: this.getRandomSpeed(),
      x: -1 * (clientWidth + textEl.clientWidth),
    });

    textEl?.parentNode?.removeChild(textEl);
  }
  getRandomColor() {
    const index = this.getRandomInteger(COLOR_LIST.length - 1);

    return COLOR_LIST[index];
  }
  getRandomSize() {
    return SIZES_BASE + this.getRandomInteger(SIZES_BASE);
  }
  getRandomSpeed() {
    return SPEED_BASE + this.getRandomInteger(SPEED_BASE) / 2;
  }
  getRandomInteger(range: number) {
    return Math.round(Math.random() * range);
  }
}
</script>

<style lang="scss" scoped>
#app {
  div {
    transition: 0.5s;
  }
}
</style>
