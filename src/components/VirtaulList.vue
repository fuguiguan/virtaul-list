<template>
  <div class="virtaul-list" @scroll="handleScroll">
    <div class="all-list">
       <!-- :style ="{ paddingTop: `${startOffset}px`, paddingBottom: `${endOffset}px` }" -->
      <div v-for="item in visibleData" :key="item" class="list-wrap">
        <div class="item">{{item}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'virtaul-list',
  props: {
    msg: String
  },
  data() {
    return {
      startIndex: 0,
      endIndex: 20,
      // startOffset:0,//列表的上偏移
      // endOffset: 0,// 列表的下偏移
      visiableCount: 20, //每次渲染的个数
      bufferSize: 10, //缓冲的个数
      itemHeight: 40, //每个item的高度
      cache: [],//缓存以渲染元素
      anchorItem: {
        index: 0, // 锚点元素的索引值
        top: 0, // 锚点元素的顶部距离第一个元素的顶部的偏移量(即 startOffset)
        bottom: 0 // 锚点元素的底部距离第一个元素的顶部的偏移量
      },//锚点元素，即临界元素
      dataSource:[],//数据源，长列表
      visibleData: [], //展示数组
      observer: null, //监听dom的变动
    }
  },
  created() {
    // setTimeout(() => {
        for (let i = 1; i < 1000; i++) {
        this.dataSource.push(i);
      }
    // },1000)
  },
  mounted() {
    // this.visibleData = this.dataSource.slice(this.startIndex, this.endIndex)
    // let container = document.querySelector(".virtaul-list");
    let all = document.querySelector(".all-list");
    // container.addEventListener("scroll",this.updateIndex);
    this.initData();
    this.updateVisibleData();
    this.cachePosition(all, this.startIndex)
  },
  methods: {
    getElementTop(element) {
      var actualTop = element.offsetTop; //这是获取元素距父元素顶部的距离
      var current = element.offsetParent; //这是获取父元素
      while (current !== null) {
        //当它上面有元素时就继续执行
        actualTop += current.offsetTop; //这是获取父元素距它的父元素顶部的距离累加起来
        current = current.offsetParent; //继续找父元素
      }
      return actualTop;
    },
    initData() {
      // const listLen = 400;
      // this.visiableCount = Math.ceil( listLen / this.itemHeight ) + this.bufferSize;
      this.endIndex = this.startIndex + this.visiableCount;
    },
    updateVisibleData() {
      this.visibleData = this.dataSource.slice(this.startIndex, this.endIndex);
      console.log(`data: ${this.visibleData}`);
      // this.endOffset = (this.dataSource.length - this.endIndex) * this.itemHeight
    },
    cachePosition(node, index) {
      const rect = node.getBoundingClientRect();
      const top = rect.top + this.getElementTop(document.querySelector(".virtaul-list"));
      this.cache.push({
        index,
        top,
        bottom: top + this.itemHeight
      });
    },
    updateIndex(scrollTop = 0) {
      let all = document.querySelector(".all-list");
      console.log(`scrollTop: ${this.getElementTop(all)}`);
      scrollTop = scrollTop || 0
      //用户正常滚动下，根据 scrollTop 找到新的锚点元素位置
      const anchorItem = this.cache.find(item => item.bottom >= scrollTop)

      this.anchorItem = {
        ...anchorItem
      }

      this.startIndex = this.anchorItem.index
      this.endIndex = this.startIndex + this.visibleCount
    },
    handleScroll() {
      const scrollTop = document.querySelector(".virtaul-list").scrollTop ;
      // const scrollTop = document.documentElement.scrollTop
      // if(scrollTop > this.anchorItem.bottom ) {
        console.log(`scrolling-- scrollTop: ${scrollTop}`)
        // this.updateIndex(scrollTop);
        // this.cachePosition(document.querySelector(".all-list"), this.startIndex);
        // this.updateVisibleData();
      // }
    }
  },
  computed: {
    startOffset() {
      return this.anchorItem.top;
    },
    endOffset() {
      return (this.dataSource.length - this.startIndex - this.bufferSize) * this.itemHeight
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.virtaul-list {
  /* position: relative; */
  width: 300px;
  height: 400px;
  margin: 100px auto;
  background-color: rgba(220, 220, 220, .2);

  overflow: auto;
}
.all-list {
  height: 4000px; /**列表总高度 */
}
.item {
  text-align: center;
  height: 30px;
  margin-bottom: 10px;
  background-color: rgba(226, 171, 171, 0.2);
}
</style>
