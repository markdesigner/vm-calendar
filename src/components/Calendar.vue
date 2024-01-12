<template>
  <div class="calendar">
    <div class="calendar-content" v-if="isShowCalendar">
      <div class="year selection">
        <button @click="adjustYear('minus')">-</button>
        <div class="title">{{ selectorDateMap.year }} 年</div>
        <button @click="adjustYear('add')">+</button>
      </div>
      <div class="month selection">
        <button @click="adjustMonth('minus')">-</button>
        <div class="title">{{ selectorDateMap.month + 1 }} 月</div>
        <button @click="adjustMonth('add')">+</button>
      </div>
      <div class="date selection">{{ selectorDateMap.date }} 日</div>
      <div class="date-selector">
        <div class="date-grid">
          <div class="day-title">
            <div class="day" v-for="(day, index) in 7" :key="index">
              {{ mapDayTitle(day) }}
            </div>
          </div>
          <div class="grid">
            <div class="week" v-for="(week, index) in 6" :key="index">
              <div class="day" v-for="(day, index) in 7" :key="index">
                {{ generateDayName(week, day) }}
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app-calendar",
  props: {
    msg: String,
  },
  data() {
    return {
      isShowCalendar: true,
      todayDateInstance: null,
      selectorDateInstance: null,
      selectorDateMap: {},
      monthList: []
    };
  },
  computed: {
    firstDateOfMonth() {
      const date = new Date(this.selectorDateMap.year, this.selectorDateMap.month, 1);
      return new Date(
        this.selectorDateMap.year,
        this.selectorDateMap.month,
        1 - date.getDay()
      ).getDate();
    },
  },
  methods: {
    openCalendar() {
      this.isShowCalendar = !this.isShowCalendar;
    },
    mapDayTitle(day) {
      const dayName = ["一", "二", "三", "四", "五", "六", "日"];
      return dayName[day - 1];
    },
    generateMonthMap() {
      let monthList = []
        const preMonthLastDateInstance = new Date(this.selectorDateMap.year, this.selectorDateMap.month , 0)
        const preMonthLastDate = preMonthLastDateInstance.getDate();
        const diff = preMonthLastDate - this.firstDateOfMonth;
        for(let i = 0; i <= diff; i +=1) {
          monthList.push({
            date: preMonthLastDate - (diff - i)
          })
        }
        const thisMonthLastDate = new Date(this.selectorDateMap.year, this.selectorDateMap.month + 1 , 0).getDate()
        for(let i = 1; i <= thisMonthLastDate; i += 1) {
          monthList.push({
            date: i

          })
        }
        const remainingDayLength = 42 - monthList.length;
        for(let i = 1; i <= remainingDayLength; i += 1){
          monthList.push({
            date: i
          })
        }
        this.monthList = monthList;
    },
    generateDayName(week, day) {
      const weekIndex = week - 1;
      const dayIndex = day - 1;
      const date = weekIndex * 7 + dayIndex;
      return this.monthList[date].date
    },
    adjustMonth(action) {
      if(action === 'minus') {
        this.selectorDateMap.month -=1;
      }else {
        this.selectorDateMap.month +=1;
      }
      this.generateMonthMap();
    },
    adjustYear(action) {
      if(action === 'minus') {
        this.selectorDateMap.year -=1;
      }else {
        this.selectorDateMap.year +=1;
      }
      this.generateMonthMap();
    }
  },
  created() {
    this.todayDate = new Date();
    this.selectorDateMap = {
      year: this.todayDate.getFullYear(),
      month: this.todayDate.getMonth(),
      date: this.todayDate.getDate(),
    };
    this.generateMonthMap();
  },
};
</script>

<style lang="scss" scoped>
.calendar {
  .calendar-content {
    display: flex;
    flex-direction: column;
    align-items: center;
    .selection {
      display: flex;
      margin: 10px 0;
    }
  }
}
.date-selector {
  .date-grid {
    .day-title {
      display: flex;
      .day {
        width: 50px;
        height: 50px;
        background-color: pink;
      }
    }
    .grid {
      .week {
        display: flex;
        .day {
          width: 50px;
          height: 50px;
          background-color: #b7cdde;
        }
      }
    }
  }
}
</style>


