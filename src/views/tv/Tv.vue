<template>
  <div class="content">
    <ul>
      <li @click="goDetail(item.id)" v-for="item in dataList" :key="item.id">
        <div class="left">
          <img :src="'https://images.weserv.nl/?url='+item.cover.url" alt />
        </div>
        <div class="right">
          <h3>{{item.title}}</h3>
          <p>{{item.info}}</p>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      dataList: [],
      start: 0,
      isGoOn: true
    };
  },
  created() {
    this.getData();
  },
  mounted() {
    this.listenScroll();
  },
  methods: {
      //跳转至详情页
      goDetail(id){
          this.$router.push("/tvdetail/"+id)
      },
    listenScroll() {
      let htmlDom = document.documentElement; //获取html元素
      window.onscroll = () => {
        let scrollHeight = htmlDom.scrollHeight; //可滚动区域高度
        let scrollTop = htmlDom.scrollTop; //获取滚动距离（滚动条距离顶部的距离）
        let htmlHeight = htmlDom.clientHeight; //获取当前视口高度
        if (htmlHeight + scrollTop > scrollHeight - 10) {
          //如果滚动条距离底部还差10的时候判断为到底了
          this.start += 10;
          this.getData();
        }
      };
    },
    getData() {
      if (this.isGoOn) {
        this.isGoOn = false;
        let RequestUrl = `https://m.douban.com/rexxar/api/v2/subject_collection/tv_domestic/items?os=ios&for_mobile=1&start=${this.start}&count=10`;
        let baseUrl = "https://bird.ioliu.cn/v2?url=";
        axios.get(baseUrl + RequestUrl).then(res => {
          //   this.dataList = res.data.subject_collection_items;
          this.dataList = this.dataList.concat(
            res.data.subject_collection_items
          ); //返回的数据是拼接在之间的数据之后
          if (this.dataList.length < res.data.total) {
            //仅在有可获取的数据时发起请求
            this.isGoOn = true;
          }
        });
      }
    }
  }
};
</script>
<style scoped>
.content {
  padding: 10px;
}
li {
  display: flex;
  border-bottom: 1px solid #ddd;
}
li .left {
  flex-grow: 1;
  width: 0;
}
li .left img {
  width: 100%;
}
li .right {
  font-size: 0.3rem;
  flex-grow: 2;
  width: 0;
  padding: 0 10px;
}
li .right h3 {
  margin-bottom: 10px;
}
</style>