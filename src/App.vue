<template>
  <main>
    <div class="box">
      <div class="box-side" v-for="side in ['left','right','top']" :class="side" :key="side">
        <div class="box-face" v-for="face in 12" :key="face"></div>
      </div>
    </div>
  </main>
</template>

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
