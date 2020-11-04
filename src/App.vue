<template>
  <div>
    <button @click="currentComponent = notCurrentComponent">
      Go to {{notCurrentComponent}}
    </button>
    <component :is="currentComponent"></component>
  </div>
</template>

<script>
import Reading from './components/Reading.vue';
import Writing from './components/Writing.vue';
export default {
  components: {
    Reading,
    Writing
  },
  computed : {
    notCurrentComponent(){
      return this.currentComponent==='reading' ? 'writing' : 'reading'
    }
  },
  data() {
    return{
      currentComponent : 'writing'
    }
  },
};
</script>