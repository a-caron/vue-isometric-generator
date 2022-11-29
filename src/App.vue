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
      <div class="box-side" v-for="side in ['left','right','top']" :class="side" :key="side">
        <div class="box-face" v-for="face in 12" :key="face"></div>
      </div>
    </div>
  </main>
</template>

<script>
  export default {
    data() {
      return {
        width: 3,
        depth: 3,
        height: 3,
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

  :root {
    --background-color: #151515;
    --box-width: 16px;
    --box-height: 30px;
    --grid-border-width: 1px;
    --grid-border-color: #2c2c2c;
  }

  html, body {
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: sans-serif;
    color: white;
  }

  .form {
    --input-size: 40px;
    position: absolute;
    font-size: 15px;
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
      opacity: .65;
      font-weight: 100;
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
      background-color: rgba(white,.065);

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
          color: inherit;
          font-size: 1.2em;

          &.plus {
            grid-area: 1 / 3 / 2 / 4;
            background-color: rgba(white,.065);
          }

          &.minus {
            grid-area: 2 / 3 / 3 / 4;
            background-color: rgba(white,.13);
          }
        }
      }
    }
  }

  body {
    background-color: var(--background-color);
    display: flex;
  }

  main {
    display: flex;
    height: 100%;
  }

  .box {
    position: relative;

    &-side {
      position: absolute;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-gap: var(--grid-border-width);
      background: var(--grid-border-color);
      border: var(--grid-border-width) solid var(--grid-border-color);

      &.left {
        transform: perspective(700px) skew(0deg,25deg) translateX(-50%);
      }

      &.right {
        transform: perspective(700px) skew(0deg,-25deg) translateX(50%);
      }

      &.top {
        transform: perspective(700px) rotate(315deg) skew(20deg,20deg) scale(1.05) translate(calc(100% + 1px),calc(-100% - 1px));
      }
    }

    &-face {
      width: var(--box-width);
      height: var(--box-height);
      position: relative;
      background-color: var(--background-color);
      .top & { height: var(--box-width) }
    }
  }
</style>