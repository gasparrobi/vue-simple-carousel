<template>
  <div class="simple-carousel">
    <div class="carousel-inner">
      <div class="carousel-items" ref="carouselItems" :style="itemStyle">
        <slot></slot>
      </div>
    </div>
    <button
      v-if="isNavigationVisible"
      type="button"
      class="navigation navigation-left"
      :class="{ 'navigation-disabled': isFirstSlide }"
      :disabled="isFirstSlide"
      @click="slideLeft"
    >
      <svg
        width="11px"
        height="19px"
        viewBox="0 0 11 19"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        xmlns:sketch="http://www.bohemiancoding.com/sketch/ns"
        fill="#c6007e"
      >
        <g stroke="none" stroke-width="1" fill-rule="evenodd">
          <path
            d="M0.499,9.514 L9.276,18.497 L10.494,17.25 L2.92,9.499 L10.496,1.746 L9.277,0.499 L0.5,9.483 L0.514,9.498 L0.499,9.514 L0.499,9.514 Z"
          ></path>
        </g>
      </svg>
    </button>
    <button
      v-if="isNavigationVisible"
      type="button"
      class="navigation navigation-right"
      :class="{ 'navigation-disabled': isLastSlide }"
      :disabled="isLastSlide"
      @click="slideRight"
    >
      <svg
        width="11px"
        height="19px"
        viewBox="0 0 11 19"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        xmlns:sketch="http://www.bohemiancoding.com/sketch/ns"
        fill="#c6007e"
      >
        <g stroke="none" stroke-width="1" fill-rule="evenodd">
          <path
            d="M10.496,9.482 L1.719,0.499 L0.501,1.746 L8.075,9.497 L0.499,17.25 L1.718,18.497 L10.495,9.513 L10.481,9.498 L10.496,9.482 Z"
          ></path>
        </g>
      </svg>
    </button>
  </div>
</template>

<script>
import ResizeObserver from "resize-observer-polyfill";

export default {
  props: {
    height: {
      type: Number,
      required: true
    }
  },

  data: () => ({
    itemWidth: 0,
    carouselWidth: 0,
    currIndex: 0,
    observer: null,
    carouselElement: null
  }),

  computed: {
    itemStyle() {
      return {
        transform: `translateX(${Number(this.offset).toString()}px)`
      };
    },

    offset() {
      return -(this.currIndex * this.itemWidth);
    },

    isLastSlide() {
      return this.currIndex >= this.itemsCount() - this.visibleItemsCount;
    },

    isFirstSlide() {
      return this.currIndex <= 0;
    },

    visibleItemsCount() {
      return Math.round(this.carouselWidth / this.itemWidth);
    },

    isNavigationVisible() {
      return !this.isFirstSlide || !this.isLastSlide;
    }
  },

  methods: {
    slideLeft() {
      if (this.isFirstSlide) return;
      this.currIndex--;
      this.adjustPaddings();
    },

    slideRight() {
      if (this.isLastSlide) return;
      this.currIndex++;
      this.adjustPaddings();
    },

    handleWindowResize() {
      this.restoreStyling();
      this.carouselWidth = this.$refs.carouselItems.clientWidth;
      this.itemWidth = this.$slots.default[0].elm.clientWidth;
      this.adjustPaddings();
      this.adjustVisibility();
    },

    itemsCount() {
      return this.$slots.default.length;
    },

    adjustVisibility() {
      if (this.currIndex > this.itemsCount() - this.visibleItemsCount) {
        this.currIndex--;
        this.adjustVisibility();
      }
    },

    adjustPaddings() {
      this.$slots.default.forEach((el, index) => {
        if (this.currIndex === index) {
          el.elm.style.paddingLeft = "0";
        } else if (this.visibleItemsCount - 1 + this.currIndex === index) {
          el.elm.style.paddingRight = "0";
        } else {
          el.elm.removeAttribute("style");
        }
      });
    },

    restoreStyling() {
      this.$slots.default.forEach(el => el.elm.removeAttribute("style"));
    }
  },

  mounted() {
    this.$el.style.height = `${this.height}px`;
    this.carouselElement = this.$refs.carouselItems;
    this.observer = new ResizeObserver(this.handleWindowResize);
    this.observer.observe(this.carouselElement);
  },

  beforeDestroy() {
    this.observer.unobserve(this.carouselElement);
  }
};
</script>

<style lang="scss" scoped>
.simple-carousel {
  position: relative;
  width: 100%;

  .carousel-inner {
    display: flex;
    align-items: center;
    width: 100%;
    height: 100%;
    overflow: hidden;

    .carousel-items {
      display: flex;
      align-items: center;
      min-width: 100%;
      height: 100%;
      list-style: none;
      /* transition: transform 600ms cubic-bezier(0.86, 0, 0.07, 1); */
      transition: transform 600ms cubic-bezier(0.165, 0.84, 0.44, 1);
    }
  }

  .navigation {
    position: absolute;
    top: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background-color: #fff;
    cursor: pointer;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
    border: none;
    color: blue;
    transition: all 200ms;

    &-left {
      left: 0;
      transform: translateX(-50%) translateY(-50%);

      svg {
        margin-left: -10%;
      }
    }

    &-right {
      right: 0;
      transform: translateX(50%) translateY(-50%);
    }

    &-disabled {
      background-color: #eee;
      opacity: 0.5;
    }
  }
}
</style>