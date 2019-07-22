<template>
  <f7-page name="single-problem">
    <f7-navbar title="帮助与反馈" back-link="返回"></f7-navbar>
    <f7-block-title>{{problem}}</f7-block-title>
    <f7-block strong>
      <p v-html="content"></p>
    </f7-block>
    <f7-block-title>未解决问题？点击此处
      <i class="unsettled-icon" @click="$refs.actions.open()">
        <svg t="1563522320639" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="3808" width="14" height="14"><path d="M182.880895 329.119349q0 14.84694-10.82437 25.744447t-25.744447 10.82437q-15.432041 0-25.96386-10.82437t-10.604957-25.744447q0-15.432041 10.604957-25.96386t25.96386-10.604957q14.84694 0 25.744447 10.604957t10.82437 25.96386zM274.302936 621.669881l0-365.688165q0-14.84694-10.82437-25.744447t-25.744447-10.82437l-164.559674 0q-14.84694 0-25.744447 10.82437t-10.82437 25.744447l0 365.688165q0 14.84694 10.82437 25.744447t25.744447 10.82437l164.559674 0q14.84694 0 25.744447-10.82437t10.82437-25.744447zM919.37686 536.537676q31.449182 34.886651 31.449182 85.132205-0.585101 44.540819-32.838797 77.160203t-76.867652 32.546247l-158.269838 0q2.267267 7.972002 4.534533 13.676737t6.289836 12.579673 5.704735 10.312406q10.312406 21.136776 15.432041 32.546247t10.82437 33.423898 5.704735 43.736305q0 13.676737-0.292551 22.306978t-2.852368 25.744447-6.874938 28.596815-13.676737 25.744447-22.818942 23.111492-34.30155 14.84694-47.173773 5.997286q-14.84694 0-25.744447-10.82437-11.409471-11.409471-19.45461-28.596815t-11.11692-29.693879-7.167488-34.886651q-5.119634-23.989144-7.679451-34.5941t-10.019856-27.719163-17.699307-27.426612q-18.869509-18.869509-57.705592-68.529962-28.011713-36.568817-57.705592-69.115063t-43.443754-33.716449q-14.261838-1.170202-24.574245-11.702021t-10.312406-24.866795l0-366.273266q0-14.84694 10.82437-25.451896t25.744447-11.11692q19.966574-0.585101 90.251839-25.159346 44.028855-14.84694 68.822513-22.599529t69.407614-16.602243 82.279837-8.849654l73.722734 0q75.990001 1.170202 112.558817 44.540819 33.131348 39.421184 28.011713 103.416613 22.306978 21.136776 30.864081 53.683023 9.727305 34.886651 0 66.847797 26.25641 34.886651 24.574245 78.257267 0 18.284408-8.557103 43.443754z" p-id="3809" fill="#007aff"></path></svg>
        未解决
      </i>
    </f7-block-title>
    <f7-list>
      <f7-list-item title="掌上理工大用户群" link="mqqapi://card/show_pslcard?src_type=internal&version=1&uin=420082599&card_type=group&source=qrcode"></f7-list-item>
      <f7-list-item title="小纬QQ" link="mqqwpa://im/chat?chat_type=wpa&uin=2577438164&version=1&src_type=web&web_src=iwut.wutnews.net"></f7-list-item>
      <f7-list-item title="智慧理工大相关服务反馈群" link="mqqapi://card/show_pslcard?src_type=internal&version=1&uin=700852640&card_type=group&source=qrcode"></f7-list-item>
    </f7-list>
    <f7-actions ref="actions">
      <f7-actions-group>
        <f7-actions-label>您可以选择</f7-actions-label>
        <f7-actions-button @click="$f7router.navigate('/edit-feedback/')">反馈</f7-actions-button>
        <f7-actions-button color="red">取消</f7-actions-button>
      </f7-actions-group>
    </f7-actions>
  </f7-page>
</template>

<script>
import { api_domain } from '../js/config.js';

export default {
  name: 'single-problem',
  data () {
    return {
      problem: '',
      content: ''
    }
  },
  mounted () {
    this.$f7ready((f7) => {
      f7.preloader.show();
      f7.request({
        url: `${api_domain}/look_problem`,
        method: 'POST',
        dataType: 'json',
        data: {
          'problem_id': this.$f7route.params.problemId
        },
        success: res => {
          f7.preloader.hide();
          if (res.code == 0) {
            this.problem = res.data.problem;
            this.content = res.data.content;
          } else {
            f7.dialog.alert('获取问题详情失败!');
          }
        },
        error: err => {
          f7.preloader.hide();
          f7.dialog.alert('获取问题详情失败!');
        }
      })
    });
  }
}

</script>

<style>
.unsettled-icon {
  padding: 2px 4px;
  display: inline-block;
  border: 1px solid #d9d9d9;
  border-radius: 4px;
  font-style: normal;
}
</style>