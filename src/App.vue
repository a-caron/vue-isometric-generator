<template>

  <form class="form">
    <div class="form-group">
      <label class="form-label">width</label>
      <div class="form-input">
        <div class="form-input-block number">{{ width }}</div>
        <button class="form-input-block button plus" @click.prevent="width++">+</button>
        <button class="form-input-block button minus" @click.prevent="width--">-</button>
      </div>
    </div>
    <div class="form-group">
      <label class="form-label">depth</label>
      <div class="form-input">
        <div class="form-input-block number">{{ depth }}</div>
        <button class="form-input-block button plus" @click.prevent="depth++">+</button>
        <button class="form-input-block button minus" @click.prevent="depth--">-</button>
      </div>
    </div>
    <div class="form-group">
      <label class="form-label">height</label>
      <div class="form-input">
        <div class="form-input-block number">{{ height }}</div>
        <button class="form-input-block button plus" @click.prevent="height++">+</button>
        <button class="form-input-block button minus" @click.prevent="height--">-</button>
      </div>
    </div>
  </form>

  <main>
    <div class="box">
      <div class="box-side" v-for="face in structure" :class="face.side" :key="face.side" :style="'grid-template-columns: repeat('+face.cols+', 1fr)'">
        <div class="box-face" v-for="face in face.count" :key="face.side"></div>
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
    --box-width: 16px;
    --box-height: 30px;
    --grid-border-width: 1px;
  }

  body {
    background-color: $background-color;
    display: flex;
  }

  main {
    display: flex;
    height: 100%;
  }

  html, body {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: sans-serif;
    color: $text-color;
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

          &.plus {
            grid-area: 1 / 3 / 2 / 4;
            background-color: rgba($text-color,.06);
          }

          &.minus {
            grid-area: 2 / 3 / 3 / 4;
            background-color: rgba($text-color,.12);
          }
        }
      }
    }
  }

  .box {
    position: relative;

    &-side {
      position: absolute;
      display: grid;
      grid-gap: var(--grid-border-width);
      background: $text-color;
      border: var(--grid-border-width) solid $text-color;

      &.width {
        right: 0;
        transform-origin: 100% 0;
        transform: perspective(700px)
        skew(0deg,30deg);
      }

      &.depth {
        left: -1px;
        transform-origin: 0 0;
        transform: perspective(700px)
        skew(0deg,-30deg);
      }

      &.top {
        right: 0;
        bottom: -1px;
        transform-origin: 100% 100%;
        transform: rotate(30deg)
        scaleX(1.16)
        skew(-26deg);
      }
    }

    &-face {
      width: var(--box-width);
      height: var(--box-height);
      position: relative;
      background-color: $background-color;
      .top & { height: var(--box-width) }
    }
  }
</style>