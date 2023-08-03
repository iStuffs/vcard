<script setup>
const clamp = (num, min, max) => Math.min(Math.max(num, min), max)

const debug = false
const target = ref(null)
const { width } = useElementSize(target)
const { isSupported, alpha, beta, gamma } = useDeviceOrientation()
const cardTransformation = computed(() => {
  if (!isSupported) return ''
  const MAX_RATATION = 30
  const rX =
    beta.value && clamp(beta.value.toFixed(2), -MAX_RATATION, MAX_RATATION)
  const rY =
    gamma.value && clamp(gamma.value.toFixed(2), -MAX_RATATION, MAX_RATATION)
  const rZ =
    alpha.value &&
    clamp(alpha.value.toFixed(2) - 180, -MAX_RATATION, MAX_RATATION)
  return `perspective(${
    width.value
  }px) rotateZ(${0}deg) rotateX(${rX}deg) rotateY(${-rY}deg)`
})

// const { elementX, elementY, isOutside, elementHeight, elementWidth } =
//   useMouseInElement(target);
//   const cardTransformation = computed(() => {
//   const MAX_RATATION = 30;
//   const rX = (
//     MAX_RATATION / 2 -
//     (elementY.value / elementHeight.value) * MAX_RATATION
//   ).toFixed(2);
//   const rY = (
//     (elementX.value / elementWidth.value) * MAX_RATATION -
//     MAX_RATATION / 2
//   ).toFixed(2);

//   return isOutside.value
//     ? ''
//     : `perspective(${elementWidth.value}px) rotateX(${rX}deg) rotateY(${rY}deg)`;
// });
</script>
<template>
  <div class="wrapper">
    <!-- <div class="info" v-if="debug">
      elementX: {{ elementX }}<br />
      elementY: {{ elementY }}<br />
      isOutside: {{ isOutside }}<br />
      elementHeight: {{ elementHeight }}<br />
      elementWidth: {{ elementWidth }}<br />
      cardTransformation: {{ cardTransformation }}<br />
    </div> -->
    <div class="info" v-if="debug">
      isSupported: {{ isSupported }}<br />
      alpha: {{ alpha }}<br />
      beta: {{ beta }}<br />
      gamma: {{ gamma }}<br />
      cardTransformation:<br />{{ cardTransformation }}
    </div>
    <header
      ref="target"
      :style="{
        transform: cardTransformation,
        transition: 'transform 0.25s ease-out',
      }"
    >
      <h1>Joachim RACZ</h1>
      <h2>Group Director</h2>
    </header>
    <main>
      <img
        src="qrcodes/joachim_racz.png"
        alt="vCard of Samuel DIDIER-LAURENT"
        class="qrcode"
      />
    </main>
    <footer class="footer">
      <img src="ageas_re.png" alt="ageas re" class="footer__logo" />
    </footer>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Heebo:wght@700&display=swap');
img {
  display: inline-block;
  max-width: 100%;
}

.info {
  position: fixed;
  padding: 1rem;
  top: 0;
  right: 0;
}

:root {
  --background: #5ec2cf;
  --color: #fffffe;
}

body {
  font-family: 'Heebo', -apple-system, blinkmacsystemfont, 'Segoe UI', 'Roboto',
    'Helvetica Neue', arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji',
    'Segoe UI Symbol';
  background-color: var(--background);
  color: var(--color);
  text-align: center;
}

.wrapper {
  margin-inline: auto;
  max-width: 375px;
  padding-inline: 2rem;
  padding-block: 112px;
}

header {
  margin: 0 0 48px 0;
}

h1 {
  font-size: 24px;
  margin: 0 0 24px 0;
}

h2 {
  font-size: 16px;
}

main {
  margin: 0 0 48px 0;
}
.qrcode {
  max-width: 176px;
}
.footer__logo {
  max-width: 176px;
}
</style>
