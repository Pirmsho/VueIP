<template>
  <div
    class="
      absolute
      -bottom-64
      lg:-bottom-20
      flex flex-col
      lg:flex-row
      bg-white
      gap-y-4 gap-x-12
      px-8
      pt-8
      pb-12
      rounded-md
    "
  >
    <div class="flex flex-col">
      <h3 class="text-xs mb-2 uppercase">IP Address</h3>
      <span class="text-xl">{{ IpInfo.address }}</span>
    </div>
    <div class="flex flex-col">
      <h3 class="text-xs mb-2 uppercase">Location</h3>
      <span class="text-xl">{{ IpInfo.state }}</span>
    </div>
    <div class="flex flex-col">
      <h3 class="text-xs mb-2 uppercase">City</h3>
      <span class="text-xl">{{ IpInfo.city }}</span>
    </div>
    <div class="flex flex-col">
      <h3 class="text-xs mb-2 uppercase">Timezone</h3>
      <span class="text-xl">UTC {{ IpInfo.timezone }}</span>
    </div>
    <div class="flex flex-col">
      <h3 class="text-xs mb-2 uppercase">ISP</h3>
      <span class="text-xl">{{ IpInfo.isp }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["IpInfo"],
};
</script>

