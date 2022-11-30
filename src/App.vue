<template>
  <weather/>
</template>

<script>
import weather from './components/weather.vue'

export default {
  name: 'App',
  components: {
    weather
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

h1,h2 {
  font-weight: 300;
}

#app {
  height: 100%;
  font-family: 'Roboto', Helvetica, Arial, sans-serif;
  font-weight: 300;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
}

#app:before {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  z-index: -1;
}

body {
  height: 100vh;
  background: url('assets/img/bg.jpg') no-repeat center / cover;
}

@keyframes fade{
  from{
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
