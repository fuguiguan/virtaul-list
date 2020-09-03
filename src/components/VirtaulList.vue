<template>
  <div class="virtaul-lst">
    <div class="all-list">
      <div v-for="item in showList" :key="item" class="list-wrap">
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
      dataSource:[],//数据源，长列表
      showList: [] //展示数组
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
    this.showList = this.dataSource.slice(0,40)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.virtaul-lst {
  width: 300px;
  height: 600px;
  margin: 100px auto;
  background-color: rgba(220, 220, 220, .2);

  overflow: auto;
}
.all-list {
  height: 4000px; /**列表总高度 */
}
.item {
  text-align: center;
  height: 20px;
  margin: 10px;
  background-color: rgba(226, 171, 171, 0.2);
}
</style>
