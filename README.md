# NWAFU-StayOnline
## 西北农林科技大学校园网新版深澜认证自动登录与保持，使用GO语言单文件版，避免路由器安装python环境。

[![Lisense](https://img.shields.io/github/license/Mmx233/BitSrunLoginGo)](https://github.com/Mmx233/BitSrunLoginGo/blob/main/LICENSE)
[![Release](https://img.shields.io/github/v/release/Mmx233/BitSrunLoginGo?color=blueviolet&include_prereleases)](https://github.com/Mmx233/BitSrunLoginGo/releases)
## 本项目仅对配置文件做了补充修改，经测试可用于西北农林科技大学深澜校园网网页认证。包括Windows、Linux（路由器padavan，OpenWrt系统同样适用）
编译详情请查看 https://github.com/Mmx233/BitSrunLoginGo 的项目，本项目只介绍使用方法

## 使用方法
![演示](https://github.com/littleOwl233/NWAFU-StayOnline/blob/main/%E6%BC%94%E7%A4%BA%E6%88%AA%E5%9B%BE.png)
1. 根据设备架构下载编译后的可执行文件，解压后获得名为`autoLogin`的文件
   > [点这里跳转到原项目的可执行文件下载页面](https://github.com/Mmx233/BitSrunLoginGo/releases)
   
   > [速度过慢可使用国内分流阿里云盘](https://www.aliyundrive.com/s/2Ug5BP1w9aX)
   
   > 由于阿里云盘不支持分享包括zip,rar在内的各种压缩包，因此使用7-zip打包为了.exe，若打不开请安装7-zip
2. 在`autoLogin`同级文件下创建`Config.yaml`文件
    > 可以通过添加启动参数`--config`指定配置文件路径，默认为当前目录的`Config.yaml`。
3. 修改`Config.yaml`的内容如下（复制后更改其中的 `学号` 和 `密码` 即可）：
    ```yaml
    form:
        # 登录域名或IP（本人测试使用IP登录失败，建议用域名）
        domain: portal.nwafu.edu.cn
        # 帐号
        username: "学号"
        # 运营商类型，西农此值为空
        usertype:
        # 密码
        password: "密码"
    meta:
        # 一些登录参数
        "n": "200"
        type: "1"
        acid: "1"
        enc: srun_bx1
    settings:
        # 基础设置
        basic:
            # 访问校园网API时直接使用https URL，西农此值为true
            https: true
            # 是否忽略证书验证
            skip_cert_verify: false
            # 网络请求超时时间（秒）
            timeout: 5
            # 网卡名称正则（注意JSON转义），如：eth0\\.[2-3]，不为空时为多网卡模式
            # 本人实际使用时，以eth0 eth1作为参数没能成功，最终没实现多网卡同时登录
            interfaces: ""
        # 守护模式（我猜测是防掉线？还没尝试）
        guardian:
            # true为启用
            enable: false
            # 网络检查周期（秒）
            duration: 300
        # 后台模式（不建议windows使用）
        # 不知道这个daemon和Linux中的daemon（守护进程）是不是一个意思，如果仅仅是需要后台运行的话，在运行命令前加上nohup同样可以实现
        daemon:
            enable: false
            # 守护监听文件路径，确保只有单守护运行
            path: .autoLogin
        debug:
            # 开启debug模式，报错将更加详细
            enable: false
            # 写日志文件
            write_log: false
            # 日志文件存放路径
            log_path: ./
    ```
4. 运行`autoLogin`程序
   > Windows平台下可直接双击，或在当前目录打开cmd，输入```autoLogin```运行

   > Linux下，先使用```chmod a+x autoLogin```赋予可执行权限，再执行```./autoLogin```

至此应该认证完成，打开 https://portal.nwafu.edu.cn/ 应该会显示已登录页面

## 附1 使用`--config`指定配置文件
首次运行将自动生成`Config.yaml`配置文件（注意此处文件名中的C是大写，--config中是小写）。可使用--config添加参数，指定配置文件。
```shell
./autoLogin --config=/pathA/Config1.yaml
./autoLogin --config=/pathB/Config2.yaml
```
## 附2 无注释版简洁版`Config.yaml`配置文件
    ```yaml
    form:
        domain: portal.nwafu.edu.cn
        username: "学号"
        usertype:
        password: "密码"
    meta:
        "n": "200"
        type: "1"
        acid: "1"
        enc: srun_bx1
    settings:
        basic:
            https: true
            timeout: 5
            interfaces: ""
        guardian:
            enable: false
            duration: 300
        daemon:
            enable: false
            path: .autoLogin
        debug:
            enable: false
            write_log: false
            log_path: ./
    ```
