<template>
  <div>
    <Head></Head>
    <div class="user_main">
      <div class="user_main_table user_main_left">
        <el-row class="tac">
          <el-col>
            <h5>用户中心</h5>
            <el-menu :default-active='tag' class="el-menu-vertical-demo"
                     @select="handleSelect">
              <el-menu-item index="datum">
                <i class="el-icon-menu"></i>
                <span slot="title">个人资料</span>
              </el-menu-item>
              <el-menu-item index="order">
                <i class="el-icon-tickets"></i>
                <span slot="title">订单列表</span>
              </el-menu-item>
              <el-menu-item index="car">
                <i class="el-icon-goods"></i>
                <span slot="title">购物车</span>
              </el-menu-item>
              <el-menu-item index="msg">
                <i class="el-icon-message"></i>
                <span slot="title">消息中心</span>
              </el-menu-item>
            </el-menu>
          </el-col>
        </el-row>
      </div>
      <div class="user_main_table user_main_right">
        <div v-if=" tag == 'datum'">
          <Datum></Datum>
        </div>
        <div v-else-if="tag == 'msg'">
          <Msg></Msg>
        </div>
        <div v-else-if="tag == 'order'">
          <Order></Order>
        </div>
        <div v-else-if="tag == 'car'">
          <Car></Car>
        </div>
      </div>
    </div>
    <Foot></Foot>
  </div>
</template>

<script>
import Head from '@/components/Head.vue'
import Foot from '@/components/Foot.vue'
import Datum from '@/components/Datum.vue'
import Msg from '@/components/Msg.vue'
import Order from '@/components/Order.vue'
import Car from '@/components/Car.vue'
import conf from '../assets/conf/conf.js'
export default {
  name: 'UserCenter',
  props: ['tag'],
  components: {
    Head, Foot, Msg, Order, Datum, Car
  },
  data: function () {
    return {
      current_user: null,
      token_key: conf.token_key
    }
  },
  methods: {
    handleSelect (key, keyPath) {
      this.$router.push('/usercenter/' + key)
    }
  },
  mounted: function () {
    let _self = this
    _self.current_user = _self.$tool.check_user(_self)
    if (_self.current_user === null) {
      return _self.$router.push('/')
    }
  }
}
</script>

<style scoped>
.user_main{
  width: 69%;
  margin: 5em auto;
}
.user_main_table{
  display: table-cell;
}
.user_main_left{
  width: 15em;
}
.user_main_right{
  width: 85em;
}
</style>
