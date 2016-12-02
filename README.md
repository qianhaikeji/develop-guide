# develop-guide
本文档规定项目开发流程规范，以及各种技术栈所使用的seed工程。

## 目录

[TOC]

## 开发工具

接口管理：[postman](https://www.getpostman.com/)

在线文档：[石墨](https://shimo.im)

项目管理：[trello](https://trello.com)

markdown编辑器：[typora](http://www.typora.io/)

代码仓库：git(coding, github)



## 项目开发流程

1. 产品经理使用axure绘制产品原型，并打包成html发布到公网中。

2. ui设计师进行产品设计。

3. 产品经理/前端工程师梳理前端场景（根据具体项目情况），编写到**石墨**文档中，描述与后端接口所需的场景，不要定义url和数据结构，这些由后端工程师定义。

4. 后端工程师根据前端工程师的描述文档，使用postman定义接口，包括url和数据结构，并导出为json文件。

5. 前端工程师根据postman中的定义进行开发。

6. 前后端接口联调前，后端工程师必须先完成接口自验。

7. 联调过程中遇到的bug记录到trello的bug list中。

8. 代码及时上传到git仓库中。

   ​

## 项目开发规范

> 在没有特殊需求的情况下，一律使用前后端分离的方式进行项目开发，统一使用restful接口。

### 1.技术栈选择

#### 前端框架

| 框架           | 使用场景                                     |
| ------------ | ---------------------------------------- |
| vue2.0       | 优先使用vue开发,不支持IE8及以下版本                                |
| angular1.0   | 1. 所用后台管理项目<br>2.如果项目无SEO要求，优先使用angular进行开发。 |
| react        | 有SEO需求的项目，使用react进行开发。                   |
| jquery/zepto | html5中的小项目可以使用后端渲染的方式进行开发。               |

#### 后端框架

| 框架               | 使用场景                                     |
| ---------------- | ---------------------------------------- |
| express(nodejs)  | server业务简单，偏重于io/转发类型的业务，可以使用Nosql，无事务要求的业务，使用nodejs来开发。 |
| flask(python)    | server业务较复杂，需要使用关系型数据库，需要保证事务一致性，使用python开发。 |
| springboot(Java) | 在上述两种框架不可用的情况下，选择java来开发。                |

#### 服务器

leancloud：对服务器、域名、数据库无特殊要求，配合nodejs使用。

aliyun：在leancloud方案不适用的情况下使用。

#### 数据库

mongo：无事务要求的场景下，或者快速实现产品原型，使用mongo。

mysql：关系型数据库优先使用。

#### 缓存

redis

### 2.编码规范

参考如下规范

[前端编码规范](https://github.com/mdo/code-guide)



## seed工程

[angular](https://github.com/qianhaikeji/angular-seed.git)

[leancloud nodejs angular](https://github.com/qianhaikeji/leancloud-nodejs-angular-seed.git)

[react express](https://github.com/qianhaikeji/react-express-webpack.git)

[python flask]()

[Java springboot base on mongo](https://github.com/qianhaikeji/spring-boot-mongo-seed.git)

[Java springboot base on mysql](https://github.com/qianhaikeji/spring-boot-mysql-seed.git)

