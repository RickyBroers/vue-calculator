<script setup lang="ts">
//bg-[#14532D]
</script>
<template>
  <div class="p-12">
    <div
      class="display h-full w-full bg-[#14532D] rounded-lg flex justify-end items-center p-8 text-5xl"
    >
      <slot name="display"></slot>
    </div>
  </div>
</template>

<style lang="scss">
.display {
  color: white;
  //   font-family: "Digital-7";
}
</style>
