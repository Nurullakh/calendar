<template>
  <div class="header">
    <div class="header__prev" @click="prev" />
    <div class="header__title">{{ title }}</div>
    <div class="header__next" @click="next" />
  </div>
</template>

<script>
import moment from "moment";

export default {
  props: {
    currentMonth: {
      type: Object,
      reuired: true,
    },
    locale: {
      type: String,
      default: "ru",
    },
  },
  computed: {
    title() {
      if (moment().format("YYYY") === this.currentMonth.format("YYYY")) {
        return this.currentMonth.locale(this.locale).format("MMMM");
      }
      return this.currentMonth.locale(this.locale).format("MMMM YYYY");
    },
  },
  methods: {
    prev() {
      var newMonth = moment(this.currentMonth)
        .subtract(1, "months")
        .startOf("month");
      this.$emit("change", newMonth);
    },
    next() {
      var newMonth = moment(this.currentMonth)
        .add(1, "months")
        .startOf("month");
      this.$emit("change", newMonth);
    },
  },
};
</script>

<style lang="scss" scoped>
.header {
  display: flex;
  align-items: center;
  &__prev,
  &__next {
    width: 17px;
    height: 17px;
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
    cursor: pointer;
  }
  &__prev {
    background-image: url("../assets/arrow-left.png");
  }
  &__next {
    background-image: url("../assets/arrow-right.png");
  }
  &__title {
    margin: 0 10px;
    color: #a183ba;
    text-transform: uppercase;
    font-weight: 500;
  }
}
</style>
