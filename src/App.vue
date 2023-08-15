<template>
  <div id="app">
    <header>
      <h1>연차계획서 페이지</h1>
    </header>
    <main>
      <DateSelector v-model="selectedDate" />
      <div class="calendar-info">
        <Calendar :selectedDate="selectedDate" />
        <InfoComponent :selectedDate="selectedDate" />
      </div>
    </main>
  </div>
</template>

<script>
import DateSelector from './components/DateSelector.vue';
import Calendar from './components/Calender.vue';
import InfoComponent from './components/ViewInfo.vue';

export default {
  components: {
    DateSelector,
    Calendar,
    InfoComponent,
  },
  data() {
    return {
      selectedDate: null,
    };
  },
};
</script>

<style>
#app {
  font-family: Arial, Helvetica, sans-serif;
  text-align: center;
}

header {
  background-color: #f0f0f0;
  padding: 20px;
}

main {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 20px;
}

.calendar-info {
  display: flex;
  gap: 20px;
}

footer {
  background-color: #f0f0f0;
  padding: 10px;
}
</style>
