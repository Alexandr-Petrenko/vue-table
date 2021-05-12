<template>
  <div>
    <div class="list__wrapper" v-if="display">
      <div class="list__row list__row--header">
      <span class="list__item list__item--start">
        Stock
      </span>
        <span class="list__item list__item--center">
        Current
      </span>
        <span class="list__item list__item--end">
         Change
      </span>
      </div>
      <div
          class="list__row"
          v-for="item in data"
          :key="item.name"
      >
      <span class="list__item list__item--start">
        {{ item.name }}
      </span>
        <span class="list__item list__item--center">
        {{ item.current.toFixed(2) }}
      </span>
        <span
            class="list__item list__item--end"
            :style="{color: item.change >= 0 ? 'green' : 'red'}"
        >
        {{ item.change }}
      </span>
      </div>
    </div>
    <div class="list_load" v-if="load">
      Loading...
    </div>
    <div class="list_error" v-if="error">
      No data
    </div>
  </div>
</template>

<script>

export default {
  name: "List",

  props: {
    data: Array,
    error: Boolean,
    load: Boolean,
    display: Boolean,
  },
}
</script>

<style scoped lang="scss">
  .list {
    &__row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: 100%;
      height: 50px;
      box-sizing: border-box;
      padding: 7px;
      border: 1px solid #eee;
      border-top: none;

      &:first-child {
        border-top: 1px solid #eee;
      }

      &--header {
        font-weight: 600;
        background: #eee;
      }
    }

    &__item {
      width: 55px;

      &--start {
        text-align: left;
      }

      &--center {
        text-align: center;
      }

      &--end {
        text-align: right;
      }

    }
  }
</style>
