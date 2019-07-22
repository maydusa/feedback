<template>
  <div class="upload">
    <div class="upload-image-item image-block" v-for="(item, index) in images" :key="item.url" :data-index="index" :data-filename="item.filename" :style="{ backgroundImage: 'url(' + item.url + ')' }" @click="image_preview($event)">
      <svg t="1563618413977" :data-filename="item.filename" :data-guid="item.guid" @click.stop="cancel($event)" class="cancel" :style="{ display: item.isUpload ? 'block' : 'none' }" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2612" width="18" height="18"><path d="M512 0C230.4 0 0 230.4 0 512c0 281.6 230.4 512 512 512 281.6 0 512-230.4 512-512C1024 230.4 793.6 0 512 0zM716.8 563.2 307.2 563.2c-30.72 0-51.2-20.48-51.2-51.2 0-30.72 20.48-51.2 51.2-51.2l409.6 0c30.72 0 51.2 20.48 51.2 51.2C768 542.72 747.52 563.2 716.8 563.2z" p-id="2613" fill="#ff4d4f"></path></svg>
      <svg t="1563606276439" :data-filename="item.filename" class="loading" :style="{ display: !item.isUpload && !item.isFailure ? 'block' : 'none' }" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="4374" width="32" height="32"><path d="M843.307 742.24c0 3.217 2.607 5.824 5.824 5.824s5.824-2.607 5.824-5.824a5.823 5.823 0 0 0-5.824-5.824 5.823 5.823 0 0 0-5.824 5.824zM714.731 874.912c0 6.398 5.186 11.584 11.584 11.584s11.584-5.186 11.584-11.584-5.186-11.584-11.584-11.584-11.584 5.186-11.584 11.584zM541.419 943.2c0 9.614 7.794 17.408 17.408 17.408s17.408-7.794 17.408-17.408-7.794-17.408-17.408-17.408-17.408 7.794-17.408 17.408z m-186.56-9.152c0 12.795 10.373 23.168 23.168 23.168s23.168-10.373 23.168-23.168-10.373-23.168-23.168-23.168-23.168 10.373-23.168 23.168zM189.355 849.12c0 16.012 12.98 28.992 28.992 28.992s28.992-12.98 28.992-28.992-12.98-28.992-28.992-28.992-28.992 12.98-28.992 28.992zM74.731 704.736c0 19.228 15.588 34.816 34.816 34.816s34.816-15.588 34.816-34.816-15.588-34.816-34.816-34.816-34.816 15.588-34.816 34.816z m-43.008-177.28c0 22.41 18.166 40.576 40.576 40.576s40.576-18.166 40.576-40.576-18.166-40.576-40.576-40.576-40.576 18.166-40.576 40.576z m35.392-176.128c0 25.626 20.774 46.4 46.4 46.4s46.4-20.774 46.4-46.4c0-25.626-20.774-46.4-46.4-46.4-25.626 0-46.4 20.774-46.4 46.4z m106.176-142.016c0 28.843 23.381 52.224 52.224 52.224s52.224-23.381 52.224-52.224c0-28.843-23.381-52.224-52.224-52.224-28.843 0-52.224 23.381-52.224 52.224z m155.904-81.344c0 32.024 25.96 57.984 57.984 57.984s57.984-25.96 57.984-57.984-25.96-57.984-57.984-57.984-57.984 25.96-57.984 57.984z m175.104-5.056c0 35.24 28.568 63.808 63.808 63.808s63.808-28.568 63.808-63.808c0-35.24-28.568-63.808-63.808-63.808-35.24 0-63.808 28.568-63.808 63.808z m160.32 72.128c0 38.421 31.147 69.568 69.568 69.568s69.568-31.147 69.568-69.568-31.147-69.568-69.568-69.568-69.568 31.147-69.568 69.568z m113.92 135.488c0 41.638 33.754 75.392 75.392 75.392s75.392-33.754 75.392-75.392-33.754-75.392-75.392-75.392-75.392 33.754-75.392 75.392z m45.312 175.488c0 44.854 36.362 81.216 81.216 81.216s81.216-36.362 81.216-81.216c0-44.854-36.362-81.216-81.216-81.216-44.854 0-81.216 36.362-81.216 81.216z" fill="#ffffff" p-id="4375"></path></svg>
      <svg t="1563693442275" :data-filename="item.filename" class="failure" :style="{ display: item.isFailure ? 'block' : 'none' }" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="8586" width="32" height="32"><path d="M999.328 876.787l-415.774-786.738c-9.992-16.896-28.118-28.048-48.849-28.048s-38.858 11.153-48.706 27.787l-415.917 787c-4.897 8.2-7.793 18.087-7.793 28.65 0 31.138 25.161 56.398 56.258 56.562h831.563c0.133 0.001 0.29 0.002 0.447 0.002 31.239 0 56.563-25.324 56.563-56.563 0-10.563-2.895-20.45-7.937-28.91zM534.337 282.508c40.402 0 94.761 47.748 80.804 146.917l-38.932 197.603c-1.141 22.269-18.928 40.035-41.103 41.137-22.303-1.14-40.070-18.928-41.169-41.102l-44.81-204.248c-2.527-8.918-3.98-19.16-3.98-29.74 0-53.998 37.834-99.16 88.438-110.424zM534.337 920.861c-56.797 0-102.842-46.044-102.842-102.842s46.044-102.842 102.842-102.842c56.797 0 102.842 46.044 102.842 102.842-2.033 55.195-47.27 99.169-102.775 99.169-0.023 0-0.046 0-0.069 0z" fill="#ff4d4f" p-id="8587"></path></svg>
      <div class="mask" :data-filename="item.filename" :style="{ display: item.isUpload ? 'none' : 'block' }"></div>
    </div>
    <div class="upload-btn image-block" @click="upload">
      <input type="file" id="file" accept="image/gif, image/jpeg, image/jpg, image/png" @change="select($event)">
      <svg viewBox="64 64 896 896" focusable="false" class="" data-icon="plus" width="1em" height="1em" fill="currentColor" aria-hidden="true"><path d="M482 152h60q8 0 8 8v704q0 8-8 8h-60q-8 0-8-8V160q0-8 8-8z"></path><path d="M176 474h672q8 0 8 8v60q0 8-8 8H176q-8 0-8-8v-60q0-8 8-8z"></path></svg>
      <div class="upload-text">Upload</div>
    </div>
  </div>
