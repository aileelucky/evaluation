<template>
  <div class="wrapper" :style="'background-image: url(' + backgroundImageSrc + ')'">
      <div class="login-qrcode-wrapper">
        <span class="qrcode-title">APP扫码登录</span>
        <div class="login-qrcode"
             v-loading="loginQrcodeStatus === 'CONFIRM_LOGIN'"
             element-loading-background="rgba(255, 255, 255, 0.95)"
             >
          <div id="login_qrcode_img" class="login_img"></div>
          <div v-if="loginQrcodeStatus==='QR_EXPIRED' || loginQrcodeStatus==='SCAN_SUCCESS'" class="login_img_cover"></div>
          <img v-if="loginQrcodeStatus==='QR_EXPIRED'" src="./images/scan_refresh.png" class="icon-refresh" @click="scanRefresh">
        </div>

        <span v-if="loginQrcodeStatus==='CONFIRM_LOGIN'" class="qrcode-footer-logining">正在登陆…</span>
        <span v-if="loginQrcodeStatus==='CREATE_SUCCESS' || loginQrcodeStatus==='QR_EXPIRED'" class="qrcode-footer">请使用回来啦APP扫描二维码登录</span>
      </div>

      
  </div>
</template>

<script>
import QRCode from 'qrcodejs2'
export default {
  name: 'Evaluation',
  props: {
    msg: String
  },
  data() {
    return {
      qrCode: '',
      loginQrcodeStatus: 'QR_EXPIRED', // CREATE_SUCCESS, QR_EXPIRED, CANCEL_LOGIN ,SCAN_SUCCESS ,CONFIRM_LOGIN
      backgroundImageSrc: 'http://devapi.4001113900.com:7080//img/201908/shopgoods/shopgoods156532081458922.png',
      qrCodeTimer: undefined,
      qrCodeTime: 3
    }
  },
  mounted () {
    this.dealWithQRCodeShow()
  },
  methods:{
    destroyed () {
      clearInterval(this.sendCodeTimer)
    },
    // 创建二维码
    dealWithQRCodeShow () {
      // const { data } = await this.$axios({
      //   method: 'get',
      //   url: `${API_CONFIG.butlerBaseURL}manager/qrLogin/code`
      // })
      // if (!data) {
      //   return
      // }
      // this.qrCode = data.qrCode
      // this.qrCodeTime = data.validityPeriod
      // this.createSocket()
      let dom = document.getElementById('login_qrcode_img')
      if (dom) {
        while (dom.firstChild) {
          dom.removeChild(dom.firstChild)
        }
        let encryCode = "data.qrCode"
        if (encryCode && encryCode.length) {
          let id = `login_qrcode_img`
          this.$nextTick(() => {
                new QRCode(id, { // eslint-disable-line
              width: 324,
              height: 324,
              text: encryCode,
              correctLevel: 3
            })
          }, 20)
          this.loginQrcodeStatus = 'CREATE_SUCCESS'
          this.startQrCodeTimer()
        }
      }
    },
     startQrCodeTimer () {
      let _this = this
      let time = _this.qrCodeTime || 0
      this.qrCodeTimer = setInterval(() => {
        if (time <= 0) {
          // if (_this.loginQrcodeStatus !== 'CONFIRM_LOGIN') {
          //   _this.loginQrcodeStatus = 'QR_EXPIRED'
          // }
         _this.loginQrcodeStatus = 'CONFIRM_LOGIN'
          clearInterval(_this.qrCodeTimer)
          this.webSocket && this.webSocket.close()
        } else {
          time--
        }
      }, 1000)
    },
    scanRefresh () {
      this.dealWithQRCodeShow()
    },
  }
}
</script>

<style scoped lang="scss">
.wrapper{
  height: 100%;
  background-size: 100% 100%;
  background-repeat: no-repeat;
  padding-top: 200px;
}
.login-qrcode-wrapper{
  display: flex;
  flex-direction: column;
  align-items: center;
  
}
.login-qrcode{
      width: 360px;
      height: 360px;
      margin:26px 0 16px 0;
      background:rgba(255,255,255,1);
      border-radius:4px;
      display:flex;
      font-size: 40px;
      color:#A2A0A0;
      justify-content:center;
      align-items:center;
      .login_img_cover{
        position:absolute;
        width:360px;
        height:360px;
        background:rgba(255,255,255,1);
        border-radius:4px;
        opacity:0.95;
        display:flex;
        justify-content:center;
        align-items:center;
      }
      .icon-refresh{
        width:76px;
        height:76px;
        position:absolute;
        }
    }
    .qrcode-title{
      font-size:24px;
      font-family:PingFangSC-Semibold,PingFang SC;
      font-weight:600;
      color:rgba(255,255,255,1);
    }
    .qrcode-footer-logining{
      font-size:24px;
      font-family:PingFangSC-Semibold,PingFang SC;
      font-weight:600;
      color:rgba(255,255,255,1);
    }
    .qrcode-footer{
      font-size:20px;
      font-family:PingFangSC-Regular,PingFang SC;
      font-weight:400;
      color:rgba(255,255,255,1);
    }
</style>
