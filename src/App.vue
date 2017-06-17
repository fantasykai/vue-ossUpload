<template>
  <div id="app">
    <upload-ali-oss :url.sync="uploadForm.content.path" :multiple="true"
                    :id="uploadForm.id"
                    :ossClient="this.ossClient"
                    :inputName="uploadForm.inputName"
                    :path="uploadForm.upload.path"
                    :uploadRes.sync="uploadForm.uploadRes"
    ></upload-ali-oss>
    <div v-show="uploadForm.uploadRes"
         v-text="uploadForm.content.path">
    </div>
    <div>
      <img v-if="uploadForm.uploadRes"
           :src="this.imgSrc">
    </div>
  </div>
</template>

<script>
  import UploadAliOSS from './UploadAliOSS.vue';
  export default {
    name: 'app',
    components: {
      'upload-ali-oss': UploadAliOSS
    },
    data() {
      return {
        ossClient: Object,
        uploadForm: {
          id: 'imgFile',
          inputName: '上传图片',// 自定义
          content: {
            path: '',
          },
          upload: {
            path: 'img/test/',// 自定义路径
          },
          uploadRes: false,
        },
        imgSrc: ''
      }
    },
    methods: {
      //初始化OSS 权限, 建议后台提供获取oss临时权限的接口
      initOSSAuth() {

        let ossSts = JSON.parse(localStorage.getItem('ossSts'));

        let Oss = OSS.Wrapper;

        this.ossClient = new Oss({
          region: '',
          accessKeyId: '',
          accessKeySecret: '',
          stsToken: '',
          bucket: '',
          endpoint: '',
        });
      },
      showUploadContent() {
        if (this.uploadForm.content.path) {

          let path = this.uploadForm.content.path;

          var result = this.ossClient.signatureUrl(path, {
            response: {
              // 'content-disposition': 'attachment; filename="' + filename + '"'
              'Content-Type': 'image/jpeg'
            }
          });
          this.imgSrc = result;
        }
      }
    },
    watch: {
      'uploadForm.content.path' (val, oldVal) {
//                console.log('new: %s, old: %s', val, oldVal)
        if ('' !== val) {
          this.showUploadContent();
        }
      }
    },
    mounted() {
      this.initOSSAuth();
    }
  }
</script>

<style>

</style>
