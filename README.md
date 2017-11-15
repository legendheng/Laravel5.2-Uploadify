# Laravel5.2-Uploadify
Laravel5.2-结合Uploadify插件实现无刷新上传图片功能
##完整实例demo ，注意（这里用了文档两个方法，一是buttonText属性修改文字，二是onUploadSuccess成功上传返回，）
```html
<script src="{{asset('resources/org/uploadify/jquery.uploadify.min.js')}}" type="text/javascript"></script>
<link rel="stylesheet" type="text/css" href="{{asset('resources/org/uploadify/uploadify.css')}}">
<input id="file_upload" name="file_upload" type="file" multiple="true">
<img src="" alt="" name="" id="art_thumb_img" style="max-width: 150px;max-height: 150px">
<script type="text/javascript">
  <?php $timestamp = time();?>
  $(function() {
      $('#file_upload').uploadify({
          'buttonText' : '图片上传',
          'formData'     : {
              'timestamp' : '<?php echo $timestamp;?>',
              '_token'     : "{{csrf_token()}}"
          },
          'swf'      : '{{asset('resources/org/uploadify/uploadify.swf')}}',
          'uploader' : "{{url('admin/upload')}}",
          'onUploadSuccess' : function(file, data, response) {
              $('#art_thumb_img').attr('src','../../public/uploads/'+data);   //上图片后马上显示  
          }
      });
  });
</script>
```
### 第一步、解压uploadify压缩包到开发项目的resources目录，并新建org目录(用于放第三方的类库资源)，此示例解压到resourcs/org/uploadify目录
### 第二步、在前端的页面引入uploadify类的js、css文件
```html
<script src="{{asset('resources/org/uploadify/jquery.uploadify.min.js')}}" type="text/javascript"></script>
<link rel="stylesheet" type="text/css" href="{{asset('resources/org/uploadify/uploadify.css')}}">
```
### 第三步、使用Uploadify的uploader属性编辑提交方法
```php
'uploader' : "{{url('admin/upload')}}"
```
### 第四步、新建一条路由，注意这里要用post或者any方法（坑过）
```php
Route::post('upload','TestController@upload');//图片上传路由
```
### 第五步、编写控制器TestController的upload方法
```php
public function upload(){
        $input=Input::file('Filedata');//只获取Filedata字段的内容，这个Filedata是文件上传有的
        if ($input->isValid()){     //检验文件上传是否有效
            $extension=$input->getClientOriginalExtension();    //获取文件后缀
            $filename=date('YmdHis').mt_rand(100,999).'.'.$extension;    //文件名
            $input->move('public\uploads',$filename);    //移到public的uploads下
           return $filename;
        }
    }
```
### 第六步、前端编写获取返回图片html+jq
```html
<img src="" alt="" name="" id="art_thumb_img" style="max-width: 150px;max-height: 150px">
$('#art_thumb_img').attr('src','../../public/uploads/'+data);   //上图片后马上显示
```