</template>
<script>
  import { static_img_domain, api_domain } from '../js/config.js';

  export default {
    name: 'Upload',
    data () {
      return {
        images: [],
        files: [],
        images_preview_group: [],
        images_previewer: null
      }
    },
    mounted () {
      this.$f7ready((f7) => {});
    },
    methods: {
      upload () {
        this.$$('#file')[0].click();
      },
      select ($event) {
        if ($event.target.files.length == 0) return;
        let reader = new FileReader();
        reader.readAsDataURL($event.target.files[0]);
        reader.onload = e => {
          let image = {
            url: e.target.result,
            filename: $event.target.files[0].name,
            isUpload: false,
            isFailure: false,
            guid: ''
          };
          let images_preview_item = {
            url: e.target.result,
            caption: $event.target.files[0].name
          };
          this.images_preview_group = [images_preview_item, ...this.images_preview_group];
          this.images = [image, ...this.images];
          if (this.images_previewer) {
            this.$f7.photoBrowser.destroy(this.images_previewer);
          }
          this.images_previewer = this.$f7.photoBrowser.create({
            photos: this.images_preview_group
          });
        }
        this.$f7.request({
          url: `${api_domain}/get_code`,
          method: 'GET',
          dataType: 'json',
          success: res => {
            let formData = new FormData();
            formData.append('image', $event.target.files[0]);
            let image_type = $event.target.files[0].type.substring(6);
            let url = `${static_img_domain}/upload/${res.data.access_token}-${res.data.nonce}.${image_type}`;
            this.$f7.request({
              url: url,
              method: 'POST',
              contentType: 'multipart/form-data',
              dataType: 'json',
              data: formData,
              success: res => {
                let image_url = `${static_img_domain}/image/${res.message}-100-100.${image_type}`;
                this.$emit('update_images', image_url);
                let images = this.images;
                images[0].isUpload = true;
                images[0].guid = res.message;
                this.images = images;
              },
              error: err => {
                let images = this.images;
                images[0].isFailure = true;
                this.images = images;
              }
            })
          },
          error: err => {
            let images = this.images;
            images[0].isFailure = true;
            this.images = images;
          }
        });
      },
      cancel ($event) {
        let current = $event.currentTarget;
        let images = this.images, cancel_image = '', images_preview_group = this.images_preview_group;
        for (let i = 0; i < images.length; i++) {
          if (images[i].filename == current.dataset.filename) {
            images.splice(i, 1);
            cancel_image = current.dataset.filename;
            this.images = images;
            this.$emit('cancel_image', current.dataset.guid);
            break;
          }
        }
        for (let i = 0; i < images_preview_group.length; i++) {
          if (images_preview_group[i].caption == cancel_image) {
            images_preview_group.splice(i, 1);
            this.images_preview_group = images_preview_group;
            break;
          }
        }
        this.$f7.photoBrowser.destroy(this.images_previewer);
        this.images_previewer = this.$f7.photoBrowser.create({
          photos: images_preview_group
        });
      },
      image_preview ($event) {
        let current = $event.currentTarget;
        this.images_previewer.open(Number(current.dataset.index))
      }
    }
  };
