<template>
  <f7-page name="single-catagory">
    <f7-navbar title="帮助与反馈" back-link="返回"></f7-navbar>
    <f7-block-title>分类</f7-block-title>
    <f7-list>
      <f7-list-item v-for="(item, index) in problems" :key="item.id" :title="item.problem" :link="'/single-problem/'+item.id+'/'"></f7-list-item>
    </f7-list>
    <f7-list>
      <f7-list-item title="没有对应问题？请点击此处进行反馈" link="/edit-feedback/"></f7-list-item>
    </f7-list>
  </f7-page>
</template>

<script>
import { api_domain } from '../js/config.js';

export default {
  name: 'single-catagory',
  data () {
    return {
      problems: []
    }
  },
  mounted () {
    this.$f7ready((f7) => {
      f7.preloader.show();
      f7.request({
        url: `${api_domain}/look_type`,
        method: 'POST',
        dataType: 'json',
        data: {
          'problem_type': this.$f7route.params.catagoryId
        },
        success: res => {
          f7.preloader.hide();
          if (res.code == 0) {
            this.problems = res.data;
          } else {
            f7.dialog.alert('获取问题失败!');
          }
        },
        error: err => {
          f7.preloader.hide();
          f7.dialog.alert('获取问题失败!');
        }
      })
    });
  }
}

</script>