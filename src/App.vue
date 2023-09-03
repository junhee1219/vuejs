<template>
  <h1>휴가언제쓰지?</h1>

  <div>
    <input type="date" v-model="addedHoliday">
    <button @click="addHoliday">빨간날추가</button>
  </div>

  <div>
    <span>사용할 연차 수</span>
    <input v-model="useRest">
  </div>

  <div>
    <div>
      <input type="date" v-model="dateFrom" @input="checkDateRange('From')">
      <span>부터</span>
    </div>
    <div></div>
    <input type="date" v-model="dateTo" @input="checkDateRange('To')">
    <span>까지</span>
  </div>
  <table>
    <tbod>
      <tr>
        <th>일</th>
        <th>월</th>
        <th>화</th>
        <th>수</th>
        <th>목</th>
        <th>금</th>
        <th>토</th>
      </tr>
    </tbod>
  </table>
  <h2></h2>
</template>

<script>


export default {
  created() {
    this.dateFrom = this.todayString;
    this.dateTo = this.yearLastDayString;
  },
  watch: {
    useRest: function () {
      this.getDefaultDataForYear();
    }
  },

  data() {
    return {
      useRest: 0,
      addedHoliday: '',
      dateFrom: '',
      dateTo: '',
      previousDateFrom: '',
      previousDateTo: '',
      count: 0,
      holidays: ['2023-01-01'
        , '2023-01-21'
        , '2023-01-22'
        , '2023-01-23'
        , '2023-01-24'
        , '2023-03-01'
        , '2023-05-05'
        , '2023-05-27'
        , '2023-06-06'
        , '2023-08-15'
        , '2023-09-28'
        , '2023-09-29'
        , '2023-09-30'
        , '2023-10-03'
        , '2023-10-09'
        , '2023-12-25'],
    };
  },
  computed: {
    todayString() {
      const date = new Date();
      const yyyy = date.getFullYear();
      const mm = String(date.getMonth() + 1).padStart(2, '0');
      const dd = String(date.getDate()).padStart(2, '0');
      return `${yyyy}-${mm}-${dd}`;
    },
    yearLastDayString() {
      const date = new Date();
      const yyyy = date.getFullYear();
      return `${yyyy}-12-31`;
    }
  },
  methods: {
    getAdjustedDate(inputDate, adjustedDays) {
      const adjustedDate = inputDate;
      adjustedDate.setDate(inputDate.getDate() + adjustedDays);
      return adjustedDate;
    },
    checkDateRange(input) {
      if (this.dateTo <= this.dateFrom) {
        alert("날짜 범위가 올바르지 않습니다!");
        if (input == 'From') {
          this.dateFrom = this.previousDateFrom;
        }
        if (input == 'To') {
          this.dateTo = this.previousDateTo;
        }
      } else {
        this.previousDateFrom = this.dateFrom;
        this.previousDateTo = this.dateTo;
      }
    },
    addHoliday() {
      this.holidays.push(this.addedHoliday);
      this.addedHoliday = '';
    },
    getDateNumFromString() {
      const date = new Date(dateString);
      const startOfYear = new Date(date.getFullYear(), 0, 1); // 해당 연도의 1월 1일
      const differenceInTime = date - startOfYear;
      const dateNum = Math.floor(differenceInTime / (1000 * 60 * 60 * 24));
      return dateNum + 1;
    },
    getDefaultData() {
      let dates = [];
      let holidayString = '';
      let result = {};
      let startDate = new Date(this.dateFrom)
      let endDate = new Date(this.dateTo)
      while (true) { // 전의 날짜가 1이 될때까지
        startDate.setDate(startDate.getDate() - 1);
        var day = startDate.getDay();
        if (day.toString() == '0' || day.toString() == '6' || this.holidays.indexOf(this.getStringFromDate(startDate)) != -1) {
        }
        else {
          break;
        }
      }
      while (true) { // 다음의 날짜가 1이 될때까지
        endDate.setDate(endDate.getDate() + 1);
        var day = endDate.getDay();
        if (day.toString() == '0' || day.toString() == '6' || this.holidays.indexOf(this.getStringFromDate(endDate)) != -1) {
        }
        else {
          break;
        }
      }
      for (let date = startDate; date <= endDate; date.setDate(date.getDate() + 1)) {
        const day = date.getDay();
        if (day.toString() == '0' || day.toString() == '6' || this.holidays.indexOf(this.getStringFromDate(date)) != -1) {
          holidayString += '1';
        } else {
          holidayString += '0';
        }
        dates.push(this.getStringFromDate(date));
      }
      result = {
        dates: dates,
        holidayString: holidayString,
      }
      console.log(result);
      return result;
    },
    getStringFromDate(date) {
      const yyyy = date.getFullYear();
      const mm = String(date.getMonth() + 1).padStart(2, '0');
      const dd = String(date.getDate()).padStart(2, '0');
      return `${yyyy}-${mm}-${dd}`;
    },
    getOptimalHoliPlan() {
      var dayInfo = this.getDefaultData();
      var dates = dayInfo.dates;
      var holidayString = dayInfo.holidayString;
      var maxRestDays = 0;
      var startDate = 0;
      var endDate = 0;
      var restDayList;
      for (var startDay = fromDate; startDay < Math.min(toDate - maxRestDays, holidayString.length - maxRestDays); startDay++) {
        let tempRestDayList = [];
        let restDays = 0;
        let useRestDays = useRestDays;
        while (useRestDays > 0 || holidayString[startDay + restDays] == 1) {
          if (holidayString[startDay + restDays] == 0) {
            useRestDays = useRestDays - 1; // 평일이면 연차사용
            tempRestDayList.push(this.getStringFromDate(new Date(year, 0, startDay + restDays + 1)));
          }
          restDays = restDays + 1;  // 쉬는날
        }
        if (restDays > maxRestDays) {
          maxRestDays = restDays;
          startDate = startDay;
          endDate = startDate + maxRestDays - 1;
          restDayList = JSON.parse(JSON.stringify(tempRestDayList));
        }
      }
      result = { start: dates[startDate], end: dates[endDate], maxRestDays: maxRestDays, restDayList: restDayList };
      return result;
    }
  }
};
</script>

<style></style>
