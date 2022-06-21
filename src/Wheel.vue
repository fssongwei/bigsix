<template>
  <div>
    <!-- type: image -->
    <!-- <FortuneWheel
      style="width: 500px"
      type="image"
      :useWeight="true"
      :prizes="prizes"
      :angleBase="-10"
      @rotateStart="onImageRotateStart"
      @rotateEnd="onRotateEnd"
    >
      <img slot="wheel" src="@/assets/logo.png" />
      <img slot="button" src="@/assets/logo.png" />
    </FortuneWheel> -->

    <!-- type: canvas -->
    <div>balance: {{ balance }}</div>
    <FortuneWheel
      style="width: 500px"
      :canvas="canvasOptions"
      :prizes="prizes"
      :verify="cavansVerify"
      @rotateStart="onCanvasRotateStart"
      @rotateEnd="onRotateEnd"
      :useWeight="true"
    />
    <div>
      <button @click="start(1)">1</button>
      <button @click="start(3)">3</button>
      <button @click="start(5)">5</button>
      <button @click="start(11)">11</button>
      <button @click="start(23)">23</button>
      <button @click="start('💎')">💎</button>
      <button @click="start('❤️')">❤️</button>
    </div>
    <div>
      You bet: {{ bet }} × $<input
        type="number"
        v-model="multiper"
        style="width: 30px"
      />
    </div>

    <div>
      history:
      {{ history.join(", ") }}
    </div>
  </div>
</template>

<script>
import FortuneWheel from "vue-fortune-wheel";
import "vue-fortune-wheel/lib/vue-fortune-wheel.css";

const distribution = [
  "❤️",
  "🎡",
  1,
  5,
  1,
  3,
  1,
  11,
  1,
  3,
  1,
  5,
  1,
  23,
  1,
  3,
  5,
  1,
  3,
  11,
  1,
  3,
  1,
  5,
  1,
  "🎡",
  1,
  "💎",
  3,
  1,
  5,
  1,
  3,
  1,
  11,
  1,
  5,
  11,
  1,
  23,
  1,
  3,
  5,
  1,
  3,
  11,
  1,
  3,
  1,
  5,
  1,
  3,
  1,
];

const bgColors = [
  ["❤️", "black"],
  ["💎", "black"],
  ["🎡", "yellow"],
  [1, "#B63522"],
  [3, "#648F37"],
  [5, "#38548C"],
  [11, "#737371"],
  [23, "grey"],
];

const colors = [
  ["❤️", "red"],
  ["💎", "white"],
  ["🎡", "black"],
  [1, "black"],
  [3, "black"],
  [5, "black"],
  [11, "black"],
  [23, "black"],
];

const bgColorsMap = new Map();
bgColors.forEach(([key, value]) => bgColorsMap.set(key, value));
const colorsMap = new Map();
colors.forEach(([key, value]) => colorsMap.set(key, value));

const prizes = distribution.map((name, index) => {
  return {
    id: index,
    name: name + "",
    value: name + "",
    weight: 1,
    bgColor: bgColorsMap.get(name),
    color: colorsMap.get(name),
  };
});

console.log(prizes.length);

export default {
  components: {
    FortuneWheel,
  },
  data() {
    return {
      cavansVerify: false, // Whether the turntable in canvas mode is enabled for verification
      canvasOptions: {
        borderWidth: 6,
        borderColor: "#584b43",
        fontSize: 15,
        textRadius: 230,
      },
      prizes,
      bet: "",
      balance: 0,
      history: [],
      multiper: 1,
    };
  },
  methods: {
    onImageRotateStart() {
      console.log("onRotateStart");
    },
    onCanvasRotateStart(rotate) {
      if (this.canvasVerify) {
        const verified = true; // true: the test passed the verification, false: the test failed the verification
        this.DoServiceVerify(verified, 2000).then((verifiedRes) => {
          if (verifiedRes) {
            console.log("Verification passed, start to rotate");
            rotate(); // Call the callback, start spinning
            this.canvasVerify = false; // Turn off verification mode
          } else {
            alert("Failed verification");
          }
        });
        return;
      }
      console.log("onCanvasRotateStart");
    },
    onRotateEnd(prize) {
      this.history.push(prize.value + "");
      let isWin = prize.value == this.bet;
      let msg = `${prize.value} win!`;
      if (this.bet !== "") {
        let amount =
          (this.bet === "❤️"
            ? 45
            : this.bet === "💎"
            ? 45
            : parseInt(this.bet)) * this.multiper;
        if (isWin) {
          this.balance += amount;
          msg += `\n You win $${amount}😁`;
        } else {
          this.balance -= this.multiper;
          msg += `\n You lose $${this.multiper}😭`;
        }
      }
      alert(msg);
    },
    // Simulate the request back-end interface, verified: whether to pass the verification, duration: delay time
    DoServiceVerify(verified, duration) {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve(verified);
        }, duration);
      });
    },
    start(prize) {
      this.bet = prize;
    },
  },
};
</script>
