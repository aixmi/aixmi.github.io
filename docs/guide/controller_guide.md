### 1. 入参与出参

- 所有接口使用post方式提交，使用`@PostMapping`注解标记，请求参数的内容格式为`application/json`
- api接口的Controller类名应以`*Controller`/`*Resource`/`*Api`结尾
- api接口的Controller应标记为`@RestController`
- api接口的Controller 都应标记`@Validated`
- 请求接口参数会有以下几种情况
  - 创建请求：使用`*CreateForm` 定义创建请求实体，并将对应实体打上注解`@Valid`
  - 更新请求：使用`*EditForm` 定义更新请求实体,该`Form`实体不能通用，如果有通用需求，请抽象出父类，使用子类作为参数，并将对应实体打上注解`@Valid`
  - 查询请求: 使用`*QueryForm`定义查询请求实体
  - queryParam形式: 少量参数使用`@RequestParam`,如果参数较多使用`*QueryParam`形式定义实体
  - 如果需要分页: 请求的实体参数应该继承`QueryRequest`
- 请求请求响应
  - 搜友响应的实体都已`*VO`定义
  - 所有的接口都应该使用`ResultBean<T>`封装
  - 如果需要返回分页结果使用`ResultBean<T extends PageResult>`封装
  - 所有的接口必须有返回内容
    - 新增: 返回新增主键ID
    - 更新: 返回true/false
  - 下载流使用`DownloadBean<T>`封装响应结果

下面是一个controller类的例子:

```java
@RestController
@RequestMapping("/demo")
@Validated
@RequiredArgsConstructor
public class DemoController{
    private final IDemoService iDemoSerivce;
    @PostMapping("/add")
    public ResultBean<Long> add(@RequestBody @Valid DemoCreateForm demoCreateForm){
        return new ResultBean<Long>(iDemoService.add(demoCreateForm));
    }
    @PostMapping("/update")
    public ResultBean<Boolean> update(@RequestBody @Valid DemoUpdateForm demoUpdateForm){
        return new ResultBean<Boolean>(iDemoService.update(demoCreateForm));
    }
    
    @PostMapping("/search")
    public ResultBean<DemoQueryVO> search(@RequestBody @Valid DemoQueryForm demoQueryForm){
        return new ResultBean<DemoQueryVO>(iDemoService.search(demoCreateForm));
    }
}
```



### 接口方法命名

命名是一个比较难的问题，我们在这里列出一些常见的业务需求，遵循这个规则将会让组内的其他人员一眼就能明白你这个方法是在做什么业务。这需要团队成员之间的磨合，也需要在业务中不断的完善这个规范。总之,这个规范的目的就是为了统一命名，避免业务开发人员取名字的烦恼，减少沟通的成本。

|      需求      |   建议路径(例)   |  建议命名(例)   |
| :------------: | :--------------: | :-------------: |
|      新增      |       /add       |       add       |
|  更新多个字段  |     /update      |     update      |
|  更新单个字段  | /changePayStatus | changePayStatus |
|    单个查询    |      /info       |      info       |
|    批量查询    |     /search      |     search      |
| 提供三方的回调 |    /*Callback    |    *Callback    |
| 获取某单条信息 |      /get*       |      get*       |
| 获取某批量信息 |      /list*      |      list*      |
|  获取分页信息  |      /page*      |      page*      |
|  其他常见动词  |                  |     send* ,     |

