<template>
  <div>
    <button @click="onLine">连线</button>
    <div id="diagramContainer" class="jsplumbContainer">
      <ul id="source-table" class="left">
        <li>
          <div>来源表字段名</div>
          <div>类型</div>
        </li>
        <li
          v-for="itemLeft in leftData"
          :id="'left_' + itemLeft.id"
          :key="itemLeft.id"
        >
          <div>{{ itemLeft.label }}</div>
          <div>int</div>
        </li>
      </ul>
      <ul id="target-table" class="right">
        <li>
          <div>目标表字段名</div>
          <div>类型</div>
        </li>
        <li
          v-for="itemRight in rightData"
          :id="'right_' + itemRight.id"
          :key="itemRight.id"
        >
          <div>{{ itemRight.label }}</div>
          <div>int</div>
        </li>
      </ul>
    </div>
  </div>

</template>

<script>
import { jsPlumb } from 'jsplumb'
import Sortable from 'sortablejs'
export default {
  data() {
    return {
      plumb: null,
      leftData: [
        {
          id: 1,
          label: '来源1'
        }, {
          id: 2,
          label: '来源2'
        }, {
          id: 3,
          label: '来源3'
        }
      ],

      rightData: [
        {
          id: 4,
          label: '去向1'
        }, {
          id: 5,
          label: '去向2'
        }, {
          id: 6,
          label: '去向3'
        }, {
          id: 7,
          label: '去向4'
        }
      ]
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.initJsplumb()

      // 排序
      this.initSortable()
    })
  },
  destroyed() {
    // 删除全部连接
    // this.plumb.deleteEveryConnection()
    // 删除全部连接线
    // this.plumb.deleteEveryEndpoint()
    // 移除所以事件
    // this.plumb.cleanupListeners()
    // 清除实例
    this.plumb.destroy()
  },
  methods: {
    onLine() {
      // 自定连接点
      this.plumb.connect({ uuids: ['1', '6'] })
    },
    initJsplumb() {
      // 创建实例
      this.plumb = jsPlumb.getInstance()

      this.plumb.ready( () => {
        this.setDot()
      })
    },

    setDot() {
      for (let i = 0; i < this.leftData.length; i++) {
        this.addPoint('left_' + this.leftData[i].id, this.leftData[i].id, 'Right')
      }

      for (let i = 0; i < this.rightData.length; i++) {
        this.addPoint('right_' + this.rightData[i].id, this.rightData[i].id, 'Left')
      }
    },

    addPoint(id, uuid, direction) {
      this.plumb.addEndpoint(id, {
        anchor: direction,
        // 为每个连接桩生成位移 id
        uuid: '' + uuid
      }, {
        endpoint: 'Dot',
        isSource: true,
        isTarget: true,
        // 连线样式 贝塞尔曲线
        connector: ['Bezier'],
        connectorStyle: {
          outlineStroke: '#00B4D4',
          strokeWidth: 1
        },
        connectorHoverStyle: {
          strokeWidth: 2
        },
        // 连接桩样式
        paintStyle: { fill: '#00B4D4', stroke: 'lightgray', strokeWidth: 4, radius: 8 }
      })
    },
    // 创建鼠标拖动排序
    initSortable() {
      const sourceTable = document.getElementById('source-table')
      const targetTable = document.getElementById('target-table')
      const _this = this
      new Sortable(sourceTable, {
        animation: 150,
        // 拖拽时预览图样式
        ghostClass: 'blue-background-class',
        onEnd() {
        // 更新视图
          _this.plumb.repaintEverything()
        }
      })

      new Sortable(targetTable, {
        animation: 150,
        // 拖拽时预览图样式
        ghostClass: 'blue-background-class',
        onEnd() {
        // 更新视图
          _this.plumb.repaintEverything()
        }
      })
    }

  }

}
</script>

<style scoped>
  .jsplumbContainer {
    padding: 20px;
    width: 800px;
    margin: 10px;
    display: flex;
    justify-content: space-between;
    position: relative;
    align-items: center;

  }

  .jsplumbContainer ul {
      padding: 0;
      width: 30%;
      border: 1px solid #ccc;

    }
  .jsplumbContainer ul li {
      list-style: none;
      padding: 20px;
      display: flex;
      justify-content: space-between;


    }
    .jsplumbContainer ul li:not(:last-child) {
      border-bottom: 1px solid #ccc;
    }

    .blue-background-class {
      background: lightblue;
    }

</style>
