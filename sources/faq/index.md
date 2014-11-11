## 概念性 FAQ

**空间分几种类型，有哪些区别？**

四种，图片空间、文件空间、静态 CDN 空间、动态 CDN 空间。


| 功能 | 文件类空间 | 图片类空间 | 静态CDN类空间 | 动态 CDN 空间 |
|---|---|---|---|
| CDN加速 | 支持 | 支持 | 支持 | 支持 |
| 可上传文件 | 所有文件 | 仅支持图片类文件，后缀jpg/png/gif/bmp/tif/webp | 不需要上传，UPYUN 直接从客户源站获取 | 不需要上传，UPYUN 直接从客户源站获取 |
| 单个文件上传限制 | 单个文件最大支持1GB | 单个文件最大支持20M | - | - |
| 图片文件像素限制 | 无限制 | 宽X高X帧数 < 200000000像素 | 无限制 | 无限制 |
| 防盗链功能 | 支持 | 支持 | 支持 | 不支持
| 绑定独立域名 | 支持 | 支持 | 支持 | 支持 |
| 自定义缩略图 | 不支持 | 支持| 支持 | 不支持

**空间类型是否可以更改？**

空间类型一旦创建，将无法更改。

**一个账号最多可以创建多少个空间？**

一个账号最多可创建200个空间。

**一个空间最多可以绑定多少个域名？**

每个空间最多绑定5个域名

1. 绑定的域名必须经过备案；
2. 只能绑定非 www 开头的二级或多级域名（动态 CDN 支持 www 开头二级域名）；
3. 同一个域名只能绑在一个空间上。

**如何查看我空间里的文件？**

可以使用 FTP 查看，推荐使用客户端：FileZilla / FtpRush。

**一个账号最多可以创建多少个操作员？**

UPYUN 可创建50个操作员，以方便分配给不同部门或项目使用，并且可以灵活交叉授权。

**又拍云是否支持 ？v=xxx 来刷新缓存？**

不支持， 对于 ?v=xxx 参数是忽略的。

**你们支持哪几种防盗链，有什么区别？**

UPYUN 支持 IP 禁用，域名防盗链，客户端白名单，Token 防盗链，并且可以自定义防盗链提示图。

- **IP 禁用**： 如果不希望某个 IP 或者某段 IP 访问空间，可以使用此功能，最多支持50个 IP 或 IP 段；
- **域名防盗链**： 通过设置域名白名单或黑名单，允许或禁止某个网站引用空间文件，最多各支持50个黑白名单；
- **客户端白名单**： 对于客户端或手机 APP 的客户，可以通过设置 User Agent 白名单来限制访问；
- **Token 防盗链**： 适合下载类网站或应用，可以设置签名和过期时间来控制空间文件的访问时限。


**CDN 缓存时间是多久？**

* 若客户有自定义缓存过期时间，那么始终以用户设置的为准；
* 若客户通过其源站带上来的相应头中 Expires 字段减去当前服务器时间大于7天，则按照用户的为准；
* 其他情况，均统一默认为7天+24小时随机。

**如何刷新缓存？**

CDN 空间缓存刷新有两种：

1. API 接口，调用缓存刷新 API 接口，0~10分钟更新到全国节点；
2. 全网空间刷新，登陆 UPYUN ，选择空间>通用>高级功能，全网缓存刷新会对源站产生一定的压力，操作请慎重。

**又拍云支持 HTTPS 吗？**
支持，暂只支持默认域名，不支持**绑定域名** ，每个用户可免费开通一个，比如：https://bucket.b0.upaiyun.com/pic.jpg。

## 技术性 FAQ

### HTTP REST API

**使用标准 API 上传，提示：400 Bad Request？**

错误请求。

可能情况：

1. 接口请求 URL 中缺少空间名；
2. 发起的接口操作数据传输到又拍云服务器发生错误。

**使用标准 API 上传，提示：401 Unauthorized？**

