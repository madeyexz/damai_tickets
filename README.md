# 大麦网抢票脚本

版本迭代说明
- 基于 [AnTi-anti/damai_ticket](https://github.com/AnTi-anti/damai_ticket) 开源版本进行修改
- 基于 [838239178/damai_tickets](https://github.com/838239178/damai_tickets) 开源版本进行修改 
- 基于 [lsbnbdz/damai_tickets](https://github.com/lsbnbdz/damai_tickets) 开源版本进行注释 
  - 以下使用说明中**加粗**部分是 [lsbnbdz](https://github.com/lsbnbdz) 对于[838239178/damai_tickets](https://github.com/838239178/damai_tickets)的改动

> 2023.03.04 修改使抢票脚本对部分演唱会有效
> 
> 2023.03.20 对应页面更新了class_name, 但是出现新机制触发机器验证无法完成下单
>
>
> 2023.03.23 感谢 @wuliang9524 提供的解决方案, 需要修改chromedriver以隐藏爬虫标志
>
> **2023.05.07 大麦新机制是先选场次才会出现票价选项，所以场次和票价不能一同获取（原代码只有一个场次的还是可以正常使用，多场次就不行了），本次修改做了修正**
> 2023.05.09 @madeyexz 增加了 `main_cable.py` 和 `main_wifi.py`。

## 使用说明
1.  安装依赖项
    ```bash
    pip install -r requirements.txt
    ```
2. 修改或安装chromedriver（参见下文）
3. 根据自己需求修改 `config.json` 文件（参见下文）
4. 运行脚本（部分操作需要手动进行）
    ```bash
    python3 main_cable.py
    # 或
    python3 main_wifi.py
    ```
    - `main_cable.py` 和 `main_wifi.py` 区别说明：在实际使用中，刷新时有时候会因为页面加载速度较慢导致无效刷新，因此在网络环境较好的情况下(网线cable)，页面的响应时间设计的更短；反之更长(wifi)。


## 【Windows 用户】 Chromedriver 修改

1. 下载 [chromedriver.exe](https://registry.npmmirror.com/binary.html?path=chromedriver/)
2. 下载Hexedit或者使用Vscode中的Hexedit插件
3. 使用Hexedit打开`chromedriver.exe `
4. 搜索 `$cdc_`
5. 更改为任意其他字符串，如`$cxx_`

## 【MacOS 用户】Chromedriver 安装
1. 首先在chrome里面打开设定（<kbd>command</kbd>+<kbd>,</kbd>），在左下栏里面点击“关于chrome”，查看chrome的版本号。
2. 在[这里](https://chromedriver.chromium.org/downloads)下载对应的（111, 112, 113）Chromedriver，并解压。
3. 解压后的文件夹里面有一个binary的执行文件档案和一个授权文件。
4. 在Finder里面，按<kbd>command</kbd>+<kbd>shift</kbd>+<kbd>G</kbd>，输入`/usr/local/bin`将下载好的Chromedriver移动到这里。
5. 在终端里面输入`sudo chmod +x /usr/local/bin/chromedriver`，输入密码，授权Chromedriver。
6. 在终端里面输入`chromedriver`，如果出现`Starting ChromeDriver...`，则说明安装成功。
7. 在 `config.json` 文件中，修改 `"driver_path": /usr/local/bin/chromedriver`。
8. **注意**：初次运行软件的时候可能会显示运行程序未得到授权，此时需要在`系统偏好设置-安全性与隐私`中点击`允许`。

## 【Windows用户】配置文件

```json
{
    "date": [1],
    "sess": [1],
    "price": [2],	
    "real_name": [1,2,3],
    "nick_name": "",
    "ticket_num": 3,
    "driver_path": "./chromedriver.exe",
    "damai_url": "https://www.damai.cn/",
    "target_url": "https://m.damai.cn/damai/detail/item.html?itemId=704494827883&spm=a2o71.category.itemlist.ditem_3"
}
```

## 【MacOS用户】配置文件
```json
{
    "date": [1],
    "sess": [1],
    "price": [2],	
    "real_name": [1],
    "nick_name": "",
    "ticket_num": 1,
    "driver_path": "/usr/local/bin/chromedriver",
    "damai_url": "https://www.damai.cn/",
    "target_url": "https://m.damai.cn/damai/detail/item.html?itemId=716946530906"
}
```

- `target_url` 抢票的网页，必须是移动端网页，即m.damai.cn域名下的网页。
- `damai_url` 一般不需要更改，用于登录
- `ticket_num` 抢票张数
- `date` 日期 这个版本未测试包含日期筛选的网页，目前大概率不可用
- `sess` 场次，对应买票页面弹框的场次，1代表第一个按钮，数组表示多个可选项
- `price` 价位，对应买票页面弹框的价格，1代表第一个按钮，数组表示多个可选
- `driver_path` 对应当前电脑Chrome浏览器版本的驱动文件
- **`real_name` 实名信息可以指定序号（提前看好要选择的人的序号，大麦网不会改变顺序的，记好了填上就行）,用英文逗号`,`隔开，有几个人就写几个数字**
- `nick_name` 没什么用

## 注意事项

1. 账号必须先做好实名制认证，并添加至少一个实名制的人的信息
2. 第一次打开后会进入登录页面，需要手动选择扫码登陆
3. 如果太久没用，需要先清空目录下的 cookie 文件，然后在重新登录
4. 大麦网每日最多成功下单5次，测试的时候注意以免当天被禁止下单
