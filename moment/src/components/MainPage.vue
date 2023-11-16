<template>
  <div class="loading"></div>
  <header id="header">
    <h1 class="sr-only">검색창과 날씨 정보</h1>
    <div class="header__search">
      <button @click="toggleClass" class="header__toggle" type="button">
        Search
      </button>
      <div class="header__form-container" v-bind:class="{ active: toggle }">
        <form
          @submit="submitGoogle"
          class="header__form"
          action="https://www.google.com/search"
          method="GET"
        >
          <input
            v-model="InputValue"
            class="header__form--input"
            type="search"
          />
          <button class="header__form--btn" type="submit"></button>
        </form>
      </div>
    </div>
    <div class="header__weather">
      <div class="header__weather--container">
        <i class="header__weather--ico wi"></i>
        <div class="header__weather--temperature"></div>
      </div>
      <div class="header__weather--location"></div>
    </div>
  </header>
  <section class="container-top">
    <h1 class="container-top--time"></h1>
    <form
      class="container-top__greeting"
      action=""
      method="get"
      name="greeting"
    >
      <h2 class="container-top__greeting--text">
        Thank you visiting our website !
      </h2>
      <input
        class="container-top__greeting--input"
        type="text"
        placeholder="What's Your Name ?"
        name="name"
        autocomplete="off"
        required
      />
      <button
        class="container-top__greeting--edit hide"
        type="button"
        data-type="edit"
      ></button>
    </form>
  </section>
</template>

<script setup>
import { ref } from "vue";
import "../assets/css/weather-icons.min.css";
const toggle = ref(false);
const toggleClass = () => {
  toggle.value = !toggle.value;
};

const InputValue = ref("");
const submitGoogle = (e) => {
  e.preventDefault();
  const value = InputValue.value;
  console.log("e", value);
  window.open(`http://www.google.co.kr/search?=q=${value}`);
  InputValue.value = "";
};

const COORDS = "coords";
const WEATHER_KEY = "656940ab113d2f9387d07fe60d1bd857";

const createWeather = (weather) => {
  const textTemp = document.querySelector(".header__weather--temperature");
  const textLoaction = document.querySelector(".header__weather--location");
  const weatherIcon = document.querySelector(".header__weather--ico");
  const weatherContainer = document.querySelector(
    ".header__weather--container"
  );
  const temp = weather.main.temp;
  const name = weather.name;
  const status = weather.weather[0].id;
  const weatherTitle = weather.weather[0].description;
  const sunrise = weather.sys.sunrise;
  const sunset = weather.sys.sunset;
  const getTime = new Date().getTime();
  const str = getTime.toString();
  const substr = str.substring(0, 10);
  const number = Number(substr);
  textTemp.innerText = `${temp}°`;
  textLoaction.innerText = name;
  textLoaction.setAttribute("title", `${name}`);
  weatherIcon.setAttribute("title", `${weatherTitle}`);
  weatherContainer.setAttribute("title", `${temp}°`);
  if (number >= sunrise && number < sunset) {
    weatherIcon.classList.add(`wi-owm-day-${status}`);
  } else {
    weatherIcon.classList.add(`wi-owm-night-${status}`);
  }
};

function getWeather(coords) {
  const lat = coords.latitude;
  const lon = coords.longitude;
  const requestOptions = {
    method: "GET",
    redirect: "follow",
  };

  fetch(
    `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=metric&appid=${WEATHER_KEY}`,
    requestOptions
  )
    .then((response) => response.json())
    .then((json) => createWeather(json))
    .catch((error) => console.log("error", error));
}

function saveCoords(coordsObj) {
  localStorage.setItem(COORDS, JSON.stringify(coordsObj));
}

function succCoordsHandle(position) {
  const latitude = position.coords.latitude;
  const longitude = position.coords.longitude;
  const coordsObj = {
    latitude,
    longitude,
  };
  getWeather(coordsObj);
  saveCoords(coordsObj);
}

function errLoacitonHandle() {
  console.log("cant access geo location");
}

function loadCoords() {
  navigator.geolocation.getCurrentPosition(succCoordsHandle, errLoacitonHandle);
}

