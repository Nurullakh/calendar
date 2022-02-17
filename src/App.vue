<template>
  <div id="app">
    <Calendar :events="events" locale="ru" />
  </div>
</template>
<script>
import Calendar from "./components/Calendar.vue";

export default {
  components: {
    Calendar,
  },
  data() {
    return {
      events: [
        {
          id: 1,
          title: "Выполнить тестовое задание",
          date: "2022-02-25",
          type: "low",
        },
        {
          id: 2,
          title: "Выполнить тестовое задание",
          date: "2022-02-26",
          type: "high",
        },
        {
          id: 3,
          title: "Выполнить тестовое задание",
          date: "2022-02-27",
          type: "middle",
        },
        {
          id: 4,
          title: "Выполнить тестовое задание",
          date: "2022-02-27",
          type: "low",
        },
        {
          id: 5,
          title: "Выполнить тестовое задание",
          date: "2022-02-27",
          type: "middle",
        },
        {
          id: 6,
          title: "Выполнить тестовое задание",
          date: "2022-02-26",
          type: "middle",
        },
        {
          id: 7,
          title: "Выполнить тестовое задание",
          date: "2022-01-27",
          type: "low",
        },
        {
          id: 8,
          title: "Выполнить тестовое задание",
          date: "2022-01-27",
          type: "high",
        },
        {
          id: 9,
          title: "Выполнить тестовое задание",
          date: "2022-03-27",
          type: "middle",
        },
      ],
    };
  },
};
</script>
<style lang="scss">
* {
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
}
</style>
