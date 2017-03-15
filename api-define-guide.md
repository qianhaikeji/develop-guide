# 项目接口定义规范(Java版)

#### 需要解决的问题

1. 项目迭代时间长之后，代码与接口文档不一致
2. postman定义的接口，多人修改时版本难以管理，容易冲突
3. postman定义的接口，参数定义不直观
4. mock数据不方便



因此，使用swagger2提供的swagger core功能，通过代码注解的方式自动生成api文档。后端工程师在编写代码的同时即可完成接口文档的编写。



## 1.项目开发的流程

1.后端工程师定义model，并用swagger annonation注解

2.后端工程师定义restful接口，不需实现，并用swagger annonation注解

3.后端工程师导出swagger接口文件分发给前端工程师，作为接口文档

4.前端工程师根据接口文档行开发，可以将接口文档导入到swagger工具(swagger editor / swagger hub)中。

5.前端工程师可以通过swagger codegen（集成在swagger editor中）生成server stubs来测试前端，也可以使用swagger hub集成的virtserver（收费）来在线测试接口。

6.如果开发过程中接口变更，后端工程师重新导出接口文档，并分发给前端工程师使用。



## 2.集成swagger2(Java版)

java有两种方案集成swagger2

1.java springmvc + springfox

2.jersey + swagger-jersey2-jaxrs

由于我们使用spring boot + jersey 构建restful风格的server，所以我们使用第二种方式来集成swagger。



#### 2.1 把swagger集成到jersey中

1.在build.gradle中添加依赖，并refresh cradle

```groovy
	// swagger api doc
    compile (
        'io.swagger:swagger-jersey2-jaxrs:1.5.10'
    )
```

2.配置swagger

```java
文件：JerseyConfig.java
@Component
@ApplicationPath("/api")
public class JerseyConfig extends ResourceConfig {

  ...
    
  @PostConstruct
  public void init() {
    this.register(ApiListingResource.class);
    this.register(SwaggerSerializers.class);

    BeanConfig config = new BeanConfig();
    config.setTitle("zyzb");
    config.setVersion("1.0.0");
    config.setContact("denghan");
    config.setSchemes(new String[] { "http", "https" });
    config.setBasePath("/api");
    config.setResourcePackage("com.qhkj.zyzb.api");
    config.setPrettyPrint(true);
    config.setScan(true);
  }
}
```

3.在接口文件中加入注解

```java
@Component
@Path("/admin")
@Api(value = "UserController", description = "用户相关api") //将api加入文档
public class AdminRest extends BaseRest {
...

	@GET
    @Path("/users")
    @ApiOperation(value="获取用户列表", notes="") //定义api
    @Produces(MediaType.APPLICATION_JSON)
    public Response getUserList(@BeanParam BaseQueryParams bps) {
        Page<Admin> list = adminService.getUserList(bps);
        System.out.println(SecurityContextHolder.getContext().getAuthentication()
                .getPrincipal());
        return Response.ok(list).build();
    }
}
```

4.启动server，访问localhost:8080/api/swagger.json，可得到swagger格式的接口文档

```json
{
  "swagger" : "2.0",
  "info" : {
    "version" : "1.0.0",
    "title" : "zyzb",
    "contact" : {
      "name" : "denghan"
    }
  },
  "basePath" : "/api",
  "tags" : [ {
    "name" : "UserController"
  } ],
  "schemes" : [ "http", "https" ],
  "paths" : {
    "/admin/perms" : {
      "get" : {
        "tags" : [ "UserController" ],
        "operationId" : "getPermList",
        "produces" : [ "application/json" ],
        "parameters" : [ {
          "name" : "fuzzy",
          "in" : "query",
          "required" : false,
          "type" : "string"
        }, {
          "name" : "limit",
          "in" : "query",
          "required" : false,
          "type" : "integer",
          "default" : 10,
          "format" : "int32"
        },
	...
```

5.虽然有了文档，但是不是方便开发人员直观的查看，所以需要使用swagger ui来展示这些数据。

有三种方式：

1）下载swagger ui并在本地部署，将swagger ui代码中的地址配置指向本地接口文件（localhost:8080/api/swagger.json）

2）将swagger.json或swagger.yaml导入swagger editor可查看

3）将swagger.json或swagger.yaml导入swaggerhub可查看



### 2.2 swagger注解使用

[简单例子](https://jakubstas.com/spring-jersey-swagger-create-documentation/#.WMj-khJ95E4)

[接口文档](http://docs.swagger.io/swagger-core/current/apidocs/index.html)





