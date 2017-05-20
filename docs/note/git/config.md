## git config 配置项

#### core.autocrlf

是否自动将 LF 转换为 CRLF，当 git 提示 `warning: LF will be replaced by CRLF` 时，可以将该选项设为 `false` 关闭提示。

```sh
git config core.autocrlf false
```

#### core.filemode

是否忽略文件权限

```sh
# 忽略文件全向
git config core.filemode false
```

#### credential.helper

认证助手，当我们使用HTTPS方式clone项目时，每次 pull、push 都会提示输入密码，为了不每次都输入密码，就需要配置 `credential.helper` 选项。

```sh
# cache 将凭据存储在内存中
git config credential.helper cache
# store 将凭据保存在磁盘上
git config credential.helper store
```

操作步骤如下：

###### 在家目录创建并编辑 .git-credentials 文件

```sh
cd ~
touch .git-credentials
vim .git-credentials
```

###### 把如下内容写入.git-credentials文件中，保存并退出

```
# {username} 你的git账户名
# {password} 你的git密码
# example.com 你的git仓库域名
# 例子：https://hcysunyang:12345678@github.com

https://{username}:{password}@example.com
```

###### 设置凭据存储方式

```sh 
git config credential.helper store
```

#### user.name

配置git用户名

#### user.email

配置用户邮箱