访问未授权。

可能情况：

1. 操作员或操作员密码错误；
2. 空间名错误；
3. 其他错误（可从接口返回的 BODY 中获取详细信息）。

**使用标准 API 上传，提示：401 Sign error？**

签名错误。

可能情况：

1. 签名格式错误；
2. 服务器时间错误，请检查服务器的时间是否与世界时间有较大偏差。

**使用标准 API 上传，提示：401 Need Date Header？**

发起的请求缺少 Date 头信息。

可能情况：

发起的接口请求 HEADER 中确少 Date 参数；

**注意：**部分 HTTP Client 开发包在发起 GET 请求时不自动加入 Date 参数。

**使用标准 API 上传，提示：401 Date offset error？**

发起请求的服务器时间错误。

可能情况：

服务器时间错误，请检查服务器的时间是否与世界时间有较大偏差。

**使用标准 API 上传，提示：403 Not Access？**

权限错误。

可能情况：

1. 非有效的图片文件上传到图片空间；
2. 图片文件超过又拍云系统最大限制（2亿个像素）；
3. 图片文件容量超过又拍云系统最大限制（20Mb）；
4. 空间配额已满（请到管理后台进行设置，或购买更多存储配额）；
5. 空间被禁用；
6. 空间被禁止新增文件；
7. 上传图片过程中，缺少缩略图参数。

**使用标准 API 上传，提示：403 File size too max？**

文件容量过大。

可能情况：

上传的文件容量超过又拍云系统限制（单文件1Gb）。

**使用标准 API 上传，提示：403 Not a Picture File？**

不是正确的图片内容。

可能情况：

上传的图片文件内容错误，又拍云系统无法正确识别处理（图片空间只允许上传 jpg、png、gif、bmp、tif、webp 格式）。

**使用标准 API 上传，提示：403 Picture Size too max？**

图片尺寸过大。

可能情况：

1. 图片文件超过又拍云系统最大限制（2亿个像素）；
2. 图片文件容量超过又拍云系统最大限制（20Mb）。

**使用标准 API 上传，提示：403 Bucket full？**

空间配额已满。

可能情况：

空间配额已满（请到管理后台进行设置，或购买更多存储配额）。

**使用标准 API 上传，提示：403 Bucket blocked？**

空间被禁用。

**使用标准 API 上传，提示：403 User blocked？**

操作员被禁用。

**使用标准 API 上传，提示：403 Image Rotate Invalid Parameters？**

图片旋转参数错误。

**使用标准 API 上传，提示：403 Image Crop Invalid Parameters？**

图片裁剪参数错误。

**使用标准 API，提示：404 Not Found？**

请求的文件不存在。

可能情况：

1. 在上传文件过程中遇到 404 错误，请检查上传文件路径的目录路径是否存在；
2. 在下载文件过程中遇到 404 错误，请检查文件名和路径是否存在。

**使用标准 API，提示：406 Not Acceptable？**

不接受请求。

可能情况：

1. 在上传文件过程中遇到 406 错误，请检查上传文件名是否已经被用于存在的目录；
2. 在上传文件过程中遇到 406 错误，如有设置 Content-MD5 校验，可能是又拍云服务器端收到的文件内容 MD5 不一致。请检查 API 接口请求时设置的 Content-MD5 值；
3. 在创建目录过程中遇到 406 错误，请检查目录名是否已经被用于存在的文件。

**使用标准 API 上传，提示：503 System Error？**

系统错误。

可能情况：

1. 又拍云系统正在维护中；
2. 又拍云系统出现故障，请与客服联系。

### FORM API

**使用表单 API 上传，提示：400 Not accept, Bucket not exists？**

不接受请求，空间不存在。

可能情况：

空间名填写错误。

**使用表单 API 上传，提示：403 Not accept, Miss signature？**

不接受请求，缺少签名。

可能情况：

