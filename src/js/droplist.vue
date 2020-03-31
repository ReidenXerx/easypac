<template>
<select>
  <option v-model="item.value" v-for="(item, index) in items">{{item.value}}</option>
</select>
</template>

<script>
export default {
}
</script>
