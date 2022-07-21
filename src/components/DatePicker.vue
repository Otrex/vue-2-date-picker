<template>
<div class="w-max border border-emerald-900 border-2"> 
  <div class="flex flex-row ">
    <div class="w-[300px] p-4">
      <!-- Head Controls Component -->
      <div class="flex flex-row justify-between mb-5" data-name="head">
        <div class="flex flex-row items-center gap-2" @click="showCalender = !showCalender">
          <span class="text-black font-semibold text-base cursor-pointer" style="--color: #fff"> {{MONTH_MAP[selectedMM]}} {{selectedYY}} </span>
          <i :class="[!showCalender && 'transform rotate-90']">
            <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M10.0947 8.14134C10.1745 8.06356 10.1745 7.93644 10.0947 7.85866L5.05981 2.9483C4.98006 2.87052 4.98006 2.7434 5.05981 2.66563L6.19622 1.55732C6.27459 1.48089 6.40064 1.48089 6.47901 1.55732L12.9402 7.85866C13.0199 7.93644 13.0199 8.06356 12.9402 8.14134L6.47901 14.4427C6.40064 14.5191 6.27459 14.5191 6.19622 14.4427L5.05981 13.3344C4.98006 13.2566 4.98006 13.1295 5.05981 13.0517L10.0947 8.14134Z" fill="#A4A5A4"/>
            </svg>
          </i>
        </div>

        <div class="flex flex-row gap-6 items-center">
          <i data-name="go-left" class="hover:bg-slate-200" @click="changeMonthOrYear(-1)">
            <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M5.90527 7.85866C5.82552 7.93644 5.82552 8.06356 5.90527 8.14134L10.9402 13.0517C11.0199 13.1295 11.0199 13.2566 10.9402 13.3344L9.80378 14.4427C9.72541 14.5191 9.59936 14.5191 9.52099 14.4427L3.05981 8.14134C2.98006 8.06356 2.98006 7.93644 3.05981 7.85866L9.52099 1.55732C9.59936 1.48089 9.72541 1.48089 9.80378 1.55732L10.9402 2.66562C11.0199 2.7434 11.0199 2.87052 10.9402 2.94829L5.90527 7.85866Z" fill="#A4A5A4"/>
            </svg>
          </i>
          <i data-name="go-right" class="hover:bg-slate-200" @click="changeMonthOrYear(+1)">
            <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M10.0947 8.14134C10.1745 8.06356 10.1745 7.93644 10.0947 7.85866L5.05981 2.9483C4.98006 2.87052 4.98006 2.7434 5.05981 2.66563L6.19622 1.55732C6.27459 1.48089 6.40064 1.48089 6.47901 1.55732L12.9402 7.85866C13.0199 7.93644 13.0199 8.06356 12.9402 8.14134L6.47901 14.4427C6.40064 14.5191 6.27459 14.5191 6.19622 14.4427L5.05981 13.3344C4.98006 13.2566 4.98006 13.1295 5.05981 13.0517L10.0947 8.14134Z" fill="#A4A5A4"/>
            </svg>
          </i>
        </div>
      </div>
      <!-- Calender Component -->
      <div class="" data-name="calender" v-show="showCalender">
        <!-- <CalenderTable v-model="selectedDate"/> -->
        <div>
          <div class="flex flex-row" data-name="weekdays">
            <div v-for="(days, idx) in ['M', 'T', 'W', 'T', 'F', 'S', 'S']" :key="idx" class="w-full flex items-center justify-center aspect-square"> {{days}} </div>
          </div>

          <div class="grid grid-cols-7">
            <div v-for="(n, idx) in days" :key="idx" @click="changeDD(n)" :class="[ selectedDD === n && 'active__dd', today.getDate() === n && today.getFullYear() === selectedYY && today.getMonth() === selectedMM && 'current__dd', 'cursor-pointer flex items-center rounded-sm justify-center aspect-square']">{{n || ''}}</div>
          </div>
        </div>
      </div>

      <div class="" data-name="months-calender" v-show="!showCalender">
        <div class="grid grid-cols-3">
          <div v-for="(yy, idx) in range(selectedYY, selectedYY + 11)" :key="idx" @click="changeYear(yy)" :class="[ selectedYY === yy && 'active__yy', 'cursor-pointer hover:bg-slate-200 flex items-center rounded-sm justify-center py-2']">
            {{ yy }}
          </div>
        </div>
      </div>
    </div>

    <!-- Time Picker Component -->
    <div class="w-2/6 p-4 pl-0" data-name="time-picker">
      <div class="w-full">
        <!-- Time Input Component -->
        <div data-id="time-input" class="inline-block relative h-max overflow-hidden">
          <input type="text" placeholder="00:00" v-model="selectedTime" class="block pl-8 bg-transparent ml-1 w-full appearance-none border-none"/>
          <i class="absolute top-1/2 left-0 flex items-center transform -translate-y-1/2 mx-1 h-full p-2">
            <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M14.6 8C14.6 11.6451 11.6451 14.6 8 14.6C4.35492 14.6 1.4 11.6451 1.4 8C1.4 4.35492 4.35492 1.4 8 1.4C11.6451 1.4 14.6 4.35492 14.6 8ZM16 8C16 12.4183 
            12.4183 16 8 16C3.58172 16 0 12.4183 0 8C0 3.58172 3.58172 0 8 0C12.4183 0 16 3.58172 16 8ZM8.70078 4H7.30078V8.37463L10.6125 10.5824L11.3891 9.41756L8.70078 7.62537V4Z" fill="#545455"/>
            </svg>
          </i>
        </div>

        <!-- AM/PM Picker Component-->
        <div class="p-1 w-full">
          <ul :current-tabindex="currentTab" class="m-0 p-0 flex flex-row w-full">
            <li tabindex="0" :class="[ currentTab === 0 && 'active', 'py-2 w-1/2 rounded cursor-pointer' ]" @click="currentTab = 0"> AM </li>
            <li tabindex="1" :class="[ currentTab === 1 && 'active', 'py-2 w-1/2 rounded cursor-pointer' ]" @click="currentTab = 1"> PM </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</div>
