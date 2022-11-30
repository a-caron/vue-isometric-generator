<template>

  <div class="center"/>

  <form class="form">
    <div class="form-group">
      <label class="form-label" v-text="'width'"/>
      <div class="form-input">
        <div class="form-input-block number" v-text="width"/>
        <button class="form-input-block button plus" @click.prevent="width++" v-text="'+'"/>
        <button class="form-input-block button minus" @click.prevent="width--" v-text="'-'"/>
      </div>
    </div>
    <div class="form-group">
      <label class="form-label" v-text="'depth'"/>
      <div class="form-input">
        <div class="form-input-block number" v-text="depth"/>
        <button class="form-input-block button plus" @click.prevent="depth++" v-text="'+'"/>
        <button class="form-input-block button minus" @click.prevent="depth--" v-text="'-'"/>
      </div>
    </div>
    <div class="form-group">
      <label class="form-label" v-text="'height'"/>
      <div class="form-input">
        <div class="form-input-block number" v-text="height"/>
        <button class="form-input-block button plus" @click.prevent="height++" v-text="'+'"/>
        <button class="form-input-block button minus" @click.prevent="height--" v-text="'-'"/>
      </div>
    </div>
  </form>

  <main>
    <div class="box">
      <div class="box-side" v-for="face in structure" :class="face.side" :key="face.side" :style="'grid-template-columns: repeat('+face.cols+', 1fr)'">
        <div class="box-block" v-for="(block, index) in face.count" :key="index" :class="{ 'last': face.count - index > face.count - face.cols }">
          <div v-if="face.side !== 'top'" class="box-block-col right"/>
          <div v-if="face.side !== 'top'" class="box-block-face"/>
          <div v-if="face.side !== 'top'" class="box-block-col left"/>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
  export default {
    data() {
      return {
        width: 4,
        depth: 2,
        height: 4
      }
    },
    computed: {
      structure() {
        return [
          {
            side: 'width',
            cols: this.width,
            count: this.width * this.height
          },
          {
            side: 'depth',
            cols: this.depth,
            count: this.depth * this.height
          },
          {
            side: 'top',
            cols: this.width,
            count: this.width * this.depth
          }
        ]
      }
    },
    watch: {
      width(value) {
        if (value <= 1) this.width = 1
        else if (value >= 6) this.width = 6
      },
      depth(value) {
        if (value <= 1) this.depth = 1
        else if (value >= 6) this.depth = 6
      },
      height(value) {
        if (value <= 1) this.height = 1
        else if (value >= 6) this.height = 6
      }
    }
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
      height: 1px;
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
    transform: scale(2);

    &-side {
      position: absolute;
      display: grid;
      grid-gap: var(--grid-border-width);
      background: $text-color;
      border: var(--grid-border-width) solid $text-color;

      &.width {
        right: 50%;
        transform-origin: 100% 0;
        transform: perspective(700px)
        skew(0deg,30deg);
      }

      &.depth {
        left: calc(50% - 1px);
        transform-origin: 0 0;
        transform: perspective(700px)
        skew(0deg,-30deg);
      }

      &.top {
        right: 50%;
        bottom: calc(100% - 1px);
        transform-origin: 100% 100%;
        transform: rotate(30deg)
        scaleX(1.16)
        skew(-26deg);
      }
    }

    &-block {
      width: var(--block-width);
      height: var(--block-height);
      position: relative;
      background-color: $background-color;

      .width &, .depth & {
        display: flex;
      }

      .top & {
        height: var(--floor-size);
        background-image: url('/images/floor.png');
        background-size: 100% 100%;
      }

      &-col {
        width: calc(var(--block-col-width) / 2);
        height: 100%;
        flex-shrink: 0;
        background-image: url('/images/column.png');
        background-size: 200% 100%;
        &.right { background-position: 0 0 }
        &.left { background-position: 100% 0 }
      }

      &-face {
        flex: 1;
        background-image: url('/images/face.png');
        background-size: 100% 100%;
      }
    }
  }
</style>