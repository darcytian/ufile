{{indexmenu_n>1}}

# 存储空间

### 创建存储空间

在上传文件（Object）到 UFile 之前，您需要使用 UFile API 中的 CreateBucket 接口来创建一个用于存储文件的存储空间（Bucket），存储空间具有各种配置属性，包括地域、访问权限以及其他元数据。或者使用[UFile控制台](https://console.ucloud.cn/ufile/ufile)来创建一个存储空间（Bucket），并设置存储空间的访问权限。

> 说明：创建存储空间的 API 接口的详细信息请参见 [CreateBucket](https://help.aliyun.com/document_detail/31959.html#reference-wdh-fj5-tdb)。

使用限制

- 同一用户创建的存储空间总数不能超过 20 个。

- 每个存储空间的名字全局唯一，否则会创建失败。

- 存储空间的名称需要符合命名规范。

- 存储空间一旦创建成功，名称和所处地域不能修改。


### 设置空间读写权限

您可以在创建存储空间（Bucket）时设置存储空间的访问权限（ACL），也可以在创建Bucket后根据自己的业务需求修改存储空间的ACL，该操作只有存储空间的拥有者可以执行。

存储空间有两种访问权限：

|权限值    |中文名称         |权限对访问者的限制 |
|--------- |---------------- |------------------------------------------------------------------------------------------------------------------------ |
|public    |公共读，私有写   |只有该存储空间的拥有者可以对该存储空间内的文件进行写操作，任何人（包括匿名访问者）都可以对该存储空间中的文件进行读操作。 |
|           |                  |`警告 互联网上任何用户都可以对该 Bucket 内文件进行访问，这有可能造成您数据的外泄以及费用激增，请谨慎操作。` |
|private   |私有读写         |只有该存储空间的拥有者可以对该存储空间内的文件进行读写操作，其他人无法访问该存储空间内的文件。|


### 获取空间地域信息

您可以通过UFile API的DescribeBucket接口获取存储空间（Bucket）所属的地域，即数据中心的物理位置信息。

> 说明 获取存储空间地域信息的API详情请参考[DescribeBucket](https://docs.ucloud.cn/api/ufile-api/describe_bucket)。

### 绑定自定义域名

您的文件上传到UCloud UFile 的 Bucket 后，会自动生成该文件的访问地址，您可以使用此地址访问 Bucket 内的文件。若您希望通过自定义域名访问这些文件，需要将自定义域名绑定到文件所在的存储空间，并添加 CNAME 记录指向存储空间对应的外网域名。

> 注意 按照中华人民共和国《互联网管理条例》的要求，所有需要绑定自定义域名的用户，必须提前将域名在工信部备案。若您的域名未备案，您可通过UCloud云提供的备案服务进行备案。

|操作方式   |说明 |
|---------- |------------------------ |
|控制台     |Web 应用程序，直观易用 |

绑定自定义域名，您需要了解以下概念：

- 用户域名、自定义域名、自有域名：您在域名服务商处购买的域名。

- UFile 访问域名或 Bucket 域名：UFile 为您的 Bucket 分配的的访问域名。您可以使用此域名访问您 Bucket 里的资源。如果您想使用您自己的用户域名访问 UFile Bucket，必须将您的用户域名绑定到 UFile 域名，即在云解析中添加CNAME记录。

- UCloud CDN 域名：UCloud CDN 为您的用户域名分配的 CDN 加速域名。将您的用户域名绑定到 CDN 加速域名，即在云解析中添加 CNAME 记录，才可以加速访问 UFile Bucket 里的资源。

绑定自定义域名应用场景

例如，用户 A 拥有一个域名为 img.abc.com 的网站，网站的一个图片链接为http://img.abc.com/logo.jpg。为方便后续管理，用户 A 想要访问网站中图片的请求转到 UFile，并且不想修改任何网页的代码，也就是对外链接不变。绑定自定义域名可以满足这个需求。流程如下：

1. 在 UFile 上创建一个名为 abc-img 的 Bucket，并将其网站上的图片上传至 Bucket。

2. 通过 UFile 控制台，将 img.abc.com 这个自定义的域名绑定在 abc-img。

3. 绑定成功之后，UFile 后台会将 img.abc.com 做一个映射到 abc-img。

4. 在自己的域名服务器上，添加一条 CNAME 规则，将 img.abc.com 映射成 abc-img.cn-bj.ufileos.com（即 abc-img 的 UFile 域名）。

5. http://img.abc.com/logo.jpg 请求到达 UFile 后，UFile 通过 img.abc.com 和 abc-img 的映射关系，将访问转到 abc-img 这个 Bucket。也就是说，对http://img.abc.com/logo.jpg的访问，实际上访问的是http://abc-img.cn-bj.ufileos.com/logo.jpg。

绑定自定义域名前后流程对比如下：

|            |绑定自定义域名前                         |绑定自定义域名后 |
| ---------- | ---------------------------------------- | -------------------------------------------- |
|流程对比   |1. 访问 http://img.abc.com/logo.jpg     |1. 访问 http://img.abc.com/logo.jpg |
|            |2. DNS 解析到用户服务器 IP。            |2. DNS 解析到 abc-img.cn-bj.ufileos.com。|
|            |3. 访问用户服务器上的logo.jpg。         |3. 访问 UFile 上 abc-img 里的 logo.jpg。|

### 删除存储空间

您可以通过 UFile API 的 DeleteBucket 接口删除您创建的存储空间。

> 说明 删除存储空间的 API 详细信息可参考[DeleteBucket](https://docs.ucloud.cn/api/ufile-api/delete_bucket)

> 如果存储空间不为空（存储空间中有文件或者是尚未完成的分片上传），则存储空间无法删除，必须删除存储空间中的所有文件和未完成的分片文件后，存储空间才能成功删除。如果想删除存储空间内部所有的文件，推荐使用生命周期管理。
