<template>
  <div class="container">
    <ul>
      <li v-for="(item, index) in list" :key="item.key" @click="handClick(item, index)"
        :class="{ 'active': item.checked }">
        {{ item.name }}
      </li>
    </ul>
    <button type="button" @click="submit()">提交</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      list: [
        {
          name: '时段1',
          key: 0,
          type: null, // 起始start
          checked: false
        }, {
          name: '时段2',
          key: 1,
          type: null,
          checked: false
        }, {
          name: '时段3',
          key: 2,
          type: null,
          checked: false
        }, {
          name: '时段4',
          key: 3,
          type: null,
          checked: false
        }, {
          name: '时段5',
          key: 4,
          type: null,
          checked: false
        }
      ]
    }
  },
  methods: {
    handClick(item, index) {
      const checked = this.list.some(i => i.checked) // 是否已有选中
      const startIndex = this.list.findIndex(i => i.type === 'start') // 开始下标
      if (checked) {
        if (startIndex > index) {
          // 1、选择起始卡片之前的区域，只有此卡片选中，并设置为起始卡片
          this.list = this.beforeArea(this.list, index)
        } else if (startIndex === index) {
          // 2、选中起始卡片
          this.list = this.activeArea(this.list, index)
        } else if (startIndex < index) {
          // 3、选中起始卡片位置后的卡片，从起始位置后到点击位置都要选中
          this.list = this.afterArea(this.list, startIndex, index)
        }
      } else {
        // 未选择中时
        this.list.splice(index, 1, this.formatData(item, true, 'start'))
      }
    },
    beforeArea(list, index) {
      return list.map((item, ind) => index === ind ? this.formatData(item, true, 'start') : this.formatData(item))
    },
    activeArea(list, index) {
      // 2.1、当选中的卡片大于1个时，只有起始卡片选择，其余都关闭
      // 2.2、当选中的卡片只有1个时，该卡片关闭
      const length = list.filter(i => i.checked).length // 已选中长度
      return length > 1
        ? list.map((ev, ind) => ind !== index ? this.formatData(ev) : ev)
        : list.map((ev, ind) => ind === index ? this.formatData(ev) : ev)
    },
    afterArea(list, startIndex, index) {
      return list.map((item, ind) => {
        if (ind === startIndex) {
          // 3.1、起始位置不变
          return item
        } else if (ind > startIndex && ind <= index) {
          // 3.2、起始位置之后到点击位置区域都选中
          return this.formatData(item, true)
        }
        // 3.3、其余位置都关闭
        return this.formatData(item)
      })
    },
    formatData(item, checked = false, type = null) {
      return {
        ...item,
        checked,
        type
      }
    },
    submit() {
      const checkes = this.list.filter(i => i.checked)
      window.alert(`你选择的是\n${JSON.stringify(checkes)}`)
    }
  }
}
</script>
<style scoped>
.container {
  width: 100%;
  height: 600px;
  background-color: rgba(153, 153, 153, 0.5);
  justify-content: center;
  align-items: center;
  padding: 20px;
}
ul {
  width: 400px;
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  list-style-type: none;
}

ul li {
  width: 120px;
  height: 50px;
  text-align: center;
  line-height: 50px;
  border: 1px solid #333;
  margin-bottom: 15px;

}

ul li.active {
  background-color: rgb(90, 90, 235);
  color: #fff;
}
</style>
