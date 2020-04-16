<template>
  <div class="min-h-screen py-4 px-4 flex items-center justify-center text-gray-900" :class="color">
    <div
      class="container bg-white p-12 shadow-lg rounded-md flex flex-col items-center justify-around"
    >
      <div
        class="border-b-2 border-gray-300 flex items-center justify-between flex-col sm:flex-row"
      >
        <div class="flex items-center">
          <input
            v-model.number="volume"
            class="w-24 text-xl mr-2 form-input font-semibold"
            type="number"
            min="1"
            oninput="validity.valid||(value=1)"
          />
          <p>ml</p>
        </div>
        <p class="pl-0 pt-4 sm:pl-8 sm:pt-0">of</p>
        <fruit-picker :fruits="from" v-model="iHave" />
        <p class="text-center">adjusted to</p>
        <fruit-picker :fruits="to" v-model="iWant" />
      </div>
      <div class="mt-4">
        <p class="font-semibold text-gray-800 text-xl text-center">Add</p>
        <ul class="mt-2 text-lg h-12">
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
import FruitPicker from "./components/FruitPicker.vue";

export default {
  components: {
    FruitPicker
  },
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
    from() {
      return Object.keys(this.per1000);
    },
    to() {
      return new Set(Object.values(this.per1000).flatMap(x => Object.keys(x)));
    },
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
