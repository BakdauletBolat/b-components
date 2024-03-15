<script lang="ts" setup>
import {ref} from "vue";




const {today, workDays} = defineProps<{
  today: Date,
  workDays: any
}>();

const emit = defineEmits(['onselect']);

const start = today;
const end = new Date(start.getTime());
start.setHours(workDays[start.getDay()]['startHour'], 0,0,0);
end.setHours(workDays[start.getDay()]['endHour'], 0,0,0);

const iterator = 30;

function withZeroInteger(item: number) {
  if (item.toString().length == 1) {
    return "0"+item.toString();
  }
  return item.toString();

}


const toRender: Date[] =ref([]);

const checkedDates: number[] = ref([]);


for (let i = start.getTime(); i<=end.getTime(); i+=iterator * 60 * 1000) {
  toRender.value.push(new Date(i));
}

const book= async (start: Date) => {
  const end = new Date(start.getTime()+iterator*60*1000);
  for (let i = start.getTime(); i<=end.getTime()-1; i+=60 * 1000) {
    if (checkedDates.value.includes(i)) {
      console.log('Already booked');
      return;
    }
  }
  for (let i = start.getTime(); i<=end.getTime()-1; i+=60 * 1000) {
    checkedDates.value.push(i);
  }
  await emit('onselect', start);
  console.log('Booked');
}


</script>
<template>
  <div class="flex justify-center items-center bg-slate-300 p-4">
    <div v-if="workDays[start.getDay()]['work']" >
      <div class="p-3">{{start}}</div>
      <div class="grid grid-cols-4 flex-shrink-0 w-full gap-4 max-w-[400px]">
        <div @click="book(slot)" :class="{
      'bg-slate-100 text-slate-500 border-black cursor-not-allowed': checkedDates.includes(slot.getTime())
    }" class="bg-slate-500 cursor-pointer rounded p-2 text-center text-white " v-for="slot in toRender">{{withZeroInteger(slot.getHours())}} : {{withZeroInteger(slot.getMinutes())}}</div>
      </div>
    </div>
    <div v-else>
      Эти дни не работает
    </div>
  </div>
</template>