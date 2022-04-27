<template>
  <div
    class="custom-select"
    ref="customSelect"
    :tabindex="tabindex"
    @blur="open = false"
  >
    <div class="selected" @click="openSelectBox">
      {{
        selected
          ? typeof selected == "object"
            ? selected[textItem]
            : selected
          : placeHolder
      }}
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="16"
        height="16"
        fill="currentColor"
        class="bi bi-x-circle"
        @click.stop="deSelectOption"
        v-show="selected"
        viewBox="0 0 16 16"
      >
        <path
          d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"
        />
        <path
          d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"
        />
      </svg>

      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="12"
        height="12"
        fill="currentColor"
        class="bi bi-caret-down-fill caret"
        :class="{ rotate180: open }"
        viewBox="0 0 16 16"
      >
        <path
          d="M7.247 11.14 2.451 5.658C1.885 5.013 2.345 4 3.204 4h9.592a1 1 0 0 1 .753 1.659l-4.796 5.48a1 1 0 0 1-1.506 0z"
        />
      </svg>
    </div>
    <div
      class="items"
      :style="itemsStyles"
      :class="{ selectHide: !open }"
      ref="customSelectItems"
    >
      <div
        v-for="option of options"
        :key="typeof option == 'object' ? option[trackItem] : option"
        @click="selectedOption(option)"
      >
        {{ typeof option == "object" ? option[textItem] : option }}
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "@vue/runtime-core";

export default defineComponent({
  name: "CustomSelect",
  props: {
    modelValue: String,
    options: Array,
    textItem: String,
    trackItem: String,
  },
  data() {
    return {
      tabindex: 0,
      placeHolder: "لطفا یک مورد انتخاب کنید",
      selected: null as string | object | null,
      open: false,
      itemsStyles: {
        transform: "",
      },
    };
  },
  mounted() {
    this.itemsStyles.transform = `translate3d(0px, ${
      (this.$refs.customSelect as any).getBoundingClientRect().height
    }px, 0px)`;
  },
  methods: {
    openSelectBox(): void {
      console.log("openSelectBox");
      this.open = !this.open;
      const offsetTop = (this.$refs.customSelect as any).getBoundingClientRect()
        .top;
      const offsetBottom =
        window.innerHeight -
        (this.$refs.customSelect as any).getBoundingClientRect().bottom;
      if (offsetTop < offsetBottom) {
        this.itemsStyles.transform = `translate3d(0px, ${
          (this.$refs.customSelect as any).getBoundingClientRect().height + 3
        }px, 0px)`;
      } else {
        this.itemsStyles.transform = `translate3d(0px, -100%, 0px)`;
      }
    },
    selectedOption(option: string): void {
      this.selected = option;
      this.open = false;
      this.$emit("update:modelValue", option);
    },
    deSelectOption(): void {
      this.selected = null;
      this.$emit("update:modelValue", null);
    },
  },
});
</script>

<style scoped lang="scss">
$backgroundColor: #f5f8fa;
$color: #7d7d7d;

::-webkit-scrollbar {
  width: 5px;
  border-radius: 100px;
}

::-webkit-scrollbar-track {
  background-color: inherit;
  border-radius: 100px;
}

::-webkit-scrollbar-thumb {
  background-color: darken($backgroundColor, 60%);
  opacity: 0.5;
}

.custom-select {
  max-width: 300px;
  position: relative;
  width: 100%;
  text-align: left;
  outline: none;
  height: 47px;
  line-height: 47px;
  background-color: $backgroundColor;
  color: $color;
  font-size: 0.8rem;
  .selected {
    display: flex;
    flex-flow: row-reverse;
    align-items: center;
    justify-content: space-between;
    padding-left: 1.5em;
    background-color: inherit;
    color: inherit;
    font-size: inherit;
    border-radius: 6px;
    border: 1px solid #e6e6e6;
    padding-right: 1em;
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    user-select: none;
    text-align: right;
    ::after {
      position: absolute;
      content: "";
      top: 22px;
      right: 1em;
      width: 0;
      height: 0;
      border-color: #fff transparent transparent transparent;
    }
    &.open {
      border: 1px solid #ad8225;
      border-radius: 6px 6px 0px 0px;
    }
  }
  .items {
    color: inherit;
    border-radius: 0 0 6px 6px;
    position: absolute;
    background-color: inherit;
    left: 0;
    right: 0;
    top: 0;
    z-index: 10;
    max-height: 200px;
    overflow-y: scroll;
    will-change: transform;
    > div {
      padding-right: 1em;
      padding-left: 1em;
      cursor: pointer;
      user-select: none;
      text-align: right;
      &:hover {
        background-color: darken($backgroundColor, 10%);
      }
    }
  }
}
.selectHide {
  display: none;
}
.caret {
  position: absolute;
  left: 5px;
  top: 50%;
  transform: translate(0, -50%);
  transition: transform 100ms linear;
  &.rotate180 {
    transform: translate(0, -50%) rotate(180deg) !important;
  }
}
</style>
