<template>
  <div class="main">
    <div class="title">
      <a class="active" href="/login">登录</a>
      <span>·</span>
      <a href="/register">注册</a>
    </div>

    <div class="sign-up-container">
      <form action="register">
        <div class="input-prepend restyle">
          <input
            v-model="user.phoneNum"
            type="text"
            placeholder="手机号">
          <i class="iconfont icon-phone"/>
        </div>
        <div class="input-prepend">
          <input
            v-model="user.password"
            type="password"
            placeholder="密码">
          <i class="iconfont icon-password"/>
        </div>
        <div class="btn">
          <input
            type="button"
            class="sign-in-button"
            value="登录"
            @click="submitLogin()">
        </div>
      </form>
      <!-- 更多登录方式 -->
      <div class="more-sign">
        <h6>社交帐号登录</h6>
        <ul>
          <li><a id="weixin" class="weixin" href="http://localhost:8160/api/ucenter/wx/login"><i class="iconfont icon-weixin"/></a></li>
          <li><a id="qq" class="qq" target="_blank" href="#"><i class="iconfont icon-qq"/></a></li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import '~/assets/css/sign.css'
import '~/assets/css/iconfont.css'

import cookie from 'js-cookie'
import loginApi from '~/api/login'

export default {
  layout: 'sign',

  data() {
    return {
      user: {
        phoneNum: '',
        password: ''
      }
    }
  },

  methods: {
    // 登录
    submitLogin() {
      loginApi.submitLogin(this.user).then(response => {
        cookie.set('guli_token', response.data.token, {
          domin: 'localhost',
          expires: 1 // 1天：如果是数值则单位为天，也可以是Date类型，表示有效期至Date指定时间
        })
        window.location.href = '/' // 这是强行修改浏览器地址栏的url，"/"是首页
      })
    }
  }
}
</script>