递交到又拍云表单 API 接口的 POST 表单信息中缺少签名字段。

**使用表单 API 上传，提示：403 Not accept, Signature error？**

签名错误。

可能情况：

1. 递交到又拍云表单 API 接口的 policy 信息中缺少空间名；
2. 递交到又拍云表单 API 接口的 policy 信息中缺少 save-key 字段；
3. 递交到又拍云表单 API 接口的 policy 信息中缺少 expiration 字段；
4. 表单签名密匙错误（请到空间管理后台获取表单 API 签名密匙）。

**使用表单 API 上传，提示：403 Not accept, POST URI error？**

不接受请求，递交的 URL 地址错误。

可能情况：

1. 递交到又拍云表单 API 接口的 POST 表单地址错误；
2. URL 中的空间名与表单 POST 的空间名不一致。

**使用表单 API 上传，提示：403 Not accept, Authorize has expired？**

不接受请求，授权已过期。

可能情况：

1. 在 policy 信息中设置 expiration 授权过期时间过短；
2. 服务器时间与世界标准时间偏差过大。

**使用表单 API 上传，提示：403 Not accept, Bucket disabled？**

不接受请求，空间已被禁用。

**使用表单 API 上传，提示：403 Not accept, Form API disabled？**

不接受请求，空间的表单 API 上传功能未开启。

**使用表单 API 上传，提示：403 Not accept, No file data？**

不接受请求，没有上传文件数据。

可能情况：

1. 表单信息中的文件域名称不是 file；
2. 没有选择上传的文件，就递交表单请求。

**使用表单 API 上传，提示：403 Not accept, File too small？**

不接受请求，上传的文件容量过小。

可能情况：

在 policy 信息中设置的 content-length-range 参数是否过大。

**使用表单 API 上传，提示：403 Not accept, File type Error？**

不接受请求，上传的文件类型不被允许。

可能情况：

在 policy 信息中设置的 allow-file-type 不包含当前上传的文件类型。

**使用表单 API 上传，提示：403 Not accept, Content-md5 Error？**

不接受请求，上传的文件 md5 校验错误。

可能情况：

在 policy 信息中设置的 content-md5 校验值与表单接收到的文件 md5 不一致。

**使用表单 API 上传，提示：403 Not accept, Not a image file？**

不接受请求，上传的不是一个有效图片文件。

可能情况：

1. 如上传到图片空间，仅允许上传有效的图片文件；
2. 如在 policy 信息中设置图片宽高限制，仅允许上传有效的图片文件。

**使用表单 API 上传，提示：403 Not accept, Image width too small？**

不接受请求，上传的图片文件宽度过小。

可能情况：

在 policy 信息中设置的 image-width-range 参数是否过大。

**使用表单 API 上传，提示：403 Not accept, Image width too large？**

不接受请求，上传的图片文件宽度过大。

可能情况：

在 policy 信息中设置的 image-width-range 参数是否过小。

**使用表单 API 上传，提示：403 Not accept, Image height too small？**

不接受请求，上传的图片文件高度过小。

可能情况：

在 policy 信息中设置的 image-height-range 参数是否过大。

**使用表单 API 上传，提示：403 Not accept, Image height too large？**

不接受请求，上传的图片文件高度过大。

可能情况：

在 policy 信息中设置的 image-height-range 参数是否过小。

**使用表单 API 上传，提示：403 Not accept, Data too long for ext-param？**

额外参数内容过长。

**使用表单 API 上传，提示：403 Image Rotate Invalid Parameters？**

图片旋转参数错误。

**使用表单 API 上传，提示：403 Image Crop Invalid Parameters？**

图片裁剪参数错误。

**使用表单 API 上传，提示：403 not access？**

可能情况：

表单 enctype 参数使用错误，应该为 enctype="multipart/form-data"。

**使用表单 API 上传，提示："code":-403,"message":"403 Not Access？"**

检查 save-key 格式是否正确。

