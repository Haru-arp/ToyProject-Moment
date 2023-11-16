<template>
  <Mainpage />
</template>

<script setup>
import Mainpage from "./components/MainPage.vue";
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@400;500;700&display=swap");

:root {
  /* color */
  --color-white: #fff;
  --color-background: rgba(0, 0, 0, 0.5);
  --color-gray: #bcbcbc;
  --color-lightblue: #369fff;

  /* size */
  --size-padding: 1em;
  --size-margin-bottom: 2rem;
  --size-todo: 1em;

  /* duration */
  --transition-duration: 300ms;
}
html,
body {
  height: 100vh;
}
body {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  color: var(--color-white);
  font-family: "roboto", sans-serif;
  width: 100%;
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
  background-color: black;
}

input {
  color: var(--color-white);
  outline: none;
  padding: 0;
  border: 0;
  background-color: transparent;
}
</style>
