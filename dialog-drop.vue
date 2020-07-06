<template>
  <div id="dragMain" v-drag v-show="flag">
    <div class="drag-close">
      <p class="drag-title">{{ title }}</p>
      <i class="el-icon-arrow-up drag-icon" v-if="showSlot" @click="changeSlotStatus()"></i>
      <i class="el-icon-arrow-down drag-icon" v-if="!showSlot" @click="changeSlotStatus()"></i>
      <i class="el-icon-close drag-icon" @click="close()"></i>
    </div>
    <div ref="child" :class="{ showOrHide: !showSlot }">
      <slot name="child"> </slot>
    </div>
  </div>
</template>

<script>
/* eslint-disable space-before-function-paren,no-redeclare,prefer-const,one-var */
import EventBus from '@/components/bus/index.js'

export default {
  components: {},
  data() {
    return {
      flag: false,
      title: '',
      showSlot: true,
    }
  },
  directives: {
    drag: {
      // 指令的定义
      bind: function (el) {
        let odiv = el
        el.onmousedown = (e) => {
          let disX = e.clientX - odiv.offsetLeft
          let disY = e.clientY - odiv.offsetTop
          let left = ''
          let top = ''
          document.onmousemove = (e) => {
            left = e.clientX - disX
            top = e.clientY - disY
            odiv.style.left = left + 'px'
            odiv.style.top = top + 'px'
          }
          document.onmouseup = (e) => {
            document.onmousemove = null
            document.onmouseup = null
          }
        }
      },
    },
  },
  methods: {
    close() {
      this.flag = false
    },
    changeSlotStatus() {
      this.showSlot = !this.showSlot
    },
  },
  created() {
    console.log(this)
    EventBus.$on('title', (msg) => {
      this.title = msg
    })
  },
  mounted() {},
}
</script>

<style scoped lang="scss">
#dragMain {
  width: auto;
  height: auto;
  display: inline-block;

  .drag-close {
    display: flex;
    line-height: 30px;
    height: 30px;
    align-items: center;
    background: #ededed;
    padding: 0 10px;
    border: #ccc 1px solid;

    .drag-title {
      font-weight: bold;
      flex-grow: 1;
    }

    .drag-icon {
      margin-left: 10px;
    }
  }

  #child {
    border-bottom: #ccc 1px solid;
    border-left: #ccc 1px solid;
    border-right: #ccc 1px solid;
  }

  .showOrHide {
    visibility: hidden;
  }
}
</style>
