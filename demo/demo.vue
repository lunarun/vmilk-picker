<template>
  <div class="demo-container">
    <div class="l-picker">
      <p>基础使用</p>
      <!-- <v-picker :columns="columns" title="标题" show-toolbar @click="onClick" @cancel="handleCancel" @confirm="handleConfirm"></v-picker> -->
    </div>

    <div class="l-picker">
      <div class="picker-toolbar">
        <span @click="handleReselect(index)" v-for="(item, index) in list" :class="{'active': index === currentNavIndex}">{{item.text}}</span>
        <!-- <span v-if="isCity" @click="handleReselect('city')">{{city}}</span>
        <span v-if="isDistict">{{distict}}</span> -->
      </div>
      <v-picker :columns="columns" @click="onClick"  @cancel="handleCancel" @confirm="handleConfirm"></v-picker>
    </div>
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
      columns: ['杭州', '宁波', '温州', '嘉兴', '湖州', '杭州', '宁波', '温州', '嘉兴', '湖州', '杭州', '宁波', '温州', '嘉兴', '湖州'],
      columnsArrays: [['杭州', '宁波', '温州', '嘉兴', '湖州'], ['宁波', '温州', '嘉兴', '湖州', '杭州', '宁波', '温州', '嘉兴', '湖州', '宁波', '温州', '嘉兴', '湖州', '杭州', '宁波', '温州', '嘉兴', '湖州'], ['宁波', '温州', '嘉兴', '湖州']],
      columnsArray: [[], [], []],
      list: [{text: '选择'}, {text: ''}, {text: ''}],
      currentIndex: 0,
      currentNavIndex: 0
    }
  },
  methods: {
    onClick (picker, value, index) {
      console.log(index)
      console.log(this.currentNavIndex)
      if (this.currentIndex === index) {
        this.list[this.currentNavIndex].text = value
        // 请求接口返回城市数组
        this.columns = this.columnsArrays[1]
        this.columnsArray[this.currentNavIndex] = this.columnsArrays[1]
        this.currentIndex = 0
        if (this.currentNavIndex < this.list.length - 1) {
          this.list[this.currentNavIndex + 1].text = this.list[this.currentNavIndex + 1].text ? this.list[this.currentNavIndex + 1].text : '请选择'
          this.currentNavIndex += 1
        } else {
          console.log('have selected')
        }
      } else {
        this.currentIndex = index
      }
    },
    handleReselect (navIndex) {
      console.log(navIndex)
      this.currentIndex = 0
      this.currentNavIndex = navIndex
      this.columns = this.columnsArray[navIndex]
    },
    handleCancel (value, index) {
      console.log(`当前值：${value}, 当前索引：${index}`)
    },
    handleConfirm (value, index) {
      console.log(`当前值：${value}, 当前索引：${index}`)
    }
  }
}
</script>

<style lang="less">

  @easing-in-out: cubic-bezier(0.35, 0, 0.25, 1);
  @effect-duration: .3s;

  .demo-container {
    width: 100%;
  }

  .l-picker {
    margin-top: 20px;
  }

  .picker-toolbar {
    display: flex;
    span {
      width: 33.3%;
      text-align: center;
    }
    .active {
      border-bottom: 1px solid red;
    }
  }
</style>

