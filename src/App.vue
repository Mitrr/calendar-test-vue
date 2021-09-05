<template>
  <div id="app">
    <div class="calendar__wrapper">
      <w-calendar></w-calendar>
    </div>
  </div>
</template>

<script>
  import WidgetCalendar from "./components/WidgetCalendar/WidgetCalendar";
  export default {
    name: 'App',
    components: {
      WCalendar: WidgetCalendar
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    padding-top: 50px;
    font-size: 16px;
    font-weight: 300;
  }
  .calendar__wrapper {
    margin: 0 auto;
    width: fit-content;
  }
</style>
