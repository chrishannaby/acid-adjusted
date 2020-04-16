<template>
  <div class="min-h-screen py-12 px-4 flex items-center justify-center" :class="color">
    <div
      class="container bg-white p-12 shadow-lg rounded-md flex flex-col items-center justify-around"
    >
      <div
        class="border-b-2 border-gray-300 flex items-center justify-between flex-col sm:flex-row"
      >
        <div class="flex items-center">
          <input
            v-model.number="volume"
            class="w-24 text-xl mr-2 form-input"
            type="number"
            min="1"
            oninput="validity.valid||(value=1)"
          />
          <p>ml</p>
        </div>
        <p class="pl-0 pt-4 sm:pl-8 sm:pt-0">of</p>
        <div class="flex flex-col items-center p-8">
          <img class="h-32" :src="require(`@/assets/${iHave}.svg`)" />
          <select class="mt-4 form-select" v-model="iHave">
            <option>Orange</option>
            <option>Grapefruit</option>
            <option>Pineapple</option>
          </select>
        </div>
        <p class="text-center">adjusted to</p>
        <div class="flex flex-col items-center p-8">
          <img class="h-32" :src="require(`@/assets/${iWant}.svg`)" />
          <select class="mt-4 form-select" v-model="iWant">
            <option>Lime</option>
            <option>Lemon</option>
          </select>
        </div>
      </div>
      <div class="mt-4">
        <p class="font-semibold text-gray-800 text-xl text-center">Add</p>
        <ul class="mt-2 text-lg">
          <li v-for="(ingredient, idx) in adjustments" :key="idx">
            <span class="font-semibold">{{ingredient.amount.toFixed(1)}}</span>
            g of {{ingredient.name}} acid
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      volume: 1000,
      iHave: "Orange",
      iWant: "Lime",
      per1000: {
        Orange: {
          Lime: {
            citric: 32,
            malic: 20
          },
          Lemon: {
            citric: 52
          }
        },
        Grapefruit: {
          Lime: {
            citric: 27,
            malic: 13
          },
          Lemon: {
            citric: 40
          }
        },
        Pineapple: {
          Lime: {
            citric: 32,
            malic: 20
          },
          Lemon: {
            citric: 52
          }
        }
      }
    };
  },
  computed: {
    color() {
      return this.iWant === "Lime" ? "bg-green-400" : "bg-yellow-400";
    },
    adjustments() {
      return Object.entries(this.per1000[this.iHave][this.iWant]).map(a => {
        return {
          name: a[0],
          amount: a[1] * this.volumeRatio
        };
      });
    },
    volumeRatio() {
      return this.volume / 1000;
    }
  }
};
</script>
