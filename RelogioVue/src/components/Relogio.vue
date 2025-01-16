<template>
  <div
    class="flex flex-col text-white shadow-2xl bg-indigo-300 p-10 rounded-3xl relative"
  >
    <img
      class="cat w-28 absolute -top-10 -right-10"
      src="/src/assets/cat-1.svg"
      alt=""
    />
    <span class="text-xl">{{ msg }}</span>
    <div class="flex">
      <span class="text-9xl">{{ hh }}</span>
      <span class="text-9xl">:</span>
      <span class="text-9xl">{{ mm }}</span>
      <div
        class="bg-indigo-200 h-20 w-20 rounded-full justify-center items-center flex"
      >
        <span class="text-5xl text-white">{{ ss }}</span>
      </div>
    </div>

    <div class="flex items-end justify-between p-4">
      <span
        class="opacity-50"
        :class="{ 'opacity-100 text-gray-50 text-2xl -translate-y-2': day === w }"
        v-for="(w, index) in week"
        :key="index"
      >
        {{ w }}
      </span>
    </div>

    <div class="mt-10">
      <span class="text-2xl">{{ moth }} {{ year }}</span>
    </div>

    <div class="grid grid-cols-7 gap-2 mt-4">
      <span
        v-for="(d, index) in daysInMonth"
        :key="index"
        class="w-10 h-10 flex items-center justify-center rounded-full"
        :class="{ 'bg-white text-indigo-400': d === currentDay }"
      >
        {{ d }}
      </span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

const week = ref(["Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sáb"]);
const moths = ref([
  "Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho",
  "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"
]);

const hh = ref("00");
const mm = ref("00");
const ss = ref("00");
const msg = ref("Bom dia");
const day = ref(null);
const moth = ref(null);
const year = ref("0000");

const daysInMonth = ref([]);  // 🔥 Lista com os dias do mês
const currentDay = ref(null);  // 🔥 Dia atual

function clock() {
  const Hour = new Date();

  hh.value = String(Hour.getHours()).padStart(2, "0");
  mm.value = String(Hour.getMinutes()).padStart(2, "0");
  ss.value = String(Hour.getSeconds()).padStart(2, "0");

  if (hh.value >= "18" && hh.value <= "24") {
    msg.value = "Boa Noite";
  } else if (hh.value >= "12" && hh.value < "18") {
    msg.value = "Boa Tarde";
  } else {
    msg.value = "Bom Dia";
  }

  day.value = week.value[Hour.getDay()];
  moth.value = moths.value[Hour.getMonth()];
  year.value = Hour.getFullYear();

  generateDaysInMonth(Hour.getFullYear(), Hour.getMonth());
  currentDay.value = Hour.getDate();  // 🔥 Guarda o dia atual
}

// 🔥 Gera os dias do mês
function generateDaysInMonth(year, month) {
  const totalDays = new Date(year, month + 1, 0).getDate();
  daysInMonth.value = Array.from({ length: totalDays }, (_, i) => i + 1);
}

onMounted(() => {
  clock();
  setInterval(clock, 1000);
});
</script>

<style scoped>
.cat {
  animation: jump 1s infinite alternate;
}

.current {
  color: rgb(255, 255, 255);
  opacity: 1;
  font-size: 22px;
}

@keyframes jump {
  0% {
    transform: translateY(6px);
  }
  100% {
    transform: translateY(-6px);
  }
}

@keyframes speeder {
  0% {
    transform: translateX(-40px);
  }

  100% {
    transform: rotate(-40deg);
  }
}
</style>
