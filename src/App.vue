<script setup>
import { ref, onMounted } from 'vue'
import { ShoppingCart } from 'lucide-vue-next'

const companyName = ref('Amelia Aura')
const fullTitle = 'Coming Soon ... '
const title = ref('')
const message = ref(
  'Timeless watches and elegant jewelry curated for your unique style. Crafted with love, delivered with care.'
)

// Typewriter animation
onMounted(() => {
  let index = 0
  setInterval(() => {
    title.value += fullTitle[index]
    index++
    if (index >= fullTitle.length) {
      title.value = ''
      index = 0
    }
  }, 600)
})

const hourDeg = ref(0)
const minuteDeg = ref(0)
const secondDeg = ref(0)

// Returns Namibia time
const getNamibiaTime = () => {
  const now = new Date()
  const namibiaString = now.toLocaleString("en-US", { timeZone: "Africa/Windhoek" })
  return new Date(namibiaString)
}

const updateClock = () => {
  const now = getNamibiaTime()
  const hours = now.getHours()
  const minutes = now.getMinutes()
  const seconds = now.getSeconds()
  const ms = now.getMilliseconds()

  const smoothSeconds = seconds + ms / 1000

  hourDeg.value = (hours % 12) * 30 + (minutes / 60) * 30
  minuteDeg.value = minutes * 6 + (smoothSeconds / 60) * 6
  secondDeg.value = smoothSeconds * 6
}

onMounted(() => {
  updateClock()
  setInterval(updateClock, 50)
})
</script>

<template>
  <div class="main">

    <!-- LEFT BLUE SIDE -->
    <div class="color-figure">
      <div class="left-bottom">
        <div class="message-box">
          <p class="message">{{ message }}</p>
        </div>

        <div class="cart">
          <ShoppingCart size="50" color="#fff" />
        </div>
      </div>
    </div>

    <!-- RIGHT IMAGE + CLOCK -->
    <div class="hero-image">
      <div class="center-dot"></div>
      <div class="hand hour" :style="{ transform: `rotate(${hourDeg}deg)` }"></div>
      <div class="hand minute" :style="{ transform: `rotate(${minuteDeg}deg)` }"></div>
      <div class="hand second" :style="{ transform: `rotate(${secondDeg}deg)` }"></div>
    </div>

    <!-- TOP LEFT CONTENT -->
    <div class="content">
      <div class="name">{{ companyName }}</div>
      <div class="title">{{ title }}</div>
    </div>

  </div>
</template>

<style scoped>
/* ===== MAIN ===== */
.main {
  position: relative;
  height: 100vh;
  background-color: white;
}

/* ===== LEFT SIDE BACKGROUND ===== */
.color-figure {
  position: absolute;
  top: 0;
  left: 0;
  width: 40%;
  height: 100%;
  background-color: #001334;
  z-index: 1;
}

/* ===== BOTTOM-CENTER BOX (MESSAGE + CART) ===== */
.left-bottom {
  position: absolute;
  bottom: 40px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 50px; /* space between message and cart */
  z-index: 10;
}

/* ===== MESSAGE BOX ===== */
.message-box {
  border: 1px solid white;
  padding: 8px 30px;
  width: 100%;
  border-radius: 10px;
  box-shadow: white 1px 2px;
}

.message {
  font-family: "Montserrat", sans-serif;
  font-weight: 400; /* normal weight */
  text-align: justify;
  letter-spacing: 1px;
  line-height: 2;
  font-size: 10px;
  color: white;
}

/* ===== CART ICON ===== */
.cart {
  border: 1px solid white;
  border-radius: 50%;
  padding: 15px;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

/* ===== RIGHT IMAGE + CLOCK ===== */
.hero-image {
  position: absolute;
  top: 0;
  right: 0;
  width: 60%;
  height: 100%;
  background-image: url('@/assets/BLUE_WATCH.png');
  background-size: cover;
  background-position: center;
  z-index: 0;
}

/* ===== TEXT CONTENT ===== */
.content {
  position: relative;
  z-index: 5;
  padding: 50px 120px;
  height: 70vh;
}

.name {
  font-family: "Playfair Display", serif;
  font-weight: 400; /* light / normal */
  font-size: 64px;
  padding-top: 5%;
  line-height: 1.1;
  letter-spacing: 0.5px;
  color: white;
}

.title {
  font-family: "Montserrat", sans-serif;
  font-weight: 400;
  font-size: 30px;
  padding-left: 23%;
  letter-spacing: 1px;
  line-height: 1.1;
  color: white;
}

/* ===== CLOCK CENTER DOT ===== */
.center-dot {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background-color: #c6c8c7;
  transform: translate(-50%, -50%);
  z-index: 10;
}

/* ===== CLOCK HANDS ===== */
.hand {
  position: absolute;
  width: 50%;
  height: 2px;
  top: 50%;
  left: 50%;
  transform-origin: left center;
  background-color: #c6c8c7;
  transition: none;
}

.hand.hour {
  width: 12%;
  height: 5px;
  border-radius: 10% 30% 30% 10%;
}

.hand.minute {
  width: 15%;
  height: 4px;
  border-radius: 10% 50% 50% 10%;
}

.hand.second {
  width: 18%;
  height: 3px;
  border-radius: 10% 50% 50% 10%;
}
</style>
