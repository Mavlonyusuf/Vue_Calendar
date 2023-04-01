<template>
  <div>
    <Calendar />
  </div>
</template>
<script>
import Calendar from "./components/calendar.vue";
export default {
  components: {
    Calendar,
  },
  setup() {},
};
</script>
<style>
@import url(https://fonts.googleapis.com/css2?family=Ubuntu:wght@100;200;300;400;500;600;700&display=swap);

body {
  font-family: "Ubuntu", sans-serif;
}
</style>
