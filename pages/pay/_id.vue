<template>
  <div class="cart py-container">
    <!--主内容-->
    <div class="checkout py-container  pay">
      <div class="checkout-tit" style="width: 1050px; margin: 0 auto; padding: 10px 0;">
        <h4 class="fl tit-txt"><span class="success-info">支付申请提交成功，请您及时付款！订单号：{{ payObj.out_trade_no }}</span>
        </h4>
        <span class="fr"><em class="sui-lead">应付金额：</em><em class="orange money">￥{{ payObj.total_fee/100 }}</em></span>
        <div class="clearfix"/>
      </div>
      <div class="checkout-steps">
        <div class="fl weixin">微信支付</div>
        <div class="fl sao">
          <div class="fl code">
            <!-- <img id="qrious" src="~/assets/img/erweima.png" alt=""> -->
            <qriously :value="payObj.code_url" :size="338"/>
          </div>
          <div style="color: red; text-align:center;">请使用微信扫一扫</div>
        </div>
        <div class="clearfix"/>
        <!-- <p><a href="pay.html" target="_blank"> 其他支付方式</a></p> -->
      </div>
    </div>

  </div>
</template>

<script>
import payApi from '~/api/pay'
import orderApi from '~/api/order'

export default {
  data() {
    return {
      payObj: {},
      timer: null // 定时器
    }
  },

  async asyncData(page) {
    const response = await payApi.createNative(page.route.params.id)
    return {
      payObj: response.data
    }
  },

  // created的时候就查询支付状态，没有必要，因为二维码页面尚未渲染，不可能支付成功
  mounted() {
    // 启动定时器
    this.timer = setInterval(() => {
      this.queryPayStatus()
    }, 3000)
  },

  methods: {
    queryPayStatus() {
      orderApi.queryPayStatus(this.payObj.out_trade_no)
        .then(response => {
          if (response.success) { // 响应R的success是boolean，表示是否执行成功
            this.$message.success(response.message)
            clearInterval(this.timer)
            setTimeout(() => {
              console.log('马上就要跳转了，courseId是' + this.payObj.course_id)
              this.$router.push({ path: '/course/' + this.payObj.course_id })
            }, 3000)
          }
        })
    }
  }
}
</script>
