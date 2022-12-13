<template>

  <!--<div class="center"/>-->

  <form class="form">
    <div class="form-group">
      <label class="form-label" v-text="'depth'"/>
      <div class="form-input">
        <div class="form-input-block number" v-text="houseDepth"/>
        <button class="form-input-block button plus" @click.prevent="houseDepth++" v-text="'+'"/>
        <button class="form-input-block button minus" @click.prevent="houseDepth--" v-text="'-'"/>
      </div>
    </div>
    <div class="form-group">
      <label class="form-label" v-text="'width'"/>
      <div class="form-input">
        <div class="form-input-block number" v-text="houseWidth"/>
        <button class="form-input-block button plus" @click.prevent="houseWidth++" v-text="'+'"/>
        <button class="form-input-block button minus" @click.prevent="houseWidth--" v-text="'-'"/>
      </div>
    </div>
    <div class="form-group">
      <label class="form-label" v-text="'height'"/>
      <div class="form-input">
        <div class="form-input-block number" v-text="houseHeight"/>
        <button class="form-input-block button plus" @click.prevent="houseHeight++" v-text="'+'"/>
        <button class="form-input-block button minus" @click.prevent="houseHeight--" v-text="'-'"/>
      </div>
    </div>
  </form>

  <main>
    <div class="box">
      <div class="box-side" v-for="face in structure" :class="face.side" :key="face.side" :style="face.style">
        <div class="box-block" v-for="(block, index) in face.count" :key="index" :class="{ 'last': face.count - index > face.count - face.cols }">
          <div v-if="face.side !== 'top' && face.side !== 'shadow'" class="box-block-col right"/>
          <div v-if="face.side !== 'top' && face.side !== 'shadow'" class="box-block-face"/>
          <div v-if="face.side !== 'top' && face.side !== 'shadow'" class="box-block-col left"/>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>

  import { ref, watch, computed } from 'vue'

  const houseDepth = ref(1)
  const houseWidth = ref(6)
  const houseHeight = ref(2)

  const structure = computed(() => {
    return [
      {
        side: 'left', cols: houseDepth.value,
        count: houseDepth.value * houseHeight.value,
        style: 'grid-template-columns: repeat('+houseDepth.value+', 1fr);'
             + 'right: calc(' + -houseWidth.value + ' * var(--block-width) - var(--grid-border-width));'
             + 'top: calc(' + -houseWidth.value + ' * (var(--block-height) / 2.6));'
      },
      {
        side: 'right', cols: houseDepth.value,
        count: houseDepth.value * houseHeight.value,
        style: 'grid-template-columns: repeat('+houseDepth.value+', 1fr);'
      },
      {
        side: 'front', cols: houseWidth.value,
        count: houseWidth.value * houseHeight.value,
        style: 'grid-template-columns: repeat('+houseWidth.value+', 1fr);'
      },
      {
        side: 'back', cols: houseWidth.value,
        count: houseWidth.value * houseHeight.value,
        style: 'grid-template-columns: repeat('+houseWidth.value+', 1fr);'
             + 'left: calc(' + -houseDepth.value + ' * var(--block-width) - var(--grid-border-width));'
             + 'transform-origin: calc(' + -houseDepth.value + ' * var(--block-width)) 0;'
      },
      {
        side: 'top', cols: houseDepth.value,
        count: houseDepth.value * houseWidth.value,
        style: 'grid-template-columns: repeat('+houseDepth.value+', 1fr);'
      },
      {
        side: 'shadow', cols: houseDepth.value,
        count: houseDepth.value * houseWidth.value,
        style: 'grid-template-columns: repeat('+houseDepth.value+', 1fr);'
             + 'bottom: calc(var(--block-height) * ' + -houseHeight.value + ')'
      }
    ]
  })

  watch(houseDepth, () => houseDepth.value = countMinMax(houseDepth.value, 1, 6))
  watch(houseWidth, () => houseWidth.value = countMinMax(houseWidth.value, 1, 6))
  watch(houseHeight, () => houseHeight.value = countMinMax(houseHeight.value, 2, 6))

  function countMinMax(value, min, max) {
    if (value <= 1) value = min
    else if (value >= max) value = max
    return value
  }
</script>

