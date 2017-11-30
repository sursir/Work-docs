安装时遇到的部分问题
================================


1. 需要提前设置好数据库配置，创建数据库 **`config/web.php, config/local.php`**

        // 如果需要新创建数据库用户
        create user zhangsan identified by 'zhangsan';
        grant all privileges on zhangsanDb.* to zhangsan@'%' identified by 'zhangsan';
        flush privileges;

2. 需要提前配置好邮件发信人，可以使用qq发信，不必自己配置 **`config/local.php`**

        host:                smtp.qq.com
        username:            *********@qq.com
        password:            ****************
        port:                20
        encryption:          tls

        from:                *********@qq.com

3. 其实也可以放在 `.env` 文件中，`yii` 框架配置

4. 如此之后 `./yii walle/setup` 即可安装

5. 安装之后需要创建管理员用户，接收到邮件确认激活，假如没收到邮件请检查发信设置。
   成功之后在数据库中将字段 `role, status` 统一设置为 2，即为管理员。删除不必要的初始用户。

6. 前期完成，准备设置项目，待续……

