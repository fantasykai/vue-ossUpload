> support ali-oss uplaod func

[![Vue 2.3.0+](https://img.shields.io/badge/Vue-2.x-brightgreen.svg)](https://cn.vuejs.org/)

[![NPM](https://nodei.co/npm/vue-oss-upload.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/vue-oss-upload/)

- [github](https://github.com/fantasykai/vue-ossUpload)

# vue-oss-upload

## Note

### 需要依赖阿里云的oss的SDK,可以在index.html中引入

`<script type="text/javascript" src="https://gosspublic.alicdn.com/aliyun-oss-sdk.min.js"></script>`

### 组件依赖vue2.3以上的版本

vue2.3 重新引入了 .sync 修饰符，但是这次它只是作为一个编译时的语法糖存在。它会被扩展为一个自动更新父组件属性的 v-on 侦听器。

[.sync 修饰符](https://cn.vuejs.org/v2/guide/components.html#counter-event-example)

### 使用组件时，需要导入Css样式。自己不擅长写样式，写的比较挫，希望可以被改进，哈哈

### TODO: 上传的进度，等尚未实现，只有最终上传结果

## Use Setup

### Install vue-oss-upload

``` bash
npm install vue-oss-upload --save
```

### props

     props: {
          // 需要引入阿里云OSS的SDK，并初始化ossClient
          ossClient: {
            type: Object,
            toWay: false
          },
          // 是非支持多文件上传
          multiple: {
            type: Boolean,
            twoWay: false
          },
          // 上传组件id，区别页面中多次使用上传组件
          id: {
            type: String,
            twoWay: false
          },
          // 上传成功后的uri
          url: {
            type: String,
            twoWay: true
          },
           // 上传结果
          uploadRes: {
            type: Boolean,
            twoWay: true
          },
          // 上传按钮的名称
          inputName: {
            type: String,
          },
          // 上传到OSS的路径
          path: {
            type: String,
          },
        },


## Example

    <template>
        <div>
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
        import VueUploadAliOSS from 'vue-oss-upload';
        import Css from 'vue-oss-upload/dist/vue-alioss-upload.min.css'
        export default {
            name: 'app',
            components: {
                'upload-ali-oss': VueUploadAliOSS
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
                // 需要引入阿里云的ossSDK
                //初始化OSS 权限, 建议后台提供获取oss临时权限的接口
                initOSSAuth() {

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
                // 展示上传的内容（图片）
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

### 展示效果

![](https://ws1.sinaimg.cn/large/7108d6c2ly1fgqi3miznuj20kj0l4k4f.jpg)

# Author Blog
[kai.fantasy](https://fantasykai.cc)
