<template>
  <div class="date-selector">
    <input type="date" v-model="selectedDate" @change="dateChanged" />
  </div>
</template>

<script>
export default {
  name: 'DateSelector',
  data() {
    return {
      selectedDate: null,
    };
  },
  methods: {
    dateChanged() {
      this.$emit('date-selected', this.selectedDate);
    },
  },
};
</script>

<style scoped>
.date-selector {
  text-align: center;
  margin: 20px;
}
</style>
