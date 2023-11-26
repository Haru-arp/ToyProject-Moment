<template>
  <!-- <div class="AppTitle"><span>KB!</span>오늘 뭐먹지?</div> -->
  <div class="launch-container">
    <img src="./assets/KB오늘뭐먹지.svg" />
    <div class="Menu-container">
      <span>메뉴판</span>
      <div class="MenuPlate">
        <MenuItems
          :foodName="item"
          :idx="idx"
          v-for="(item, idx) in menuItems"
          :key="item"
        />
      </div>
    </div>
    <div class="roulette">
      <span>오늘 점심 메뉴는?</span>

      <div v-if="selectedItem !== null">
        <span class="Select-Menu">{{ selectedItem }}</span>
        입니다.
      </div>
      <div v-else></div>
      <button @click="selectRandomItemWithDelay">딸깍</button>
      <Vue3Lottie
        class="animation"
        v-if="isLoading"
        :animationData="AnimationData"
        :height="200"
        :width="200"
        :autoPlay="true"
      />
    </div>
  </div>
</template>

<script setup>
import { Vue3Lottie } from "vue3-lottie";
import { ref } from "vue";
import MenuItems from "./components/MenuItems.vue";
import AnimationData from "./assets/lottie/Animation.json";
const menuItems = ref([
  "우미정",
  "피자",
  "부대찌개",
  "닭한마리",
  "초밥",
  "빨강색 간판 칼국수",
  "김치찜",
  "순대국",
  "백소정 외 일식(돈까스)",
  "무적권 (돌림판 돌린 사람이 정하기)",
]);

const selectedItem = ref(null);
const isLoading = ref(false);

const selectRandomItem = () => {
  const randomIndex = Math.floor(Math.random() * menuItems.value.length);
  selectedItem.value = menuItems.value[randomIndex];
  console.log(selectedItem.value);
  isLoading.value = false;
};

const selectRandomItemWithDelay = () => {
  isLoading.value = true;
  setTimeout(() => {
    selectRandomItem();
  }, 3000);
};
</script>

<style>
* {
  background-color: #f8f6f5;
  font-family: "KBFG Display";
}

.launch-container {
  padding: 3rem;
}

.Menu-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 3rem;
}

.Menu-container span {
  position: relative;
  font-size: 1.5rem;
  font-weight: bold;
}

.Menu-container span:after {
  content: "";
  display: block;
  width: 100%;
  height: 0.15rem;
  position: absolute;
  bottom: 1px;
  left: 1px;
  background: #f46600;
}

.AppTitle {
  font-size: 2rem;
}

.MenuPlate {
  display: flex;
  flex-direction: column;

  flex-wrap: wrap;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  gap: 1.5rem;
  margin-top: 30px;
}

.roulette {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 70px;
  font-size: 1.8rem;
  /* font-weight: bold; */
}
.Select-Menu {
  color: #f46600;
  padding: 0 15px;
  font-weight: bold;
}
button {
  margin-top: 50px;
  width: 100px;
  height: 40px;
  border-radius: 14px;
  background-color: transparent;
  border: 2px solid #f46000;
  cursor: pointer;
  font-weight: 500;
}

button:active {
  /* width: 100px;
  height: 40px; */
  margin-top: 50px;

  border-radius: 14px;
  background-color: #f46000;
}
.animation {
  position: absolute;
  top: 170px;
}
</style>
