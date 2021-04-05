<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
        <router-link to="/board">Bulletin Board</router-link>
    </div>
    <router-view />
  </div>
</template>

<style>
#app {
  text-align: center;
}
</style>
