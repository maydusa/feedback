<template>
  <f7-page name="edit-feedback" class="content">
    <f7-navbar title="帮助与反馈" back-link="返回"></f7-navbar>
    <f7-block-title>问题标题</f7-block-title>
    <f7-block strong>
      <f7-input
        type="text"
        placeholder="您的问题"
        inputStyle="width: 100%; padding: 8px 0"
        :value="question"
        @input="question = $event.target.value"
      >
      </f7-input>
    </f7-block>
    <f7-block-title>请选择问题类型</f7-block-title>
    <f7-block strong>
      <f7-input
        type="select"
        :defaultValue="catagories.length == 0 ? '' : catagories[0].content"
        placeholder="您的问题类型"
        inputStyle="width: 100%; padding: 8px 0"
        :value="type"
        @input="type = $event.target.value"
      >
        <option v-for="(item, index) in catagories" :key="item.id" :value="item.id" :data-id="item.id">{{item.content}}</option>
      </f7-input>
    </f7-block>
    <f7-block-title>问题和意见</f7-block-title>
    <f7-block strong>
      <f7-input
        type="textarea"
        minlength="10"
        placeholder="请填写10个字以上的问题描述以使我们提供更好的帮助"
        inputStyle="width: 100%; height: 80px; padding: 8px 0"
        :value="suggestion"
        @input="suggestion = $event.target.value"
      >
      </f7-input>
    </f7-block>
    <f7-block-title>问题截图</f7-block-title>
    <f7-block strong>
      <Upload @update_images="handle_update_images" @cancel_image="handle_cancel_image" />
    </f7-block>
    <f7-block-title>联系方式(手机)</f7-block-title>
    <f7-block strong>
      <f7-input
        type="tel"
        placeholder="选填，方便我们与您联系"
        inputStyle="width: 100%; padding: 8px 0"
        :value="phone"
        @input="phone = $event.target.value"
        error-message="Invalid"
        error-message-force
      >
      </f7-input>
    </f7-block>
    <f7-block>
      <f7-button fill @click="submit">提交</f7-button>
    </f7-block>
  </f7-page>
</template>

<script>
import Upload from '../components/Upload';
import { static_img_domain, api_domain } from '../js/config.js';

export default {
  name: 'edit-feedback',
  components: {Upload},
  data () {
    return {
      question: '',
      type: '',
      suggestion: '',
      phone: '',
      catagories: [],
      images: []
    }
  },
  mounted () {
    this.$f7ready((f7) => {
      f7.request({
        url: `${api_domain}/get_all_type`,
        method: 'GET',
        dataType: 'json',
        success: res => {
          if (res.code == 0) {
            this.catagories = res.data.data;
          } else {
            f7.dialog.alert('获取问题类别失败!');
          }
        },
        error: err => {
          f7.dialog.alert('获取问题类别失败!');
        }
      })
    });
  },
  methods: {
    submit () {
      let data = {
        'title': this.question,
        'content': this.suggestion,
        'type': this.type ? this.type : this.catagories[0].id,
        'phone': this.phone,
        'img_url': this.images
      };
      this.$f7.request({
        url: `${api_domain}/add_feedback`,
        method: 'POST',
        dataType: 'json',
        data: data,
        success: res => {
          this.$f7.dialog.alert('提交反馈成功!');
        },
        error: err => {
          this.$f7.dialog.alert('提交反馈失败!');
        }
      })
    },
    handle_update_images (image_url) {
      let images = this.images;
      images.push(image_url);
      this.images = images;
    },
    handle_cancel_image (guid) {
      let images = this.images;
      let prefix = static_img_domain + '/image/';
      for (let i = 0; i < images.length; i++) {
        if (images[i].substring(prefix.length, 36) == guid) {
          images.splice(i, 1);
          this.images = images;
          break;
        }
      }
    }
  }
}
</script>