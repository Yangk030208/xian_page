<template>
  <div class="home_main">
    <Head></Head>
    <div class="home_meanuList">
      <div class="home_meanu">
        <MeanuMain v-on:change_show_img="change">
        </MeanuMain>
      </div>
      <div class="home_img" v-if="show_img">
        <el-carousel height="350px">
          <el-carousel-item v-for="index in 4" :key="index">
            <img class="slide_img"
                 v-if = "key === index-1"
                 v-for = "(img, key) in slide_img_data"
                 v-bind:key = "key"
                 v-bind:alt = "img.desc"
                 v-bind:src = "img.url"/>
          </el-carousel-item>
        </el-carousel>
      </div>
      <div class="home_img slide_meanu" v-if="!show_img"
           @mouseenter = "enter_meanu()"
           @mouseleave = "leave_meanu()">
        <div class="big_type_meanu"  v-bind = "slide_data" v-for = "(item, index) in slide_data"
             v-bind:key = "index">
          <span class = "big_type_meanu_title"><i class = "el-icon-search"></i>&nbsp;&nbsp;{{index}}</span>
            <span class = "big_type_meanu_sign_item"
                  v-for = "(sign_item, sign_index) in slide_data[index]"
                  v-bind:key = "sign_index">
              {{sign_item}}
            </span>
        </div>
      </div>
    </div>
    <div class = "home_product_list">
      <div class = "home_product_type_list">
        <ProductList v-for = "(item,index)  in show_unit"
                     v-bind:key = "index"
                     v-bind:item = "item">
        </ProductList>
      </div>
    </div>
    <Foot></Foot>
  </div>
</template>

<script>
import Head from '@/components/Head.vue'
import Foot from '@/components/Foot.vue'
import ProductList from '@/components/ProductList.vue'
import MeanuMain from '@/components/MeanuMain.vue'
import conf from '@/assets/conf/conf.js'
export default {
  name: 'home',
  components: {
    Head, Foot, ProductList, MeanuMain
  },
  methods: {
    enter_meanu: function () {
      this.show_img = false
    },
    leave_meanu: function () {
      this.show_img = true
    },
    change: function (data) {
      let _self = this
      _self.show_img = !(_self.show_img)
      if (data && data.item) {
        _self.slide_data = data.item
      }
      // console.log(_self.slide_data)
    }
  },
  mounted: function () {
    // 获取首页的轮播图
    let _self = this
    _self.$tool.http_tool(null, null, _self.url + '/page/getIndexSlide', function (data) {
      _self.slide_img_data = data
      for (let sign_img of _self.slide_img_data) {
        sign_img.url = _self.slide_img_host + sign_img.url
      }
      // console.log(_self.slide_img_data)
    })
  },
  data: function () {
    return {
      show_img: true,
      slide_data: '',
      slide_img_data: [],
      url: conf.host,
      img_url: conf.img_host,
      slide_img_host: conf.slide_img_host,
      show_unit: {
        'food': {
          name: '食物',
          url: '/product/findAll',
          currentPage: 0,
          pageShowNumber: 4
        },
        'clothes': {
          name: '衣服',
          url: '/product/findAll',
          currentPage: 0,
          pageShowNumber: 4
        },
        'electron': {
          name: '电器',
          url: '/product/findAll',
          currentPage: 0,
          pageShowNumber: 4
        },
        'hot_sell': {
          name: '热销',
          url: '/product/findAll',
          currentPage: 0,
          pageShowNumber: 8
        },
        'cheap_sell': {
          name: '特价',
          url: '/product/findAll',
          currentPage: 0,
          pageShowNumber: 8
        }
      }
    }
  }
}
</script>
<style scoped>
  .home_main{
    height: auto;
    width: 100%;
    margin: 2em auto;
  }
  .home_meanuList{
    margin: auto;
    width: 69%;
  }
  .home_meanu{
    width: 19em;
    display: table-cell;
    background-color: ghostwhite;
  }
  .home_img{
    width: 80em;
    display: table-cell;
    /*border: 1px solid firebrick;*/
    vertical-align: top;
  }
  .big_type_meanu{
    /*border: 2px solid green;*/
    margin: 1em 0em 1em 0.5em;
  }
  .big_type_meanu_title{
    font-size: 19px;
    color: grey;
    display: block;
  }
  .big_type_meanu_sign_item{
    margin-left: 2em;
    font-size: 14px;
  }
  .slide_meanu {
    border: 2px solid darkgray;
  }
</style>
