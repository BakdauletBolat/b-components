<script setup lang="ts">
import TimeSlot from "./components/TimeSlotGroup";
import {onMounted, ref} from "vue";


const workDays = {
  0: {
    work: false,
    startHour: 15,
    endHour: 16
  },
  1: {
    work: true,
    startHour: 12,
    endHour: 22
  },
  2: {
    work: true,
    startHour: 8,
    endHour: 22
  },
  3: {
    work: true,
    startHour: 12,
    endHour: 15
  },
  4: {
    work: true,
    startHour: 15,
    endHour: 18
  },
  5: {
    work: true,
    startHour: 10,
    endHour: 22
  },
  6: {
    work: false,
    startHour: 8,
    endHour: 22
  }
}

const dates = ref<Date[]>([]);

onMounted(()=>{
  for (let i=1; i<=7; i++) {
    const date_tmp = new Date();
    const date = new Date(date_tmp.getTime()+(24 * 60 * 60 * 1000 * i));
    dates.value.push(date);
  }
  console.log(dates.value);
});


function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

const onselect = async (selected) => {

  await sleep( 1000);
  console.log(selected, 'selected')
}

</script>

<template>
  <div class="grid gap-3">
    <TimeSlot @onselect="onselect" :key="date.getTime()" v-for="date in dates" :today="date" :workDays="workDays"></TimeSlot>
  </div>
</template>

<style scoped>

</style>
