2017-10-03

### Spring Cloud OAuth2 演示
#### 演示应用列表
    oauth2-server : oauth2 Oauth 2.0 Provider -- 认证服务器 ，进行认证和授权, 端口 9999
    oauth2-resource : Oauth 2.0 Provider -- 资源服务器，受 oauth2 认证服务保护的资源或者服务 ,端口 9000
    oauth2-client : 试图访问受 oauth2 认证服务保护的资源的WEB应用 ， 端口 8080
    
#### 演示参数
    1. 模拟用户账号 : user/password , 硬编码该账号密码
    2. 模拟客户端账号 : acme/acmesecret , 配置文件中提供该账号密码
        
#### 演示
    1. 启动 oauth2-server,oauth2-resource,oauth2-client ， 启动顺序没有要求;
    2. 访问 http://localhost:8080
       看到页面 index.html页面 Tab home.html 内容，当前处于未登录状态，欢迎数据提示请登录。
    3. 点击 login 链接
        界面弹出oauth2 验证界面，要求输入用户名密码，当前浏览器显示URL如下 ：
        http://localhost:9999/uaa/oauth/authorize?client_id=acme&redirect_uri=http://localhost:8080/login&response_type=code&state=sSHDaU
    4. 输入用户名密码 : user/password 并确定
        界面跳转到页面 index.html页面 Tab home.html 内容，当前处于登录状态，欢迎数据显示 ID 和 content 来自服务器的内容。
    5. 点击 logout 退出登录
        看到页面 index.html页面 Tab home.html 内容，当前处于未登录状态，欢迎数据提示请登录。
                

#### 原理分析
    bash 中 ：
    curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=password&username=user&password=password&redirect_uri=http://localhost:9000" "http://acme:acmesecret@localhost:9999/uaa/oauth/token"
    TOKEN=c94c84b5-7816-4a51-a2c3-e1cb2dbb8fef
    curl -H “Authorization: Bearer $TOKEN” http://localhost:9000
