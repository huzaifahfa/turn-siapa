<script setup>
import { ref, computed } from "vue";

const startingDate = new Date("9/27/2021");
const today = new Date();
// today.setDate(27 + 8 + 5 + 2);

const dayNumber = today.getDay();
const date = today.toLocaleDateString("ms-MY", {
  weekday: "long",
  year: "numeric",
  month: "long",
  day: "numeric",
});

const day = today.getDate();

const oneJan = new Date(today.getFullYear(), 0, 1);
const numberOfDays = Math.floor((today - oneJan) / (24 * 60 * 60 * 1000));
const week = Math.ceil((today.getDay() + 1 + numberOfDays) / 7);

const turnGantiPasir = {
  0: "Maryam",
  1: "Huzaifah",
  2: "Mujahid",
  3: "Hafsa",
};

const turnBuangTaik = {
  0: "Mujahid",
  1: "Huzaifah",
  2: "Hafsa",
  3: "Maryam",
};

const turnSiapa = () => {
  // If Monday, show ganti pasir punya turn
  if (dayNumber === 1) {
    return turnGantiPasir[week % 4];
  }

  // If not Monday, show buang taik punya turn
  function getNumberOfDays(start, end) {
    //const date1 = new Date(start);
    //const date2 = new Date(end);

    // One day in milliseconds
    const oneDay = 1000 * 60 * 60 * 24;

    // Calculating the time difference between two dates
    const diffInTime = end.getTime() - start.getTime();

    // Calculating the no. of days between two dates
    const diffInDays = Math.round(diffInTime / oneDay);

    return diffInDays;
  }

  const dayDifference = getNumberOfDays(startingDate, today);

  const countCertainDays = (days, d0, d1) => {
    var ndays = 1 + Math.round((d1 - d0) / (24 * 3600 * 1000));
    var sum = function (a, b) {
      return a + Math.floor((ndays + ((d0.getDay() + 6 - b) % 7)) / 7);
    };
    return days.reduce(sum, 0);
  };

  // Difference in days excluding Mondays
  return turnBuangTaik[
    (dayDifference - countCertainDays([1], startingDate, today)) % 4
  ];
};

const dahGanti = ref(false);
</script>

<template>
  <p>{{ week }}</p>
  <h1>Turn siapa harini</h1>
  <p>{{ date }}</p>

  <p>{{ dayNumber === 1 ? "Ganti pasir" : "Buang taik" }}</p>
  <h2>{{ turnSiapa() }}</h2>

  <button v-if="dahGanti === false" @click="dahGanti = true">
    {{ dayNumber === 1 ? "Dah ganti?" : "Dah buang?" }}
  </button>
  <p v-else>{{ dayNumber === 1 ? "Dah ganti." : "Dah buang." }}</p>
</template>
