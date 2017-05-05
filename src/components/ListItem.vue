<template>
  <el-form-item class="item" :prop="item.key">
    <el-checkbox v-model="tracking.filter()[].checked" >{{ item.label }}</el-checkbox>
    <h3>{{ item.desc }}</h3>
  </el-form-item>
</template>

<script>
export default {
  name: 'list-item',
  props: {
    item: {
      type: Object,
      required: true
    }
  }
}
</script>

<style>
</style>