**使用表单 API 上传，提示：503 System Error ... Retry again？**

系统错误。

可能情况：

1. 又拍云系统正在维护中；
2. 又拍云系统出现故障，请与客服联系。

## FTP

**使用 FTP 登录提示：无法解析 v0.ftp.upyun.com 的地址？**

可能情况：

本地 DNS 有问题，可尝试更换公共 DNS。

**使用 FTP 登录提示：530 Authentication failed. Disconnecting. 无法连接到服务器？**

可能情况：

1. 用户名（操作员/空间名，斜杠为必须）错误；
2. 密码（操作员密码）错误；
3. 操作员没有授权给空间。

**使用 FTP 上传提示：550 Usopp: UpYunFS - 403 Not Access？**

可能情况：

1. 非图片上传到图片空间
2. 不是有效的图片（通常因文件编码问题造成）；
2. 空间配额已满。

**FTP 看到的时间与当前时间（北京时间）不一致？**

又拍云服务器采用 GMT 时间，为 UTC+0 区，和北京时间相差8个小时。

**FTP 看到上传的中文文件名为乱码？**

又拍云的 FTP 编码为 UTF-8，请更改 FTP 编码设置为 UTF-8。

**为什么文件通过外链可以访问，但在 FTP 中看不到？**

又拍云限制单个目录下的文件及目录数量共1万条，如果超过隐藏显示，不影响外链及上传。

**FTP 中可以看到图片，但外链访问 404？**

可能情况有：

1. 使用了原图保护功能，需要带密钥访问；
2. 上传完成前有过访问， 会有5分钟404缓存，需要刷新缓存或等待缓存过期后更新；
3. URL 中含有和后台间隔符相同的字符。

**隐藏的文件 FTP 看不到怎么下载？**

通过外链或者使用 API GET 方法下载。

**为什么 FTP 不能给目录重命名？**

重命名仅支持空目录及文件。

**FTP 如何移动及复制文件及目录？**

暂不支持目录及文件移动和复制功能

**为什么 FTP 上传会少几个字节不完整？**

由于 FTP 的特性，不检查数据完整性，如网络波动导致一部分字节丢失也是可能的，可重传或采用 API 方式上传。

**FTP 提示：425 Rejected Data Connection From Foreign Address 并读取目录失败？**

可能情况：

运营商提供的 IP 发生了变化，FTP 探测到前后不一致，出于安全考虑，断掉了连接，这个是 FTP 协议决定的。如果无法改变这个 IP 地址变动的情况, 可以尝试使用使用代理来避免这种情况发生。

### 插件

**织梦 CMS 是否可以使用又拍云，有没有插件？**

暂无织梦 CMS 的插件；理论上支持 FTP 协议的 CMS 都是可以使用又拍云存储，但织梦 CMS 本身将远程附件当做备份处理，静态文件的引用是不引用远程附件的；所以织梦 CMS 可以使用又拍，但只能是把又拍当备份。

**Discuz 使用插件，不生效？**

插件当严格按照文档进行安装。

可能情况：

1. 插件版本不正确；
2. 相关文件权限没有修改成666；
3. 提交数据后，没有先进前台论坛页面一次，再去更新缓存；
4. 插件参数设置错误，比如：文件空间使用了图片空间、访问域名结尾加了斜杠或空格；
5. 装插件之前使用了论坛自带远程附件或修改了远程附件参数。

默认参数设置：

1. “启用SSL连接”应该选择“否”；
2. “FTP服务器端口”应该填“21”；
3. “被动模式（pasv）连接”应该选“是”；
4. “远程附件目录”应该填“.”；
5. “FTP 传输超时时间”应该填“0”；
6. “允许的附件扩展名”和“禁止的附件扩展名”都应该为空；
7. “附件尺寸下限”应该为空；
8. “隐藏远程附件真实路径”应该选“否”。

**Discuz 是否可以本地保留附件？**

