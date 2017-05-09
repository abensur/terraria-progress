<template>
  <div class="tip">
    <el-popover
      ref="popover"
      placement="bottom"
      width="320"
      trigger="click">
      <ul>
        <li v-for="tip in tips">
          {{ tip }}
        </li>
      </ul>
    </el-popover>
    <el-button v-popover:popover>
      <img src="../assets/fishermans_pocket_guide.png" />
    </el-button>
  </div>
</template>
<script>
export default {
  name: 'tip',
  props: {
    tips: {
      type: Array,
      required: true
    }
  }
}
</script>
<style>
.tip {
  display: inline;
}
.tip button {
  padding: 0;
  border: none;
}
.tip img {
  width: 16px;
}
.el-popover ul,
.el-popover li {
  margin: 0;
  padding: 0;
  color: #1f2d3d;
}
.el-popover ul {
  padding: 0 0 0 1.5rem;
}
.el-popover li {
  line-height: 2;
}
</style>
