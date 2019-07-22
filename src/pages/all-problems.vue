<template>
  <f7-page name="all-problems">
    <f7-navbar title="帮助与反馈" back-link="返回"></f7-navbar>
    <f7-block-title>全部问题分类</f7-block-title>
    <f7-list>
      <f7-list-item v-for="(item, index) in catagories" :key="item.id" :title="item.content" :link="'/single-catagory/'+item.id+'/'"></f7-list-item>
    </f7-list>
    <f7-list>
      <f7-list-item title="没有对应类型？请点击此处进行反馈" link="/edit-feedback/"></f7-list-item>
    </f7-list>
  </f7-page>
</template>

<script>
import { api_domain } from '../js/config.js';

export default {
  name: 'all-problem',
  data () {
    return {
      catagories: []
    }
  },
  mounted () {
    this.$f7ready((f7) => {
      f7.preloader.show();
      f7.request({
        url: `${api_domain}/get_all_type`,
        method: 'GET',
        dataType: 'json',
        success: res => {
          f7.preloader.hide();
          if (res.code == 0) {
            this.catagories = res.data.data;
          } else {
            f7.dialog.alert('获取问题类别失败!');
          }
        },
        error: err => {
          f7.preloader.hide();
          f7.dialog.alert('获取问题类别失败!');
        }
      })
    });
  }
}

</script>