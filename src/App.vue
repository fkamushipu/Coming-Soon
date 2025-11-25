<script setup>
import { ref, onMounted } from 'vue'
import { ShoppingCart } from 'lucide-vue-next'
const companyName = ref('Amelia Aura')
const fullTitle = 'Coming Soon'
const title = ref('')
const message = ref(
  'Timeless watches and elegant jewelry curated for your unique style. Crafted with love, delivered with care.',
)
onMounted(() => {
  let index = 0
  const interval = setInterval(() => {
    title.value += fullTitle[index]
    index++
    if (index > fullTitle.length) {
      title.value = ''
      index = 0
    }
  }, 300)
})

const hourDeg = ref(0)
const minuteDeg = ref(0)
const secondDeg = ref(0)
const namibiaTime = ref("00:00:00")

const updateClock = () => {
  const now = new Date()  // Use your system time directly (Namibian time)

  const hours = now.getHours()
  const minutes = now.getMinutes()
  const seconds = now.getSeconds()
  const ms = now.getMilliseconds()

  // Smooth seconds
  const smoothSeconds = seconds + ms / 1000

  // Analog hand rotation
  hourDeg.value = (hours % 12) * 30 + (minutes / 60) * 30
  minuteDeg.value = minutes * 6 + (smoothSeconds / 60) * 6
  secondDeg.value = smoothSeconds * 6

  // Digital time (24-hr format)
  namibiaTime.value = now.toLocaleTimeString("en-GB", {
    hour: "2-digit",
    minute: "2-digit",
    second: "2-digit",
    hour12: false,
  })
}


onMounted(() => {
  updateClock()
  setInterval(updateClock, 50) // <-- this keeps the clock moving
})

</script>

<template>
  <div class="main">
    <div class="color-figure"></div>
    <div class="hero-image">
      <div class="center-dot"></div>
      <div class="hand hour" :style="{ transform: `rotate(${hourDeg}deg)` }"></div>
      <div class="hand minute" :style="{ transform: `rotate(${minuteDeg}deg)` }"></div>
      <div class="hand second" :style="{ transform: `rotate(${secondDeg}deg)` }"></div>
    </div>
    <div class="content">
      <h1 class="name">{{ companyName }}</h1>
      <h5 class="title">{{ title }}</h5>
      <div class="message-box">
        <p class="message">{{ message }}</p>
      </div>
      <div class="cart"><ShoppingCart size="50" color="#000" /></div>
    </div>
  </div>
</template>

<style scoped>
.main {
  position: relative;
  height: 100vh;
  background-color: white;
}
.color-figure {
  position: absolute;
  top: 0;
  left: 0;
  width: 40%;
  height: 100%;
  background-color: #cd602f;
  z-index: 1;
}

.hero-image {
  position: absolute;
  top: 0;
  left: 20%;
  width: 100%;
  height: 100%;
  background-image: url('@/assets/picture3.png');
  background-size: cover;
  background-position: center;
  z-index: 0;
}
.content {
  position: relative;
  z-index: 5;
  padding: 50px 120px;
  height: 70vh;
}

.name {
  font-family: "Italiana", sans-serif;
  font-size: 64px;
  padding-left: 15%;
  line-height: 0%;
}

.title {
  font-size: 35px;
  font-family: poppins; 
  padding-left: 30%;
  letter-spacing: 2px;
  line-height: 0%;
  z-index: 5;
}

.message-box {
  border: 1px solid #948c8c;
  padding: 8px 16px;
  width: 20%;
  border-radius: 10px;
  position: absolute;
  bottom: 0;
  z-index: 5;
}
.message {
  font-family: monospace;
  text-align: justify;
  letter-spacing: 2px;
  line-height: 2;
  color: white;
}
.cart {
  border: 1px solid #cd602f;
  padding: 10px;
  border-radius: 100%;
  width: 50px;
  position: absolute;
  bottom: 0;
  right: 50%;
  z-index: 5;
}
.center-dot {
  position: absolute;
  top: 59.5%;
  left: 50.4%;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background-color: #b96c3e;
  transform: translate(-50%, -50%);
  z-index: 10;
}

.hand {
  position: absolute;
  width: 50%;
  height: 2px;
  top: 59%;
  left: 50.5%;
  transform-origin: left center;
  background-color: #b96c3e;
 transition: none;
}

.hand.hour {
  width: 6%;
  height: 10px;
  background-color: #b96c3e;
  border-radius: 10% 30% 30% 10%;
}

.hand.minute {
  width: 8%;
  height: 7px;
  background-color: #b96c3e;
  border-radius: 10% 50% 50% 10%;
}

.hand.second {
  width: 12%;
  height: 4px;
  background-color: #b96c3e;
  border-radius: 10% 50% 50% 10%;
}
</style>