</script>

<style>
.upload-image-item {
  width: 104px;
  height: 104px;
  box-sizing: border-box;
  border-radius: 4px;
  border: 5px solid #fff;
  background-size: contain;
  background-repeat: no-repeat;
  box-shadow: #f0f1f2 0 0 10px;
  display: inline-block;
  margin-right: 8px;
  margin-bottom: 8px;
  position: relative;
}
.image-block {
  display: inline-block;
  margin-right: 8px;
  margin-bottom: 8px;
}
.mask {
  width: 94px;
  height: 94px;
  box-sizing: border-box;
  position: absolute;
  top: 0px;
  left: 0px;
  background-color: black;
  opacity: 0.5;
}
.cancel {
  position: absolute;
  top: -10px;
  right: -10px;
  z-index: 999;
  display: none;
}
.loading {
  animation: rotate 3s infinite;
  -webkit-animation: rotate 3s infinite;
  animation-timing-function: linear;
  -webkit-animation-timing-function: linear;
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
  z-index: 999;
}
.failure {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin: auto;
  z-index: 999;
  display: none;
}
.upload-btn {
  width: 104px;
  height: 104px;
  background-color: #fafafa;
  border: 1px dashed #1890ff;
  border-radius: 4px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  box-sizing: border-box;
}
.upload-btn svg {
  font-size: 32px;
  color: #999;
  line-height: 1;
}
.upload-text {
  font-size: 14px;
  line-height: 1.5;
}
#file {
  display: none;
}

@keyframes rotate
{
from {transform: rotate(0deg);}
to {transform: rotate(360deg);}
}

@-moz-keyframes rotate /* Firefox */
{
from {transform: rotate(0deg);}
to {transform: rotate(360deg);}
}

@-webkit-keyframes rotate /* Safari 和 Chrome */
{
from {transform: rotate(0deg);}
to {transform: rotate(360deg);}
}

@-o-keyframes rotate /* Opera */
{
from {transform: rotate(0deg);}
to {transform: rotate(360deg);}
}
</style>