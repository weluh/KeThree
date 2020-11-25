<template>
  <div class="menu">
    <div class="menu-nav">
      <div class="menu-search">
        <van-search placeholder="输入商品名称" @focus="searchFocus" />
      </div>
    </div>

    <van-tree-select height="84vh" :items="typeData" :main-active-index.sync="menuIndex" @click-nav = "getData">
      <template #content>
       <div class="menu-product">
      <div class="clearfix m-pro-item" v-for="(item, index) in productData" :key="index" @click="goDetail(item.pid)">
        <div class="fl pro-img">
          <img
            class="auto-img"
            :src="item.smallImg"
            alt=""
          />
        </div>
        <div class="fl text">
          <div class="pro-text">
            <div class="pro-name">{{item.name}}</div>
            <div class="pro-enname">{{item.enname}}</div>
          </div>
        </div>
        <div class="fl price">￥{{item.price}}</div>
      </div>
    </div>
      </template>
    </van-tree-select>
  </div>
</template>

<script>
import "../assets/less/menu.less";
export default {
  name: "Menu",
  data() {
    return {
      menuIndex: 0,

      //商品数据
      productData: [],
      typeData:[{text:'推荐',key:'isHot'}]
    };
  },

  created() {
    //获取商品类型
    this.getTypes();
    this.getData(0);
    
  },

  methods: {
    
    getData(index){
      let params = {};
      if(index==0){
        params={
            appkey:this.appkey,
            key:this.typeData[index].key,
            value:1
          }
      }else{
        params={
            appkey:this.appkey,
            key:'type',
            value:this.typeData[index].key
          }
      }
      this.axios({
        method: "GET",
        url: "/typeProducts",
        params,
      })
        .then((result) => {
          this.$toast.clear();
          if (result.data.code == 500) {
            this.productData = result.data.result;
          }

        })
        .catch((err) => {
          this.$toast.clear();
          
        });
    },

    //获取商品类型
    getTypes() {

      this.$toast.loading({
        message: "加载中...",
        forbidClick: true,
        duration: 0,
      });

      this.axios({
        method: "GET",
        url: "/type",
        params: {
          appkey: this.appkey
        },
      })
        .then((result) => {
          this.$toast.clear();

          if (result.data.code == 400) {
            let data = result.data.result;
              
            data.forEach(v => {
              let obj ={};
              obj.text = v.typeDesc;
              obj.key = v.type;
              this.typeData.push(obj);
            })

          }

        })
        .catch((err) => {
          this.$toast.clear();
          
        });
    },

    //查看商品详情
    goDetail(pid){
      this.$router.push({name: 'Detail', params: {pid}});
    },

    //搜索栏获取焦点
     searchFocus() {
       this.$router.push({name: 'Search'});
     } 
  },
};
</script>