</template>
<script>
const MONTH_MAP = Object.freeze([
  "January",
  "February",
  "March",
  "April",
  "May",
  "June",
  "July",
  "August",
  "September",
  "October",
  "November",
  "December",
]);

export default {
  name: 'DatePicker',
  components: {
  },
  props: ['value'],
  mounted() {
    //TODO get selected date from v-model
    const date = new Date(this.value);
    this.setCurrentDate(date);

    // TIME
    this.selectedMin = date.getMinutes();
    this.selectedHrs = date.getHours();
  },
  data: () => ({
    selectedMin: '',
    selectedHrs: '',
    showCalender: true,
    currentTab: 0,
    selectedDate: '',
    selectedDD: undefined,
    selectedMM: undefined,
    selectedYY: undefined,
    totalDays: 0,
    today: new Date(),
    firstWeekday: 0,
    MONTH_MAP
  }),

  watch: {
    selectedDate(old, $new) {
      if (old !== $new) {
        this.$emit('input', this.selectedDate);
        this.$emit('update:modelValue', this.selectedDate);
      }
    }
  },
  computed: {
    days() {
      const datesViewArray = []
      const dates = this.range(1, this.totalDays);
      let currentIndex = -7;

      // eslint-disable-next-line no-constant-condition
      while (true) {
        if (datesViewArray[datesViewArray.length - 1] === dates[dates.length - 1]) break;
        if (currentIndex >= this.firstWeekday) {
          datesViewArray.push(dates[currentIndex - this.firstWeekday])
        } else {
          datesViewArray.push(0)
        }
        currentIndex += 1;
      }
      const first7 = datesViewArray.splice(0, 7);
      
      if (first7.reduce((s, n) => s + n, 0) !== 0) {
        datesViewArray.unshift(...first7);
      }

      return datesViewArray;
    },

    selectedTime: {
      get: function () {
        return `${this.selectedHrs || 0}:${this.selectedMin || 0}`;
      },
      set: function (v) {
        const [hrs, mint] = v.split(':');
        this.selectedMin = +mint > 60 ? 60: mint;
        this.selectedHrs = +hrs > 12 ? 12: hrs;
      }
    }
  },
  methods:{
    setCurrentDate(date) {
      this.selectedDate = date;
      this.selectedDD = date.getDate();
      this.selectedMM = date.getMonth();
      this.selectedYY = date.getFullYear();
      this.firstWeekday = this.getFirstDayOfMonth(date.getFullYear(), date.getMonth()) - 1;
      this.totalDays = this.getDaysInMonth(date.getFullYear(), date.getMonth());
      this.$emit('input', this.selectedDate);
      this.$emit('update:modelValue', this.selectedDate);
    },
    range(start, stop, skip = 1) {
      // console.log(start, stop)
      const arrayLength = Math.floor(stop/skip) - start;
      const array = arrayLength > 0 
        ? new Array(arrayLength + 1)
          .fill(0).map((e, idx) => start + (idx * skip))
        : [];
      return array;
    },
    getDaysInMonth(year, month) {
      // This is to adjust for the because 0 gets the previous months last date
      return new Date(year, month + 1, 0).getDate();
    },
    getFirstDayOfMonth(year, month) {
      // weekday
      return new Date(year, month, 1).getDay(); 
    },
    subtractMonths(numOfMonths, date = new Date()) {
      date.setMonth(date.getMonth() - numOfMonths);
      return date;
    },
    addMonths(numOfMonths, date = new Date()) {
      date.setMonth(date.getMonth() + numOfMonths);
      return date;
    },
    addYear(num, date = new Date()) {
      date.setFullYear(date.getFullYear() + num);
      return date;
    },
    subtractYear(num, date = new Date()) {
      date.setFullYear(date.getFullYear() - num);
      return date;
    },
    changeMonthOrYear(direction) {
      let $date = this.selectedDate;

      if (direction === -1) {
        if (this.showCalender) {
          $date = this.subtractMonths(1, this.selectedDate)
        } else {
          $date = this.subtractYear(1, this.selectedDate)
        }
      }

      if (direction === 1) {
        if (this.showCalender) {
          $date = this.addMonths(1, this.selectedDate)
        } else {
          $date = this.addYear(1, this.selectedDate)
        }
      }

      this.setCurrentDate($date);
    },
    changeDD(dd) {
      this.selectedDate.setDate(dd);
      this.setCurrentDate(this.selectedDate)
    },
    changeYear(yy) {
      this.selectedDate.setFullYear(yy);
      this.setCurrentDate(this.selectedDate)
    }
  }
}
</script>
<style scoped>
/* { border: 1px solid red; }
/*
 use css variables in the future to pass the active color
*/
.active {
  background-color: darkgray;
  color: white;
}

.active__dd {
  background-color: rgb(76, 44, 44);
  color: white;
}

.current__dd {
  background-color: rgb(209, 166, 166);
  color: rgb(6, 15, 119);
}

.active__yy {
  background-color: rgb(39, 188, 226);
  color: white;
}
</style>