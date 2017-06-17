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
          inputName: '上传图片',
          content: {
            path: '',
          },
          upload: {
            path: 'chat/c2c',
          },
          uploadRes: false,
        },
      }
    },
    methods: {
      //初始化OSS 权限, 建议后台提供获取oss临时权限的接口
      initOSSAuth(){

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
    },
    mounted() {
      this.initOSSAuth();
    }
  }
</script>

<style>

</style>
