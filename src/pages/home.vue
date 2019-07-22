<template>
  <f7-page name="home">
    <!-- Top Navbar -->
    <f7-navbar :sliding="false">
      <f7-nav-left>
        <f7-link icon-ios="f7:chevron_left" icon-md="material:navigate_before"></f7-link>
      </f7-nav-left>
      <f7-nav-title>帮助与反馈</f7-nav-title>
    </f7-navbar>
    <f7-block-title>常见问题</f7-block-title>
    <f7-list>
      <f7-list-item v-for="(item, index) in problems" :key="item.id" :title="item.problem" :link="'/single-problem/'+item.id+'/'"></f7-list-item>
    </f7-list>
    <f7-list>
      <f7-list-item title="未找到对应问题？这里是全部问题" link="/all-problems/"></f7-list-item>
    </f7-list>
  </f7-page>
</template>

<script>
import { api_domain } from '../js/config.js';

export default {
  name: 'home',
  data () {
    return {
      problems: []
    }
  },
  mounted () {
    this.$f7ready((f7) => {
      f7.preloader.show();
      f7.request({
        url: `${api_domain}/index`,
        method: 'GET',
        dataType: 'json',
        success: res => {
          f7.preloader.hide();
          if (res.code == 0) {
            this.problems = res.data;
          } else {
            f7.dialog.alert('获取常见问题失败!');
          }
        },
        error: err => {
          f7.preloader.hide();
          f7.dialog.alert('获取常见问题失败!');
        }
      })
    });
  },
}

</script>