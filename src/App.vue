<template>
  <div id="app">
    <Configurator modelId="photobox" collection="thumbnail"/>
  </div>
</template>

<script>
import Configurator from './components/Configurator';

export default {
  name: 'App',
  components: {
    Configurator,
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
