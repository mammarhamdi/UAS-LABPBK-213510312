<template>
  <div id="app">
    <header>
      <nav>
        <ul>
          <li :class="{ active: $route.path === '/' }"><router-link to="/">CUACA</router-link></li>
          <li :class="{ active: $route.path === '/location' }"><router-link to="/location">LOKASI</router-link></li>
          <li :class="{ active: $route.path === '/stopwatch' }"><router-link to="/stopwatch">STOPWATCH</router-link></li>
          <li :class="{ active: $route.path === '/tictactoe' }"><router-link to="/tictactoe">GAMES</router-link></li>
          <li :class="{ active: $route.path === '/photo' }"><router-link to="/photo">WALLPAPER</router-link></li>
        </ul>
      </nav>
    </header>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>
body {
  background-image: url('https://1.bp.blogspot.com/-DuRtgmaUhIk/YGFXVkyq3DI/AAAAAAAAIMQ/i80mKqCTfesgRpUd1V7QPmkJ6DEahRWLwCLcBGAsYHQ/s16000/biru-pastel.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  margin: 0;
  padding: 0;
  height: 100%;
}

#app {
  text-align: center;
  padding: 20px;
  height: 100%;
  box-sizing: border-box;
}

header {
  background: linear-gradient(to right, #040432, #063eae);
  padding: 20px;
  box-shadow: 0 2px 4px rgba(255, 248, 248, 0.2);
}

nav {
  display: flex;
  justify-content: center;
}

nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
  display: flex;
}

nav li {
  margin-right: 10px;
}

nav a {
  position: relative;
  color: #fff;
  text-decoration: none;
  padding: 5px 10px;
}

nav a:before {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: #fff;
  transform: scaleX(0);
  transition: transform 0.3s;
}

nav li.active a:before {
  transform: scaleX(1);
}
</style>
