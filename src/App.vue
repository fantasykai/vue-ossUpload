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
      //初始化OSS 权限
      initOSSAuth(){

        let ossSts = JSON.parse(localStorage.getItem('ossSts'));

        let Oss = OSS.Wrapper;

        this.ossClient = new Oss({
          region: 'oss-cn-beijing',
          accessKeyId: 'STS.KiWhDb3s9JQfQTXLFosT3bK6Y',
          accessKeySecret: 'YhdRHdJrS56vJvfuvPatmaGzQGWS9wWhFHch3pqBiM1',
          stsToken: 'CAIS/wF1q6Ft5B2yfSjIqqriI/7W3qwY/ZONU3LpqEY6f9sfjY6duzz2IHlNfnJtAO4XtPsyn2lS7fcTlr90UIQAXV3Ybcx2q49b8kasc4PEo8i4tUeBAFkGWjr9MQXy+eOPScebJYqvV5XAQlTAkTAJstmeXD6+XlujHISUgJp8FLo+VRW5ajw0b7U/ZHEVyqkgOGDWKOymPzPzn2PUFzAIgAdnjn5l4qnNqa/1qDim1QCjkbBI/96rc8b4MJg9Y60SCYnlgLZEEYPayzNV5hRw86N7sbdJ4z+vvKvGWgALuEjfbbuOrYA0dVUiN/kgeaRAsfHkjbh/offDAmhBJomyjYsagAGoHnoPwjaBxhvAScxgf5KhekToKoY8b03e4KuLURp8Z98HtpgB+cfLF2mGh/3Jr9g96rORMFv3IRESeY3xyeWl0odcyDVs9NonuMo4b4AoeKC4EJeA39WzINICz7RKRKwCPFSIreE32kKUcuA3J5XeMEP3N8f7jaEtsXBkP7wPpA==',
          bucket: 'dianxinonline',
          endpoint: 'https://oss-cn-beijing.aliyuncs.com',
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
