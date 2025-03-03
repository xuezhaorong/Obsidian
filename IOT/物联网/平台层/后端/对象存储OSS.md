## 服务器OSS配置
1. 进入阿里云对象存储OSS，网站链接：https://oss.console.aliyun.com/overview，并创建一个`Bucket`。
![image.png|1000](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/03/20-02-50-5d8bab29a02ceccf46b531925820a27b-20250303200249-092087.png)

2. 写入`Bucket`名称和修改地域后，完成创建。
![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/03/20-04-11-f2ae9f360f1623681e9baded778e846e-20250303200411-60edea.png)

3. 在Bucket列表中点击新创建的Bucket，然后修改访问权限，关闭阻止公共访问，在读写权限中设置为公共读。
![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/03/20-09-36-f5b19005cf0a6a5a7f731515a87830db-20250303200935-e53a87.png)

![image.png|900](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/03/20-10-26-e73f2d2e8ec3c414977e1cf55e3a3562-20250303201026-0c39bd.png)

4. 点击用户头像下拉框中的AccessKey，创建一个AccessKey并且记录下来。
![|632](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/03/20-12-08-0f82becf2e291eb80a61d07c6f9daa28-20250303201207-1a73f2.png)

![image.png|700](https://cdn.jsdelivr.net/gh/xuezhaorong/Picgo//Source/fix-dir/picgo/picgo-clipboard-images/2025/03/03/20-14-10-d2eb7bcc8c7ca93f391a93f7aa80469c-20250303201410-eea0e5.png)

5. 为服务器添加AccessKey的环境变量
### 文件上传
#### 后端配置
网页链接：https://help.aliyun.com/zh/oss/developer-reference/preface-1/?spm=5176.8466032.console-base_help.dexternal.30e41450pvjSI1
1. 工具包安装，在pom中粘贴。
```xml
<!--阿里云oss-->  
<dependency>  
    <groupId>com.aliyun.oss</groupId>  
    <artifactId>aliyun-sdk-oss</artifactId>  
    <version>3.17.4</version>  
</dependency>  
<dependency>  
    <groupId>javax.xml.bind</groupId>  
    <artifactId>jaxb-api</artifactId>  
    <version>2.3.1</version>  
</dependency>  
<dependency>  
    <groupId>javax.activation</groupId>  
    <artifactId>activation</artifactId>  
    <version>1.1.1</version>  
</dependency>  
<!-- no more than 2.3.3-->  
<dependency>  
    <groupId>org.glassfish.jaxb</groupId>  
    <artifactId>jaxb-runtime</artifactId>  
    <version>2.3.3</version>  
</dependency>
```

2. 新建工具类`utils.AliOssUtil`，参数`floderName`为目标目录的名称，需要先在OSS管理中为该Bucket创建对应的目录，参数`objectName`为要上传文件的名称，参数`inputStream`的输入流，上传成功后返回对应的`url`。
```java
public class AliOssUtil {  
  
    // Endpoint以深圳为例，其它Region请按实际情况填写。  
    private static final String ENDPOINT = "https://oss-cn-shenzhen.aliyuncs.com";  
  
    // 填写Bucket名称，例如examplebucket。  
    private static final String BUCKET_NAME = "BucketName";  
  
    // 填写Bucket所在地域。以深圳为例。  
    private static final String REGION = "cn-shenzhen";  
  
    public static String uploadFile(String folderName,String objectName, InputStream inputStream) throws Exception {  
  
        // 从环境变量中获取访问凭证。运行本代码示例之前，请确保已设置环境变量OSS_ACCESS_KEY_ID和OSS_ACCESS_KEY_SECRET。  
        EnvironmentVariableCredentialsProvider credentialsProvider = CredentialsProviderFactory.newEnvironmentVariableCredentialsProvider();  
  
        // 创建OSSClient实例。  
        ClientBuilderConfiguration clientBuilderConfiguration = new ClientBuilderConfiguration();  
        clientBuilderConfiguration.setSignatureVersion(SignVersion.V4);  
        OSS ossClient = OSSClientBuilder.create()  
                .endpoint(ENDPOINT)  
                .credentialsProvider(credentialsProvider)  
                .clientConfiguration(clientBuilderConfiguration)  
                .region(REGION)  
                .build();  
        String url = "";  
        try {  
  
            objectName = folderName + "/" + objectName;  
            // 创建PutObjectRequest对象。  
            PutObjectRequest putObjectRequest = new PutObjectRequest(BUCKET_NAME, objectName, inputStream);  
  
            // 如果需要上传时设置存储类型和访问权限，请参考以下示例代码。  
            // ObjectMetadata metadata = new ObjectMetadata();  
            // metadata.setHeader(OSSHeaders.OSS_STORAGE_CLASS, StorageClass.Standard.toString());            // metadata.setObjectAcl(CannedAccessControlList.Private);            // putObjectRequest.setMetadata(metadata);  
            // 上传字符串。  
            PutObjectResult result = ossClient.putObject(putObjectRequest);  
            // url组成：https://bucket名称.区域节点/目录/objectName  
            url = "https://"+BUCKET_NAME+"."+ENDPOINT.substring(ENDPOINT.lastIndexOf("/")+1)+"/"+objectName;  
        } catch (OSSException oe) {  
            System.out.println("Caught an OSSException, which means your request made it to OSS, "  
                    + "but was rejected with an error response for some reason.");  
            System.out.println("Error Message:" + oe.getErrorMessage());  
            System.out.println("Error Code:" + oe.getErrorCode());  
            System.out.println("Request ID:" + oe.getRequestId());  
            System.out.println("Host ID:" + oe.getHostId());  
        } catch (ClientException ce) {  
            System.out.println("Caught an ClientException, which means the client encountered "  
                    + "a serious internal problem while trying to communicate with OSS, "  
                    + "such as not being able to access the network.");  
            System.out.println("Error Message:" + ce.getMessage());  
        } finally {  
            if (ossClient != null) {  
                ossClient.shutdown();  
            }  
        }  
        return url;  
    }  
}
```

3. 新建一个文件上传控制层`FileUploadController`
```java
@RestController  
public class FileUploadController {  
  
    @PostMapping("/upload")  
    public Result<String> upload(@RequestParam("file") MultipartFile file,@RequestParam("folderName") String folderName) throws Exception {  
        // 把文件内容存储到本地磁盘上  
        String originalFilename = file.getOriginalFilename();  
        // 保证文件名称唯一  
        String filename = UUID.randomUUID().toString() + originalFilename.substring(originalFilename.lastIndexOf("."));  
        String url = AliOssUtil.uploadFile(folderName,filename,file.getInputStream());  
  
        return Result.success(url);  
    }  
}
```

提供服务接口：/upload，参数为`form-data`格式的`file`类型文件。
