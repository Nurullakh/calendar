<template>
  <div class="calendar">
    <Header :current-month="currentMonth" :locale="locale" @change="setMonth" />
    <div class="calendar__content">
      <div class="calendar__weeks">
        <div class="calendar__week" v-for="dayIndex in 7" :key="dayIndex">
          {{ getWeekDay(dayIndex, locale) }}
        </div>
      </div>
      <div class="calendar__dates">
        <div
          class="calendar__row"
          v-for="(week, index) in calendar"
          :key="index"
        >
          <div class="calendar__day" v-for="(day, index) in week" :key="index">
            <div
              class="calendar__day-content"
              :class="{
                'calendar__day-content_today': day.isToday,
                'calendar__day-content_other-mounth': !day.isCurMonth,
                'calendar__day-content_weekend':
                  day.weekDay === 6 || day.weekDay === 7,
              }"
            >
              <div class="calendar__day-number">{{ day.monthDay }}</div>
              <div class="calendar__event-wrapper">
                <EventCard
                  :event="event"
                  v-for="event in day.events"
                  :key="event.id"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import moment from "moment";
import EventCard from "./EventCard.vue";
import Header from "./Header.vue";

export default {
  components: {
    EventCard,
    Header,
  },
  props: {
    events: {
      type: Array,
      default() {
        return [];
      },
    },
    locale: {
      type: String,
      default: "ru",
    },
  },
  data() {
    return {
      currentMonth: moment().startOf("month"),
    };
  },
  computed: {
    calendar() {
      return this.getCalendar();
    },
  },
  mounted() {
    this.setMonth(this.currentMonth);
  },
  methods: {
    setMonth(firstDayOfMonth) {
      this.currentMonth = firstDayOfMonth;
    },
    getStartDate(date) {
      let start = moment(date);
      let startOfMonth = date.startOf("month");
      start.subtract(startOfMonth.day(), "days");

      if (startOfMonth.day() < 1) {
        start.subtract(7, "days");
      }

      start.add(1, "days");

      return start;
    },
    getCalendar() {
      let monthViewStartDate = this.getStartDate(this.currentMonth);
      let calendar = [];
      for (let perWeek = 1; perWeek <= 6; perWeek++) {
        if (perWeek === 6 && monthViewStartDate.date() < 30) {
          break;
        }
        let week = [];

        for (let day = 1; day <= 7; day++) {
          week.push({
            monthDay: monthViewStartDate.date(),
            isToday: monthViewStartDate.isSame(moment(), "day"),
            isCurMonth: monthViewStartDate.isSame(this.currentMonth, "month"),
            weekDay: day,
            date: moment(monthViewStartDate),
            events: this.getEvents(monthViewStartDate.format()),
          });

          monthViewStartDate.add(1, "day");
        }

        calendar.push(week);
      }
      return calendar;
    },
    getEvents(date) {
      let dayEvents = this.events.filter((day) => {
        let dataMoment = moment(day.date);

        return dataMoment.isSame(date);
      });

      return dayEvents;
    },
    getWeekDay(weekday, locale) {
      const localMoment = moment().locale(locale);
      return localMoment.localeData().weekdays()[weekday % 7];
    },
  },
};
</script>
<style lang="scss" scoped>
.calendar {
  margin: 0 auto;
  max-width: 960px;
  padding: 25px;
  background: #fff;
  border: 1px solid #ededed;
  border-radius: 20px;
  &__content {
    margin-top: 20px;
  }
  &__weeks {
    margin-bottom: 10px;
    display: flex;
  }
  &__week {
    flex: 1;
    text-align: right;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: 500;
  }
  &__dates {
    position: relative;
  }
  &__row {
    border-left: 1px solid #e0e0e0;
    display: flex;
  }
  &__day {
    flex: 1;
    height: 112px;
    padding: 2px;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  &__day-content {
    height: 100%;
    border: 1px solid #ededed;
    border-radius: 5px;
    padding: 5px;
    overflow-y: scroll;
    -ms-overflow-style: none;
    scrollbar-width: none;
    &::-webkit-scrollbar {
      display: none;
    }
    &_other-mounth {
      background-color: #ddd;
      .calendar__day-number {
        color: rgba(0, 0, 0, 0.24);
      }
    }

    &_weekend {
      .calendar__day-number {
        color: #a183ba;
      }
    }
    &_today {
      border: 1px solid #bdbdbd;
      .calendar__day-number {
        color: #41ee00;
      }
    }
  }
  &__day-number {
    text-align: right;
    font-weight: 500;
  }
  &__event-wrapper {
    margin-top: 5px;
  }
}
</style>
