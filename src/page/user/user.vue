<template>
  <div class="user-box">

    <transition name="parent-slide" mode="out-in">
      <div class="user-wrapper" v-show="$root.showUserChilrenRouter">

        <vheader message='个人中心' back='true'>
          <span class="fa fa-angle-left" slot="angle" @click="$router.go(-1)"></span>
        </vheader>

        <div v-show="userInfomation">
          <div class="user-data" @click="addChildren()">
            <div class="user-logo">
              <img src="https://fuss10.elemecdn.com/e/3c/1b9262cfb2c65fa3054d54cfaec54jpeg.jpeg">
            </div>
            <div class="user-name">
              <p>{{userInfo.uName}}</p>
              <p><span>loaf</span><span>{{userInfo.phone}}</span></p>
            </div>
            <span class="fa fa-angle-right"></span>
          </div>

          <ul class="user-info">

            <router-link to="/balance" tag="li">
              <p><strong class="font-weight" style="color:#FF9900">{{userInfo.balance}}</strong>元</p>
              <p>我的余额</p>
            </router-link>

            <router-link to="/favorable" tag="li">
              <p><strong class="font-weight" style="color:#FF5F3E">{{userInfo.count}}</strong>个</p>
              <p>我的优惠</p>
            </router-link>

            <router-link to="/points" tag="li">
              <p><strong class="font-weight" style="color:#6AC20B">{{userInfo.pointNumber}}</strong>分</p>
              <p>我的积分</p>
            </router-link>
          </ul>
          <ul class="about-elm">
            <router-link tag="li" to="/order">
              <span><img :src="order_icon"></span><span>我的订单</span><span
              class="fa fa-angle-right"></span>
            </router-link>
            <router-link to="/integral" tag="li">
              <span><img :src="integral_icon"></span><span>积分商城</span><span
              class="fa fa-angle-right"></span>
            </router-link>
            <router-link to="/vip" tag="li">
              <span><img :src="vip_icon"></span><span>饿了么会员卡</span><span
              class="fa fa-angle-right"></span>
            </router-link>
            <router-link to="/service" tag="li">
              <span><img :src="service_icon" ></span><span>服务中心</span><span
              class="fa fa-angle-right"></span>
            </router-link>
            <router-link tag="li" to="/download">
              <span><img :src="download_icon" ></span><span>下载我了么APP</span><span
              class="fa fa-angle-right"></span>
            </router-link>
          </ul>
        </div>

        <vfoot></vfoot>
      </div>
    </transition>

    <transition name="children-slide" mode="out-in">
      <router-view></router-view>
    </transition>

  </div>
</template>
<script>
  import vheader from '../../components/header/header.vue'
  import vfoot from '../../components/footer/footNav.vue'
  export default {
    data () {
      return {
        order_icon: require('../../../static/icon/01.png'),
        integral_icon: require('../../../static/icon/02.png'),
        vip_icon: require('../../../static/icon/03.png'),
        service_icon: require('../../../static/icon/04.png'),
        download_icon: require('../../../static/icon/05.png')
      }
    },
    components: {
      vfoot,
      vheader
    },
    computed: {
      userInfo () {
        let obj = this.$store.getters.userInfo
        if (JSON.stringify(obj) === '{}') { // 如果用户还没有登陆，就显示登陆注册信息
          obj.uName = '登陆/注册'
          obj.phone = '登陆后享有更多特权'
          obj.count = 0
          obj.balance = '00.0'
          obj.pointNumber = 0
        }
        return obj
      },
      userInfomation () {
        return this.$store.getters.userInfomation
      }
    },
    created () {
      window.localStorage.getItem('state') === 'false' ? this.$root.showUserChilrenRouter = false : window.localStorage.getItem('state') === null ? this.$root.showUserChilrenRouter = true : this.$root.showUserChilrenRouter = true
    },
    methods: {
      addChildren () {
        this.$root.showUserChilrenRouter = false
        window.localStorage.setItem('state', false)
        this.$router.push({path: '/user/info'})
      }
    }
  }
</script>
<style scoped lang="less">
  @import '../../common/comment.less';
  @import '../../common/animate';
  .user-box {
    width:100%;
    overflow: hidden;
    .user-wrapper {
      .pt(.8rem);
      .pb(.8rem);
      background: @f7;
      min-height: 100vh;
      .user-data {
        .flex;
        height:1.6rem;
        .rel;
        color: @default;
        background: @background;
        padding: .3rem;
        .user-logo {
          width: 25%;
          .tc;
          .rel;
          img {
            width: 60%;
            .b-r(50%);
            .abs;
            top:50%;
            left:50%;
            .trsl();
          }
        }
        .user-name {
          width: 70%;
          > p {
            font-size: .28rem;
            .fw;
            height: 50%;
            .pt(.14rem);
          }
        }
        .fa-angle-right {
          flex: 1;
          font-size: .4rem;
          .vc;
          right: .3rem;
        }
      }
      .user-info {
        .flex;
        background: @default;
        .mb(.2rem);
        li:nth-child(2) {
          border-left: 1px solid @ddd;
          border-right: 1px solid @ddd;
        }
        li {
          flex: 1;
          .tc;
          font-size: .24rem;
          .pt(.3rem);
          .pb(.3rem);
          > p {
            .font-weight {
              font-size: .5rem;
            }
          }
          > p:last-child {
            .mt(0.1rem);
          }
        }
      }
      .about-elm {
        background: @default;
        color: @4D;
        > li + li {
          border-top: 1px solid @ddd;
        }
        > li {
          .flex;
          font-size: .28rem;
          padding: .2rem .4rem;
          > span:first-child {
            width: 6%;
            .rel;
            img {
              width: 100%;
              .abs;
              top: 50%;
              left: 50%;
              .trsl();
            }
          }
          > span:nth-child(2) {
            width: 85%;
            .pl(.2rem);
          }
          > span:last-child {
            width: 5%;
            font-size: .4rem;
          }
        }
      }
    }
    .animateChildren;
    .animateParent;
  }
</style>
