<template>
  <div class="event-card">
    <div
      class="event-card__content"
      :class="{ [`event-card__content_${event.type}`]: event.type }"
      @click="$emit('click', event, $event)"
    >
      {{ event.title }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    event: {
      type: Object,
      default() {
        return {};
      },
    },
  },
};
</script>

<style lang="scss" scoped>
.event-card {
  height: 20px;
  &__content {
    cursor: pointer;
    font-size: 12px;
    background-color: #d2fdc7;
    border-radius: 6px;
    color: rgba(0, 0, 0, 0.87);
    padding: 0 0 0 4px;
    height: 18px;
    line-height: 18px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    &_low {
      background-color: #d2fdc7;
    }
    &_middle {
      background-color: #fde7c7;
    }
    &_high {
      background-color: #ff8282;
    }
    &:hover {
      position: absolute;
    }
  }
}
</style>