如果你使用插件，插件最后是有选项设置是否开启本地备份儿功能的；如果你使用远程附件功能，本地是不保存的。

**Discuz 上传大文件时经常失败？**

原因可能更服务器带宽有关，由于上行带宽小，上传大文件时达到超时时间，文件还未完成上传就会自动断开，可以将 source/plugin/upyun_upload/sdk/upyun.class.php 中 curl_setopt($ch, CURLOPT_TIMEOUT, 30)  超时时间改大一些。

**为什么 Discuz 远程附件的测试 FTP 按钮提示删除失败，没有权限的提示？**

该功能与又拍不兼容，确认填写配置信息无误后，可以直接保存设置，发帖测试。

**为什么用 FTP 上传的文件，WordPress 的媒体库看不到？**

因为媒体库只控制由自身上传的文件，并没有获取远程目录的功能，所以 WordPress 的媒体库看不到 FTP 上传的文件

**怎么将 Wordpress 的数据迁移到又拍？**

将网站根目录下 /wp-content/uploads 下的数据上传到又拍，安装又拍 Wordpress 的插件（安装请参考安装手册），正确填写设置，保存配置，更新数据库。


**为什么安装 Wordpress 后，上传一个文件，又拍存储空间会多几个？**

这是由于 Wordpress 的存储方式是一张原图直接分割成多张缩略存储，上传附件时是将原图以及缩略图一并上传的。

**PHPWind 安装又拍插件，头像还是保存本地？**

PHPWind 头像存储还需要另外安装插件设置；具体参考安装手册第四项，头像存储方式设置。

**为什么 PHPWind 安装插件后，上传文件后，文件外链还是本地地址？**

检查配置信息是否正确，清理 PHPWind 缓存。

**为什么下载又拍官方的 UEditor 编辑器，配置以后，上传不了？**

检查 UEditor 的权限问题，UEditor 下附件目录 uploads 至少要有写的权限。

**UEditor 的编辑器，怎么让引用文件显示又拍的 URL 改成我自己的？**

首先你要在又拍空间下绑定一个你自己的域名，（具体参考又拍后台绑定域名设置指引）然后打开编辑器目录下 UEditor.config.js 找到 imagepath , filepath , catcherPath 修改成你的绑定域名即可。

### 其它

**图片访问显示：403 Forbidden？**

开启了防盗链功能。

可能情况：

1. 使用了 Token 防盗链，访问时没有加参数：_upt；
2. 使用了 IP 禁用功能，IP 被禁了；
3. 使用了域名防盗链功能，不在允许的域名范围内；
4. 使用了客户端白名单功能，不在白名单范围内。

**图片在本地看着是正的，传到了又拍云，某些浏览器访问是倒的？**

图片本身并不是正的，部分软件或浏览器具有自动旋转识别功能，可通过查看图片 exif 信息，确定图片的具体旋转情况。

**说明：**参数 Orientation，（1：0度 6：顺时针90度 8：逆时针90度 3：180度）。

**原图加密功能不生效？**

可能情况：

1. 空间是文件类空间：文件类空间没有原图保护的功能；
2. 图片类空间不存在“缩略图版本号”：不存在缩略图版本号，就不存在“原图保护”的概念；
3. 原图保护密钥包含特殊字符：新版本规定只能使用“a-zA-Z0-9”的字符组合。

**添加水印不成功？**

检查水印图片尺寸任意边是否超过了生成图片尺寸的50%，超过不能添加水印。

**文件更新了，为什么访问还是老图？**

文件更新后，又拍云存储需要更新全国节点的缓存，这个时间需要0~10分钟。一般情况下，能够做到实时更新。10分钟后还是无法获取新图，请检查以下两种情况：

1. 浏览器可能存在缓存，请清除浏览器缓存后，再强制刷新;
2. 若是CDN空间的情况，源数据更新后是否有调用过API接口，通知又拍云存储进行缓存刷新。