<template>
  <div class="resetSecret_box">
    <vhead message="重置密码">
      <span class="fa fa-angle-left" slot="angle" @click="$router.go(-1)"></span>
    </vhead>

    <div class="update-wrapper">
      <div class="input_wrapper">
        <input type="text" placeholder="账号" maxlength="16" v-model="ZH" @input="inputZH">
      </div>
      <div class="input_wrapper">
        <input type="password" placeholder="旧密码" maxlength="16" v-model="OMA" @input="oldMiMa">
      </div>
      <div class="input_wrapper">
        <input type="password" placeholder="新密码" maxlength="16" v-model="NMA" @click=newMiMa>
      </div>
      <div class="input_wrapper">
        <input type="password" placeholder="请确认新密码" maxlength="16" v-model="CNMM" @click="confirmNewMiMa">
      </div>
    </div>

    <p class="confirm_update" @click="confirmUpdate">确认修改</p>

    <alertBounced v-if="showAlert" @closeBounced="showAlert = false" :alertText="alertText">
    </alertBounced>
  </div>
</template>
<script>
  import vhead from '../../components/header/header.vue'
  import alertBounced from '../../components/bounced/bounced'
  export default {
    data () {
      return {
        alertText: '', // 弹出框的提示消息,
        showAlert: false, // 是否弹出提示框
        ZH: '', // 账号
        OMA: '',  // 旧密码
        NMA: '',  // 新密码
        CNMM: '',  // 确认密码
        ctrlAlert: true
      }
    },
    components: {
      vhead,
      alertBounced
    },
    methods: {
      inputZH () {
      },
      oldMiMa () {
        console.log(typeof this.OMA)
      },
      newMiMa () {
      },
      confirmNewMiMa () {
      },
      confirmUpdate () {
        if (this.ZH === '') {
          this.alertText = '请输入您的账号'
        } else if (this.OMA === '') {
          this.alertText = '请输入您的密码'
        } else if (this.NMA === '') {
          this.alertText = '请输入新密码'
        } else if (this.CNMM === '') {
          this.alertText = '请确认新密码'
        } else if (this.ZH !== '' && this.OMA !== '' && this.NMA !== '' && this.CNMM !== '') {
          let userInfo = JSON.parse(window.localStorage.getItem('userInfo'))
          if (this.ZH !== userInfo.userName) {
            this.alertText = '请输入正确的账号'
            this.ZH = ''
          } else if (this.OMA !== userInfo.pwd) {
            this.alertText = '请输入正确的密码'
            this.OMA = ''
          } else if (this.NMA !== this.CNMM) {
            this.alertText = '请重新输入密码'
            this.NMA = ''
            this.CNMM = ''
          } else {
            let newUserInfo = {}
            newUserInfo.userName = userInfo.userName
            newUserInfo.pwd = this.NMA
            window.localStorage.setItem('userInfo', JSON.stringify(newUserInfo))
            this.$router.push({path: '/confirmOrder'})
          }
        }
        this.showAlert = true
      }
    }
  }
</script>
<style lang="less" scoped>
  @import '../../common/comment.less';
  .resetSecret_box{
    min-height:100vh;
    background:@f7;
    .pt(1.1rem);
    font-size:14px;
    .update-wrapper{
      background:@default;
      .mb(.4rem);
      .input_wrapper{
        padding:.3rem .4rem;
        input{
          border:none;
          outline: none;
        }
      }
      .input_wrapper+.input_wrapper{
        border-top:1px solid @ddd;
      }
    }
    .confirm_update{
      width:95%;
      .b-r(5px);
      font-size:14px;
      background:@btn;
      margin:0 auto;
      .tc;
      line-height:.8rem;
      color:@default;
    }
  }
</style>
