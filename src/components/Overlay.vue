<template>
  <Transition :appear="true" name="overlay-fade">
    <div
      v-show="show"
      class="overlay"
      @click="handleClose"
      @touchmove="preventPageSwipe"
    >
      <slot />
    </div>
  </Transition>
</template>

<script>
import { Transition } from "vue";

export default {
  name: "Overlay",
  props: {
    show: {
        type: Boolean,
        default: false,
        required: true
    },
    wrapperClosable: {
        type: Boolean,
        default: false, // Indicates whether user can close Drawer by clicking the overlay layer.
        required: false
    },
    dontPreventPageSwipe: {
      type: Boolean,
      default: false, // To prevent movement of page on swipe
      required: false,
    },
    close: {
        type: Function, // close drawer
        default: () =>{},
        required: true
    },
  },
  components: {
    Transition,
  },
  methods: {
    preventPageSwipe(e) {
      if (this.dontPreventPageSwipe) return;
      e.preventDefault();
      e.stopPropagation();
    },
    handleClose() {
        if(this.wrapperClosable) return;
        this.close()
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  z-index: 10000;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}

@keyframes overlay-fade-in {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

@keyframes overlay-fade-out {
  from {
    opacity: 1;
  }

  to {
    opacity: 0;
  }
}

.overlay-fade-enter-active {
  animation: 0.3s overlay-fade-in both ease-out;
}

.overlay-fade-leave-active {
  animation: 0.3s overlay-fade-out both ease-in;
}
</style>
