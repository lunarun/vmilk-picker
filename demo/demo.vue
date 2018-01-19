<template>
  <div class="l-picker">
    <v-picker :columns="columns" title="标题" show-toolbar @click="onClick"></v-picker>
  </div>
</template>

<script>
import VPicker from '../index'
export default {
  name: 'picker',
  components: {
    VPicker
  },
  data () {
    return {
      columns: ['杭州', '宁波', '温州', '嘉兴', '湖州', '杭州', '宁波', '温州', '嘉兴', '湖州', '杭州', '宁波', '温州', '嘉兴', '湖州']
    }
  },
  methods: {
    onClick (picker, value, index) {
      console.log(`当前值：${value}, 当前索引：${index}`)
    }
  }
}
</script>

<style lang="less">
  .l-picker {
    width: 100%;
  }
</style>

