# 吴温博写的七牛上传图片

```javascript
1.methods
addWarePic: function (event) {
                var picId = $(event.currentTarget).find("img").attr("id").toString();
                var containerId=$(event.currentTarget).parent().attr("id").toString();
                var buttonId= $(event.currentTarget).attr("id").toString();
                var myUptoken="";
                var myKey="";
                var Qiniu = new QiniuJsSDK();
                var $this=this;
                var option = {
                    runtimes: 'html5,flash,html4',      // 上传模式，依次退化
                    browse_button: buttonId,         // 上传选择的
                    uptoken_func: function (file) {
                        $.ajax({
                            url:$this.getTokenUrl,
                            type:"POST",
                            async:false,
                            dataType:"json",
                            data:{
                                path:"comm",
                            },
                            success:function(data)
                            {
                                if(data.error==0){
                                    myUptoken =data.uptoken;
                                    myKey = data.key;
                                }else{
                                    alert(data.msg);
                                }
                            },
                            error:function(a,b,c)
                            {
                                console.log(a+b+c);
                            }
                        });
                        return myUptoken;
                    },
                    get_new_uptoken: true,
                    unique_names: false,              // 默认false，key为文件名。若开启该选项，JS-SDK会为每个文件自动生成key（文件名）
                    save_key: false,                  // 默认false。若在服务端生成uptoken的上传策略中指定了sava_key，则开启，SDK在前端将不对key进行任何处理
                    domain: 'http://qiniu.yoogus.com',     // bucket域名，下载资源时用到，必需
                    container: containerId,             // 上传区域DOM ID，默认是browser_button的父元素
                    max_file_size: '100mb',             // 最大文件体积限制
                    flash_swf_url: $this.getMoxieUrl,
                    max_retries: 3,                     // 上传失败最大重试次数
                    dragdrop: true,
                    drop_element: containerId,          // 拖曳上传区域元素的ID，拖曳文件或文件夹后可触发上传
                    chunk_size: '4mb',                  // 分块上传时，每块的体积
                    auto_start: true,
                    init:{
                        'FilesAdded': function(up, files) {
                            plupload.each(files, function(file) {
                                // 文件添加进队列后，处理相关的事情
                            });
                        },
                        'BeforeUpload': function(up, file) {
                            // 每个文件上传前，处理相关的事情
                        },
                        'UploadProgress': function(up, file) {
                            // 每个文件上传时，处理相关的事情
                        },
                        'FileUploaded': function(up, file, info) {
                            // 每个文件上传成功后，处理相关的事情
                            // 其中info是文件上传成功后，服务端返回的json，形式如：
                            // {
                            //    "hash": "Fh8xVqod2MQ1mocfI4S4KpRL6D98",
                            //    "key": "gogopher.jpg"
                            //  }
                            // 查看简单反馈
                            // var domain = up.getOption('domain');
                            var res = JSON.parse(info);
                            //img.push(res.key)   ; //加入到数组中，给后台传数据
                            var domain = up.getOption('domain');
                            var picPrview = domain +"/"+ res.key; //获取上传成功后的文件的Url
                            document.getElementById(buttonId).setAttribute("disabled","disabled");
                            document.getElementById(buttonId).setAttribute("disabled","disabled");
                            document.getElementById(picId).setAttribute("src",picPrview);
                            document.getElementById(picId).setAttribute("data-key",res.key);
                        },
                        'Error': function(up, err, errTip) {
                            //上传出错时，处理相关的事情
                        },
                        'UploadComplete': function() {
                            //队列文件处理完毕后，处理相关的事情
                        },
                        'Key': function(up, file) {
                            // 若想在前端对每个文件的key进行个性化处理，可以配置该函数
                            // 该配置必须要在unique_names: false，save_key: false时才生效
                            var key = myKey;
                            // do something with key here
                            return key
                        }
                    }
                };
                var uploader = Qiniu.uploader(option);
                uploader.start();
            }
        },

	vm.$data.getTokenUrl=tokenUrl;
    vm.$data.getMoxieUrl=MoxieUrl;

2.html
    <script src="{{asset('mobile/store/js/plupload.full.min.js')}}"></script>
    <script src="{{asset('mobile/store/js/plupload.dev.js')}}"></script>
    <script src="{{asset('mobile/store/js/plupload.min.js')}}"></script>
    <script src="{{asset('mobile/store/js/qiniu.min.js')}}"></script>
    <script src="{{asset('mobile/store/js/moxie.min.js')}}"></script>

                <div id="area_longPic">
                    <button id="button_addLongPic" v-on:click="addWarePic($event)">
                        <img src="" id="img_addLongPic" alt="">
                    </button>
                </div>
           




```