<template>
  <div class="main-wrapper">
    <div class="content">
      <Title />
      <transition-group
        tag="ul"
        name="slide-in"
        class="select-container"
        appear
        :style="{ '--total': imagesArr.length }"
      >
        <SingleDesire
          v-for="(img, i) in imagesArr"
          :key="img.id"
          :id="img.id"
          :src="img.src"
          :title="img.title"
          v-model="desire"
          v-on:input="$emit('selected-desire', desire)"
          :style="{ '--i': i }"
        />
      </transition-group>
      <transition name="slide" v-on:afterLeave="afterLeave">
        <div class="next-step" v-on:click="nextStep" v-if="!isSelected">
          <span class="next-step-text" :class="{ active: desire.length > 0 }"
            >Next</span
          >
          <RightIcon
            class="next-step-arrow"
            :class="{ active: desire.length > 0 }"
          />
        </div>
      </transition>

      <NextButton
        :isSelected="isSelected"
        v-on:next-step="nextStep"
        :desire="desire"
      />
    </div>
  </div>
</template>

<script>
import SingleDesire from './SingleDesire';
import NextButton from './NextButton';
import Title from './Title';

import RightIcon from '../../../../icons/Right';

import { images } from './images';

export default {
  name: 'SelectDesire',
  props: ['setDesire', 'desire'],
  data() {
    return {
      isSelected: false,
    };
  },

  methods: {
    nextStep() {
      this.$emit('stage-change', 'guide');
      this.isSelected = true;
    },

    afterLeave() {
      this.$emit('next-step', '');
    },
  },
  computed: {
    imagesArr() {
      return images;
    },
  },

  components: {
    SingleDesire,
    Title,
    NextButton,
    RightIcon,
  },
};
</script>

<style lang="scss" scoped>
.main-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  background: #353839;
  @media (min-width: $tablet) {
    overflow: hidden;
  }
}

.content {
  overflow: hidden;

  width: 100%;
  height: 100%;
  margin: 0 auto;
  padding: 30px 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: scroll;
  scrollbar-width: none;

  ::-webkit-scrollbar {
    display: none;
  }

  @media (min-width: $mobileL) {
    width: 80%;
    padding: 40px 0px;
  }

  @media (min-width: $tablet) {
    padding: 40px 20px;
    width: 80%;
  }

  @media (min-width: $laptop) {
    width: 70%;
    margin: 0 auto;
    overflow: hidden;
  }
}
.select-container {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  width: 100%;
  height: 100%;
  flex-wrap: wrap;
  list-style-type: none;
  padding-bottom: 40px;
  align-content: center;

  @media (min-width: $laptop) {
    padding: 30px 50px;
    overflow: hidden;
  }
}
.next-step {
  display: none;
  @media (min-width: $laptop) {
    display: inline-block;
    position: fixed;
    right: 0;
    top: 50%;
    transform: translate(0, -50%);
    display: flex;
    justify-content: center;
    align-items: center;
    opacity: 1;
    &:hover > .next-step-text {
      transform: translateX(10px);
    }
  }
  &.slide {
    transform: translate(100%, -50%);
    opacity: 0;
  }

  & > .next-step-text {
    font-size: 40px;
    font-weight: 600;
    color: #202020;
    color: #ff5185;
    display: inline-block;
    position: relative;
    opacity: 0;
    right: -30px;
    padding-right: 0px;
    font-style: italic;
    transition: all 0.4s;

    &.active {
      padding-right: 20px;
      opacity: 1;
      cursor: pointer;
    }
  }

  & > .next-step-arrow {
    display: inline-block;

    width: 120px;
    height: 120px;

    fill: #202020;
    opacity: 0.2;

    transition: all 0.3s;

    &.active {
      opacity: 1;
      cursor: pointer;
      fill: #ff5185;
    }
  }
}
/* .question {
  font-size: 39px;
  color: #ff5185;

  padding: 10px 15px 30px;

  text-shadow: 0px 4px 3px rgba(0, 0, 0, 0.4), 0px 8px 13px rgba(0, 0, 0, 0.1),
    0px 18px 23px rgba(0, 0, 0, 0.1);

  @media (min-width: $mobileM) {
    font-size: 41px;
    padding-bottom: 35px;
  }
  @media (min-width: $mobileL) {
    font-size: 45px;
    padding-bottom: 40px;
  }

  @media (min-width: $laptop) {
    font-size: 55px;
    padding-bottom: 20px;
  }

  @media (min-width: $laptopL) {
    font-size: 55px;
    padding-bottom: 40px;
  }
} */

.slide-leave-active {
  transform: translate(100%, -50%);
  opacity: 0;
  transition: all 0.2s cubic-bezier(0.17, 0.67, 1, 1.23);
}
.slide-leave {
  opacity: 1;
  transform: translate(0, -50%);
}

.slide-in {
  &-move {
    transition: opacity 0.5s linear, transform 0.5s ease-in-out;
  }

  &-leave-active {
    transition: opacity 0.4s linear,
      transform 0.4s cubic-bezier(0.5, 0, 0.7, 0.4); //cubic-bezier(.7,0,.7,1);
    transition-delay: calc(0.3s * (var(--total) - var(--i)));
  }

  &-enter-active {
    transition: opacity 0.5s linear,
      transform 0.5s cubic-bezier(0.2, 0.5, 0.1, 1);
    transition-delay: calc(0.3s * var(--i));
  }

  &-enter,
  &-leave-to {
    opacity: 0;
  }

  &-enter {
    transform: translateY(40px);
  }
  &-leave-to {
    transform: translateX(0);
  }
}
</style>
