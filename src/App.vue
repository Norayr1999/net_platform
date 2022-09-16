<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="col-sm">
          <SimpleBtn />
        </div>
        <div class="col-sm">
          <AdvancedFilter/>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import SimpleBtn from './components/SimpleBtn.vue';
import AdvancedFilter from './components/AdvancedFilter.vue';

export default defineComponent({
  name: 'App',
  components: {
    SimpleBtn,
    AdvancedFilter
  }
});
</script>

<style lang="scss">
::-webkit-scrollbar {
  width: 5px;
}
::-webkit-scrollbar-track {
  box-shadow: inset 0 0 5px rgb(188, 186, 186); 
  border-radius: 10px;
}

::-webkit-scrollbar-thumb {
  background: rgb(113, 113, 113); 
  border-radius: 10px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
