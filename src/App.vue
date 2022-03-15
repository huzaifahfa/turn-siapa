<script setup>
import { ref } from "vue";
import { DateTime } from "luxon";

const startingDate = DateTime.local(2022, 3, 14).setZone("Asia/Kuala_Lumpur");
const today = DateTime.local().setZone("Asia/Kuala_Lumpur");

const weekdayNumber = today.weekday;

const todayDateString = today
  .setLocale("ms-MY")
  .toLocaleString(DateTime.DATETIME_HUGE);
const weekNumber = today.weekNumber;

const turnGantiPasir = {
  0: "Maryam",
  1: "Mujahid",
  2: "Hafsa",
};

const turnBuangTaik = {
  0: "Maryam",
  1: "Mujahid",
  2: "Hafsa",
};

const turnSiapa = () => {
  // If Monday, show ganti pasir punya turn
  if (weekdayNumber === 1) {
    return turnGantiPasir[weekNumber % 3];
  }

  // If not Monday, show buang taik punya turn
  function getNumberOfDays(start, end) {
    // One day in milliseconds
    const oneDay = 1000 * 60 * 60 * 24;

    // Calculating the time difference between two dates
    const diffInTime = end - start;

    // Calculating the no. of days between two dates
    const diffInDays = Math.round(diffInTime / oneDay);

    return diffInDays;
  }

  const dayDifference = getNumberOfDays(startingDate, today);

  const countCertainDays = (days, d0, d1) => {
    const ndays = 1 + Math.round((d1 - d0) / (24 * 3600 * 1000));
    const sum = (a, b) => a + Math.floor((ndays + ((d0.day + 6 - b) % 7)) / 7);

    return days.reduce(sum, 0);
  };

  // Difference in days excluding Mondays
  return turnBuangTaik[
    (dayDifference - countCertainDays([1], startingDate, today)) % 3
  ];
};

const dahGanti = ref(false);
</script>

<template>
  <h1>Turn siapa harini</h1>
  <p>{{ todayDateString }}</p>

  <p>{{ weekdayNumber === 1 ? "Ganti pasir" : "Buang taik" }}</p>
  <h2>{{ turnSiapa() }}</h2>

  <button v-if="dahGanti === false" @click="dahGanti = true">
    {{ weekdayNumber === 1 ? "Dah ganti?" : "Dah buang?" }}
  </button>
  <p v-else>{{ weekdayNumber === 1 ? "Dah ganti." : "Dah buang." }}</p>
  <p>Week number: {{ weekNumber }}</p>
</template>
