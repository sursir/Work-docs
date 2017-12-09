生成Changelog
==============================


在项目根目录下应该有一个叫做 **`CHANGELOG.md`** 文件，当你处理一个`issue`并在处理完成提交审核前都应该添加一个新条目以便能够说明并文档化你的工作。

这对于 **开发** **测试** 以及 **发布** 都非常重要，同时也能看到在项目每个发布（或者版本）之间有哪些变化。

你应该添加以下内容以说明你这些提交的工作内容，例如：
	
	- 修复bug
	- 新特性
	- 功能改进
	- 以及任何其他发布说明相关信息


**changelog** 每一个条目应该包含日期，版本号和一些关于发布的说明。条目应该以按时间顺序反向罗列。

## 下面是基本模版：

### 格式：

```
#### <功能名称> <版本号:x.y.z>
##### <你的名字> | <<mailto:你的邮件>> | <日期> 

* **Features**:
	- <issue number>: <issue title> (<issue link>)
	- ...

* **Bugs fixed**: 
	- <issue number>: <issue title> (<issue link>)
	- ...

* **Enhancements**: 
	- <issue number>: <issue title> (<issue link>)
	- ...

* **Other**:
	- <issue number>: <issue title> (<issue link>)
	- ...

## NEXT ITEM
```


### 代码示例：

```
#### 打折促销 1.2.0
杨旭 | <mailto:yangxu@pixara.cn> | 2017-12-10 04:28 

* **Features**:
	- \#12: 增加批量设置（打折/减钱） (<http://gitlab.huoniu.com/huoniu/huoniu/issue/12>)

* **Bugs fixed**: 
	- \#13: 减钱可能出现的精度问题 (<http://gitlab.huoniu.com/huoniu/huoniu/issue/13>)

```

### 效果：

------------------------------------------

#### 打折促销 1.2.0
杨旭 | <mailto:yangxu@pixara.cn> | 2017-12-10 04:28 

* **Features**:
	- \#12: 增加批量设置（打折/减钱） (<http://gitlab.huoniu.com/huoniu/huoniu/issue/12>)

* **Bugs fixed**: 
	- \#13: 减钱可能出现的精度问题 (<http://gitlab.huoniu.com/huoniu/huoniu/issue/13>)

------------------------------------------