<style lang="scss">

  $background-color: #727272;
  $text-color: white;

  :root {
    --block-width: 20px;
    --block-col-width: 4px;
    --block-face-width: calc(var(--block-width) - var(--block-col-width));
    --block-height: 30px;
    --floor-size: var(--block-width);
    --grid-border-width: 0px;
  }

  body {
    background-color: $background-color;
    display: flex;
  }

  html, body {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: sans-serif;
    color: $text-color;
  }

  .center {
    pointer-events: none;
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    z-index: 1;
    opacity: .25;

    &:before, &:after {
      content: '';
      position: absolute;
      left: 0;
      width: 100%;
      top: 50%;
      transform: translateY(-50%);
      height: var(--grid-border-width);
      background-color: $text-color;
    }
    &:after {
      transform: translateY(-50%) rotate(90deg);
    }
  }

  .form {
    --input-size: 40px;
    position: absolute;
    font-size: 16px;
    left: 2em;
    top: 2em;

    &-group {
      display: flex;
      justify-content: space-between;
      &:not(:last-of-type) {
        margin-bottom: 1em;
      }
    }

    &-label {
      text-transform: capitalize;
      padding-right: 1em;
      display: flex;
      align-items: center;
      font-weight: 100;
      opacity: .85;
    }

    &-input {
      --size: 24px;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      width: calc(var(--size) * 3);
      grid-template-rows: repeat(2, 1fr);
      height: calc(var(--size) * 2);
      grid-column-gap: 0;
      grid-row-gap: 0;
      background-color: rgba($text-color,.06);

      &-block {
        display: flex;
        align-items: center;
        justify-content: center;

        &.number {
          grid-area: 1 / 1 / 3 / 3;
          font-size: 1.2em;
        }

        &.button {
          border: 0;
          font-size: 1.2em;
          color: rgba($text-color, .65);
          cursor: pointer;

          &.plus {
            grid-area: 1 / 3 / 2 / 4;
            background-color: rgba($text-color,.06);
          }

          &.minus {
            grid-area: 2 / 3 / 3 / 4;
            background-color: rgba($text-color,.12);
          }

          &:hover {
            background-color: rgba($text-color,.20);
            color: $text-color;
          }
        }
      }
    }
  }

  .box {
    position: relative;
    transform: scale(1.75);

    &-side {
      position: absolute;
      display: grid;
      grid-gap: var(--grid-border-width);
      border: var(--grid-border-width) solid $text-color;

      &.left, &.right {
        right: 50%;
        transform-origin: 100% 0;
        transform: perspective(700px)
        skew(0deg, 30deg);
      }

      &.right { filter: brightness(80%) }
      &.back, &.left { filter: brightness(70%) }
      &.left { z-index: -2 }

      &.front, &.back {
        left: calc(50% - var(--grid-border-width));
        transform-origin: 0 0;
        transform: perspective(700px)
        skew(0deg,-30deg);
      }

      &.back {
        z-index: -1;
      }

      &.top, &.shadow {
        transform-origin: 100% 100%;
        bottom: calc(100% - var(--grid-border-width));
        right: 50%;
        transform: rotate(30deg)
        scaleX(1.155)
        skew(-26.5deg);
      }

      &.top {
        bottom: calc(var(--floor-size) * -1.5);
        z-index: -1;
      }

      &.shadow {
        box-shadow: 0 0 7px 7px black, 0 0 20px 20px rgba(black,.7), 0 0 50px 50px rgba(black,.5);
        opacity: .12;
        z-index: -12;
      }
    }

    &-block {
      width: var(--block-width);
      height: var(--block-height);
      position: relative;
      background-color: $background-color;

      .left &, .front &, .back &, .right & {
        display: flex;
      }

      .top &, .shadow & {
        height: var(--floor-size);
      }

      .top & {
        background-image: url('/images/floor.png');
        background-size: 100% 100%;
      }

      .shadow & {
        background-color: black !important;
      }

      &.last {
        background-color: transparent;
      }

      &-col {
        width: calc(var(--block-col-width) / 2);
        height: 100%;
        flex-shrink: 0;
        background-size: 200% 100%;
        background-image: url('/images/column.png');
        .last & { background-image: url('/images/column-last.png') }
        &.right { background-position: 100% 0 }
        &.left { background-position: 0 0 }
      }

      &-face {
        flex: 1;
        background-size: 100% 100%;
        background-image: url('/images/face.png');
        .last & { background-image: url('/images/face-last.png') }
      }
    }
  }
</style>