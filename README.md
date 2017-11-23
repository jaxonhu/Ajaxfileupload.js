# Ajaxfileupload.js
jquery ajax 文件上传的插件，验证可用。

> 前端用例

```

function imageUpload(){
        alert("hello");
        $.ajaxFileUpload({
            url : '/BusinessTrip/upload', //用于文件上传的服务器端请求地址
            fileElementId : 'image_face', //文件上传空间的id属性  <input type="file" id="file" name="file" />
            type : 'post',
            dataType : 'text', //返回值类型 一般设置为json
            success : function(data, status) //服务器成功响应处理函数
            {
                alert("头像上传成功");
                //$("#picList").datagrid('reload');
                //$('#uploadPicWindow').window('close');
                // alert(data.msg);
            },
            error : function(data, status, e)//服务器响应失败处理函数
            {
                alert("图片上传失败");
                //$("#picList").datagrid('reload');
                //$('#uploadPicWindow').window('close');
                // alert(data.msg);
            }
        });
    }
   

```

    

> 服务器端用例


```

        @Controller
        public class UploadController {

            @Resource
            public UserClientService userService;

            @RequestMapping(value = "upload", method = RequestMethod.POST)
            public @ResponseBody
            String upload(HttpServletRequest request,
                          HttpServletResponse response, ModelMap model,HttpSession session) throws IOException {
                MultipartHttpServletRequest multipartRequest = (MultipartHttpServletRequest) request;
                MultipartFile mFile = multipartRequest.getFile("file");
                String path = request.getSession().getServletContext().getRealPath("/resources/upload");
                String filename = mFile.getOriginalFilename();
                InputStream inputStream = mFile.getInputStream();
                byte[] b = new byte[1048576];
                int length = inputStream.read(b);
                Date date = new Date();
                SimpleDateFormat F = new SimpleDateFormat("yyyyMMddHHmmss");
                String prefix=filename.substring(filename.lastIndexOf("."));
                filename = "123" + F.format(date) + prefix;
                String url =path +"/"+ filename;
                System.out.println(url);
                FileOutputStream outputStream = new FileOutputStream(url);
                outputStream.write(b, 0, length);
                inputStream.close();
                outputStream.close();
                return "success";
            }
        }

```
