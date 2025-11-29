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

// Clock hands
const hourDeg = ref(0)
const minuteDeg = ref(0)
const secondDeg = ref(0)

// Get Namibia time reliably
const getNamibiaTime = () => {
  const now = new Date()
  const parts = new Intl.DateTimeFormat("en-US", {
    timeZone: "Africa/Windhoek",
    hour12: false,
    hour: "numeric",
    minute: "numeric",
    second: "numeric"
  }).formatToParts(now)

  let hours = 0, minutes = 0, seconds = 0
  parts.forEach(part => {
    if (part.type === 'hour') hours = parseInt(part.value)
    if (part.type === 'minute') minutes = parseInt(part.value)
    if (part.type === 'second') seconds = parseInt(part.value)
  })

  const ms = now.getMilliseconds()
  return { hours, minutes, seconds, ms }
}

// Update clock hands
const updateClock = () => {
  const { hours, minutes, seconds, ms } = getNamibiaTime()
  const smoothSeconds = seconds + ms / 1000

  hourDeg.value = (hours % 12) * 30 + (minutes / 60) * 30
  minuteDeg.value = minutes * 6 + (smoothSeconds / 60) * 6
  secondDeg.value = smoothSeconds * 6
}

onMounted(() => {
  updateClock()
  setInterval(updateClock, 50) // smooth movement
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
          <ShoppingCart size="30" color="#fff" />
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
/* ================= GLOBAL FIX ================ */
html, body {
  margin: 0;
  padding: 0;
  overflow: hidden; /* REMOVE SCROLLBAR */
  height: 100%;
}

/* ================= MAIN WRAPPER ================ */
.main {
  position: relative;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  background-color: white;
}

/* ================= LEFT BLUE SIDE ================ */
.color-figure {
  position: absolute;
  top: 0;
  left: 0;
  width: 40%;
  height: 100%;
  background-color: #001334;
  z-index: 1;
}

/* ================= BOTTOM CONTENT ON BLUE SIDE ================ */
.left-bottom {
  position: absolute;
  bottom: 40px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 50px;
  z-index: 10;
}

.message-box {
  border: 1px solid white;
  padding: 0.5rem 1rem;
  border-radius: 10px;
  box-shadow: white 1px 2px;
  max-width: 100%;
}

.message {
  font-family: "Montserrat", sans-serif;
  font-weight: 400;
  text-align: justify;
  color: white;
  font-size: clamp(0.7rem, 1vw, 1rem);
}

/* CART ICON */
.cart {
  border: 1px solid white;
  padding: 15px;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

/* ================= RIGHT HERO IMAGE + CLOCK ================ */
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

/* CLOCK DOT */
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

/* CLOCK HANDS */
.hand {
  position: absolute;
  top: 50%;
  left: 50%;
  transform-origin: left center;
  background-color: #c6c8c7;
}

.hand.hour {
  width: 12%;
  height: 5px;
}

.hand.minute {
  width: 15%;
  height: 4px;
}

.hand.second {
  width: 18%;
  height: 3px;
}

/* ================= TITLE + NAME ================== */
.content {
  position: relative;
  z-index: 5;
  padding: 50px 120px;
  height: 70vh;
}

.name {
  font-family: "Playfair Display", serif;
  font-weight: 400;
  font-size: clamp(2rem, 5vw, 4rem);
  padding-top: 5%;
  color: white;
}

.title {
  font-family: "Montserrat", sans-serif;
  font-weight: 300;
  font-size: clamp(1rem, 2.5vw, 2rem);
  padding-left: 23%;
  color: white;
}

/* ================= RESPONSIVE ================== */

/* -------- Tablets -------- */
@media (max-width: 1024px) {
  .color-figure {
    width: 45%;
  }
  .hero-image {
    width: 55%;
  }
}

/* -------- Mobile Phones -------- */
@media (max-width: 768px) {
  .main {
    display: flex;
    flex-direction: column;
    height: 100vh;
    padding: 0;
    margin: 0;
  }

  /* Blue section on top */
  .color-figure {
        position: relative;
    width: 100%;
    height: 50vh; /* slightly bigger for balance */
    margin: 0;
    padding: 0;
  }

  /* IMAGE NOW APPEARS ! */
  .hero-image {
   position: relative;
    width: 100%;
    height: 65vh;
    margin: 0;
    padding: 0;
    background-size: cover;
    background-position: center;
  }

  /* Text content */
  .content {
     position: absolute;
    top: 0;
    width: 50%;
    z-index: 20; /* bring title + name to front */
    text-align: center;
  }
   .name {
    color: white;
  }

  .title {
    color: white;
    padding-left: 0;
  }

  .left-bottom {
    bottom: 20px;
    gap: 1rem;
  }
}

</style>