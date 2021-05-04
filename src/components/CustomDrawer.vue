<template>
  <div>
    <Overlay :show="show" :close="close" />
    <Transition :appear="true" :name="'drawer-slide-' + direction">
      <div
        v-show="show"
        id="drawer-container"
        :class="['container', { fullScreen: fullScreen, moving: moving }]"
        @touchstart="touchStart"
        @touchend="touchEnd"
        @touchmove="touchMove"
        :style="containerStyle"
      >
        TEST
        <slot />
      </div>
    </Transition>
  </div>
</template>

<script>
import { Transition } from "vue";
import Overlay from "../components/Overlay.vue";

export default {
  name: "CustomDrawer",
  components: {
    Transition,
    Overlay,
  },
  props: {
    direction: {
      type: String,
      default: "bottom",
    },
    show: {
      type: Boolean,
      default: false,
      required: true,
    },
    fullScreen: {
      type: Boolean,
      default: false,
      required: false,
    },
    disableTouchAction: {
      type: Boolean,
      default: false, // disable native like touch action over the drawer
      required: false,
    },
    style: {
      type: Object,
      default: () => {},
      required: false,
    },
    close: {
      type: Function,
      default: () => {},
      required: true,
    },
  },
  data: function () {
    return {
      YState: {
        value: 0,
        initial: 0,
        moving: false,
      },
    };
  },
  computed: {
    moving: function () {
      return this.YState.moving;
    },
    containerStyle: function () {
      let style = {}
      if (
        this.YState.value > 0 &&
        !this.disableTouchAction &&
        this.YState.initial !== 0
      ) {
        style = {
          ...style,
          transform: `translateY(${this.YState.value}px)`,
        };
      }

      if (this.style) {
        style = {
          ...style,
          ...this.style,
        };
      }

      return style;
    },
  },
  methods: {
    touchStart(e) {
      if (this.disableTouchAction) return;
      this.YState.initial = e.changedTouches[0].clientY;
    },
    touchEnd() {
      console.log("end", this.YState.value);
      if (this.disableTouchAction) return;
      this.YState.moving = false;
      const parentElDimension = document
        .getElementById("drawer-container")
        .getBoundingClientRect();
      if (this.YState.value > parentElDimension.height * 0.7) {
        this.close();
      }
      this.YState.initial = 0;
      this.YState.value = 0;
    },
    touchMove(e) {
      if (this.disableTouchAction) return;
      if (e.cancelable) {
        e.stopPropagation();
        e.preventDefault();
      }

      this.YState.moving = true;
      const newYVal = e.changedTouches[0].clientY - this.YState.initial;
      if (newYVal >= 0) {
        this.YState.value = newYVal;
      } else {
        this.YState.value = 0;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  position: fixed;
  bottom: 0;
  max-height: 100%;
  right: 0;
  left: 0;
  margin-right: auto;
  margin-left: auto;
  width: 100%;
  max-width: 720px;
  background-color: #fff;
  overflow-y: auto;
  border-radius: 18px 18px 0 0;
  z-index: 111111;
  transition: transform 0.3s;
  -webkit-overflow-scrolling: touch;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
.moving {
  transition: none;
}

.fullscreen {
  height: 100%;
}

.drawer-slide-bottom-enter-active {
  transition-timing-function: ease-out;
}
.drawer-slide-bottom-leave-active {
  transition-timing-function: ease-in;
}
.drawer-slide-bottom-enter-from,
.drawer-slide-bottom-leave-active {
  transform: translate3d(0, 100%, 0);
}

.drawer-slide-left-enter-active {
  transition-timing-function: ease-out;
}
.drawer-slide-left-leave-active {
  transition-timing-function: ease-in;
}
.drawer-slide-left-enter-from,
.drawer-slide-left-leave-active {
  transform: translate3d(100%, 0, 0);
}
</style>
