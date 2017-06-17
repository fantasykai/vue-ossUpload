<template>
  <div>
    <div class="oss_file">{{ this.inputName }}
            <input type="file" :id="id" :multiple="multiple" @change="doUpload"/>
    </div>
  </div>
</template>
<script>
  export default{
    name: 'uploadAliOSS',
    props: {
      ossClient: {
        type: Object,
        toWay: false
      },
      multiple: {
        type: Boolean,
        twoWay: false
      },
      id: {
        type: String,
        twoWay: false
      },
      url: {
        type: String,
        twoWay: true
      },
      uploadRes: {
        type: Boolean,
        twoWay: true
      },
      inputName: {
        type: String,
      },
      path: {
        type: String,
      },
    },
    data(){
      return {
        resultUpload: '',
        content: {
          path: this.path,
        }
      };
    },
    methods: {
      doUpload(){
        const _this = this;
        const files = document.getElementById(_this.id);
        if (files.files) {

          const fileLen = document.getElementById(_this.id).files
          for (let i = 0; i < fileLen.length; i++) {
            const file = fileLen[i];
            const storeAs = file.name;
            let suffix = storeAs.split(".")[1]

            let storeKey = _this.content.path + '.' + suffix;
            _this.resultUpload = storeKey;
            _this.ossClient.multipartUpload(storeKey, file, {}).then((results) => {
              this.$emit('update:uploadRes', true);
            }).catch((err) => {
              console.log(err);
            });
          }
          _this.url = this.resultUpload;

          _this.$emit('update:url', this.resultUpload);
        }
      }
    },
    mounted() {
    }
  };
</script>

<style type="text/css">
  .oss_file {
    position: relative;
    display: inline-block;
    background: #D0EEFF;
    border: 1px solid #99D3F5;
    border-radius: 4px;
    padding: 4px 12px;
    overflow: hidden;
    color: #1E88C7;
    text-decoration: none;
    text-indent: 0;
    line-height: 20px;
  }

  .oss_file input {
    position: absolute;
    font-size: 100px;
    right: 0;
    top: 0;
    opacity: 0;
  }

  .oss_file:hover {
    background: #AADFFD;
    border-color: #78C3F3;
    color: #004974;
    text-decoration: none;
  }
</style>
