<template>
  <div class="calendar">
    <table>
      <thead>
        <tr>
          <th v-for="dayOfWeek in daysOfWeek" :key="dayOfWeek">{{ dayOfWeek }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="week in calendarData.weeks" :key="week">
          <td v-for="day in week" :key="day" :class="{ 'empty-cell': !day }">{{ day }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Calendar',
  props: {
    selectedDate: {
      type: String,
      required: true,
    },
  },
  computed: {
    daysOfWeek() {
      return ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
    },
    calendarData() {
      const firstDay = new Date(this.selectedDate);
      firstDay.setDate(1);

      const lastDay = new Date(this.selectedDate);
      lastDay.setMonth(lastDay.getMonth() + 1);
      lastDay.setDate(0);

      const weeks = [];
      let currentDay = new Date(firstDay);

      while (currentDay <= lastDay) {
        const week = Array(7).fill(null).map((_, index) => {
          const day = new Date(currentDay);
          day.setDate(day.getDate() + index);
          return (day.getMonth() === firstDay.getMonth()) ? day.getDate() : null;
        });
        weeks.push(week);
        currentDay.setDate(currentDay.getDate() + 7);
      }

      return {
        weeks,
      };
    },
  },
};
</script>

<style scoped>
.calendar {
  /* Calendar styling goes here */
}

.empty-cell {
  /* Styling for empty cells goes here */
}
</style>