function init() {
  const coords = localStorage.getItem(COORDS);
  if (coords === null) {
    loadCoords();
  } else {
    const coordsParse = JSON.parse(coords);
    getWeather(coordsParse);
    setInterval(() => getWeather(coordsParse), 60000);
  }
}

init();

const IMAGE_KEY = "HZ_VhCN89TVCLoQK0G7xCcHz33_Obskf4KB0vkklu_g";
const infoHandle = (info) => {};
</script>

<style>
.sr-only {
  position: absolute;
  z-index: -100;
  width: 1px;
  height: 1px;
  overflow: hidden;
  opacity: 0;
}
.loading {
  position: absolute;
  width: 100%;
  height: 100vh;
  left: 0;
  top: 0;
  background-color: #181515;
  z-index: 999;
  pointer-events: none;
  animation: loadingMask 1s forwards;
}

@keyframes loadingMask {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}

/*Header*/

#header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 80px;
  padding: var(--size-padding);
  background-color: transparent;
  opacity: 1;
  transition: opacity var(--transition-duration) ease-in-out;
}
.header__search {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
}

.header__toggle {
  color: var(--color-white);
  font-weight: 500;
  margin-right: 0.5rem;
  font-family: "roboto";
  background-color: transparent;
  border: none;
  transition: color var(--transition-duration);
}
.header__form-container {
  max-width: 0;
  opacity: 0;
  transition: all var(--transition-duration) ease-in-out;
  pointer-events: none;
}
.header__form-container.active {
  max-width: 100%;
  opacity: 1;
  pointer-events: visible;
}
.header__form {
  display: flex;
  align-items: center;
  margin-bottom: 4px;
}

.header__form--input {
  font-size: 0.9rem;
  width: 100%;
}

.header__form--btn {
  width: 14px;
  height: 14px;
  margin-left: 0.5em;
  border: 0;
  background-color: transparent;
}

.header__form--btn::before {
  content: "";
  display: inline-block;
  width: 14px;
  height: 14px;
  margin: 0 auto;
  background: url("../assets/image/search.svg") no-repeat center/cover;
}

.header__form-container::after {
  content: "";
  display: block;
  width: 100%;
  height: 2px;
  background-color: var(--color-white);
}

.header__weather--container {
  display: flex;
  height: 36px;
}

.header__weather--ico::before {
  font-size: 1.5rem;
  line-height: 1.5;
  vertical-align: middle;
}

.header__weather--temperature {
  font-size: 1.5rem;
  line-height: 1.5;
  text-align: center;
  width: 100%;
  margin-left: 0.5em;
  cursor: default;
}

.header__weather--location {
  font-size: 0.9rem;
  text-align: center;
  margin-top: 0.1em;
  cursor: default;
}

.container-top {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 700px;
  margin: 0 auto;
  opacity: 1;
  transition: opacity var(--transition-duration) ease-in-out;
}

.container-top--time {
  font-size: 9rem;
  margin: 0 auto var(--size-margin-bottom);
}

.container-top__greeting {
  width: 100%;
  position: relative;
  text-align: center;
}

.container-top__greeting--text {
  font-size: 3rem;
  margin-bottom: var(--size-margin-bottom);
}

.container-top__greeting--input {
  font-size: 3rem;
  text-align: center;
  border-bottom: 2px solid var(--color-white);
  display: inline-block;
  cursor: text;
}

.container-top__greeting--input.hide {
  display: none;
}

.container-top__greeting--name {
  witdh: 100%;
  font-size: 3rem;
  margin-top: 20px;
  outline: none;
  border-bottom: 2px solid transparent;
  transition: border-bottom-color var(--transition-duration) ease-in-out;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.container-top__greeting--name[contenteditable="true"]:focus {
  border-bottom-color: var(--color-white);
}

.container-top__greeting--edit {
  position: absolute;
  right: -15px;
  top: 0;
  width: 13px;
  height: 13px;
  background: url("../assets/image/edit.svg") no-repeat center/cover;
  display: block;
}

.container-top__greeting--edit.hide {
  display: none;
}
</style>
