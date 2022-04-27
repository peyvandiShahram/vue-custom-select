<template>
  <div class="row">
    <div class="col-md-4 offset-4 mt-5">
      <!--      options can be array of string or array of objects-->
      <CustomSelect
        :options="options"
        v-model="optionItem"
        text-item="title"
        track-item="id"
        :clearable="false"
      />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "@vue/runtime-core";
import CustomSelect from "@/components/CustomSelect.vue";
export default defineComponent({
  name: "App",
  components: {
    CustomSelect,
  },
  data() {
    return {
      options: [
        {
          id: 1,
          title: "option 1",
        },
        {
          id: 2,
          title: "option 2",
        },
        {
          id: 3,
          title: "option 3",
        },
        {
          id: 4,
          title: "option 4",
        },
        {
          id: 5,
          title: "option 5",
        },
      ],
      optionItem: "",
    };
  },
});
</script>

<style lang="scss"></style>
