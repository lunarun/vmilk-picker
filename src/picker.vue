<template>
  <div class="v-picker">
    <div class="v-picker__toolbar v-hairline--top-bottom" v-if="showToolbar">
      <slot>
        <div class="v-picker__cancel" @click="emit('cancel')">{{ cancelButtonText}}</div>
        <div class="v-picker__confirm" @click="emit('confirm')">{{ confirmButtonText}}</div>
        <div class="v-picker__title" v-if="title" v-text="title" />
      </slot>
    </div>
    <div class="v-picker__columns" @touchmove.prevent>
      <picker-column
        v-for="(item, index) in currentColumns"
        :key="index"
        :value-key="valueKey"
        :options="item.values"
        :class-name="item.className"
        :default-index="item.defaultIndex"
        :item-height="itemHeight"
        :visible-item-count="visibleItemCount"
        @click="onChange(index)"
      />
      <div class="v-picker__frame v-hairline--top-bottom" :style="frameStyle" />
    </div>
  </div>
</template>

<script>
import PickerColumn from './pickerColumn'
// import deepClone from '../utils/deep-clone';

export default {
  name: 'picker',
  components: {
    PickerColumn
  },
  props: {
    title: String,
    showToolbar: Boolean,
    confirmButtonText: {
      type: String,
      default: '完成'
    },
    cancelButtonText: {
      type: String,
      default: '取消'
    },
    visibleItemCount: Number,
    valueKey: {
      type: String,
      default: 'text'
    },
    itemHeight: {
      type: Number,
      default: 44
    },
    columns: {
      type: Array,
      default: () => []
    }
  },

  data () {
    return {
      children: [],
      currentColumns: []
    }
  },

  created () {
    this.initColumns()
  },

  watch: {
    columns () {
      this.initColumns()
    }
  },

  computed: {
    frameStyle () {
      return {
        height: this.itemHeight + 'px'
      }
    }
  },

  methods: {
    initColumns () {
      const columns = this.columns
      this.isSimpleColumn = columns.length && !columns[0].values
      this.currentColumns = this.isSimpleColumn ? [{ values: columns }] : columns
    },
    emit (event) {
      if (this.isSimpleColumn) {
        this.$emit(event, this.getColumnValue(0), this.getColumnIndex(0))
      } else {
        this.$emit(event, this.getValues(), this.getIndexes())
      }
    },
    onChange (columnIndex) {
      if (this.isSimpleColumn) {
        this.$emit('click', this, this.getColumnValue(0), this.getColumnIndex(0))
      } else {
        this.$emit('click', this, this.getValues(), columnIndex)
      }
    },
    // get column instance by index
    getColumn (index) {
      return this.children[index]
    },
    // get column value by index
    getColumnValue (index) {
      return (this.getColumn(index) || {}).currentValue
    },
    // set column value by index
    setColumnValue (index, value) {
      const column = this.getColumn(index)
      column && column.setValue(value)
    },
    // get column option index by column index
    getColumnIndex (columnIndex) {
      return (this.getColumn(columnIndex) || {}).currentIndex
    },
    // set column option index by column index
    setColumnIndex (columnIndex, optionIndex) {
      const column = this.getColumn(columnIndex)
      column && column.setIndex(optionIndex)
    },
    // get options of column by index
    getColumnValues (index) {
      return (this.currentColumns[index] || {}).values
    },
    // set options of column by index
    setColumnValues (index, options) {
      const column = this.currentColumns[index]
      if (column) {
        column.values = options
      }
    },
    // get values of all columns
    getValues () {
      return this.children.map(child => child.currentValue)
    },
    // set values of all columns
    setValues (values) {
      values.forEach((value, index) => {
        this.setColumnValue(index, value)
      })
    },
    // get indexes of all columns
    getIndexes () {
      return this.children.map(child => child.currentIndex)
    },
    // set indexes of all columns
    setIndexes (indexes) {
      indexes.forEach((optionIndex, columnIndex) => {
        this.setColumnIndex(columnIndex, optionIndex)
      })
    }
  }
}
</script>

<style lang="less" scoped>
  @import "./picker.less";
</style>

