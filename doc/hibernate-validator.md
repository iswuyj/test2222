## hibernate-validator



> https://beanvalidation.org/
>
> ==================
>
> https://hibernate.org/  hibernate官网
>
> https://hibernate.org/validator/

### JAVA中通过Hibernate-Validation进行参数验证

在开发JAVA服务器端代码时，我们会遇到对外部传来的参数合法性进行验证，而hibernate-validator提供了一些常用的参数校验注解，我们可以拿来使用。
**1.maven中引入hibernate-validator对应的jar：**

```xml
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>4.3.1.Final</version> 
</dependency>
```

**2.在Model中定义要校验的字段：**

```java
import javax.validation.constraints.Pattern;
import javax.validation.constraints.Size;
import org.hibernate.validator.constraints.NotEmpty;
 
public class PayRequestDto {
     
    /**
     * 支付完成时间
     **/
    @NotEmpty(message="支付完成时间不能空")
    @Size(max=14,message="支付完成时间长度不能超过{max}位")
    private String payTime;
     
    /**
     * 状态
     **/
    @Pattern(regexp = "0[0123]", message = "状态只能为00或01或02或03")
    private String status;
 
    public String getPayTime() {
        return payTime;
    }
 
    public void setPayTime(String payTime) {
        this.payTime = payTime;
    }
 
    public String getStatus() {
        return status;
    }
 
    public void setStatus(String status) {
        this.status = status;
    }
}
```

**3.定义Validation工具类：**

```
import java.util.Set;
import javax.validation.ConstraintViolation;
import javax.validation.Validation;
import javax.validation.Validator;
import org.hibernate.validator.HibernateValidator;
import com.atai.framework.lang.AppException;

public class ValidationUtils {
    
    /**
     * 使用hibernate的注解来进行验证
     * 
     */
    private static Validator validator = Validation
            .byProvider(HibernateValidator.class).configure().failFast(true).buildValidatorFactory().getValidator();

    /**
     * 功能描述: <br>
     * 〈注解验证参数〉
     *
     * @param obj
     * @see [相关类/方法](可选)
     * @since [产品/模块版本](可选)
     */
    public static <T> void validate(T obj) {
        Set<ConstraintViolation<T>> constraintViolations = validator.validate(obj);
        // 抛出检验异常
        if (constraintViolations.size() > 0) {
            throw new AppException("0001", String.format("参数校验失败:%s", constraintViolations.iterator().next().getMessage()));
        }
    }
}
```

**4.在代码中调用工具类进行参数校验：**

```
ValidationUtils.validate(requestDto);
```


**以下是对hibernate-validator中部分注解进行描述：**

## 校验注解

#### 1. JSR-303 包含的注解

| 注解名称                  | 说明                                           |
| :------------------------ | :--------------------------------------------- |
| @Null                     | 被注解元素必须为 null                          |
| @NonNull                  | 被注解元素必须不为 null                        |
| @AssertTrue               | 被注解元素必须为 true                          |
| @AssertFalse              | 被注解元素必须为 false                         |
| @Min(value)               | 被注解元素必须是一个值，并且不能小于指定的值   |
| @Max(value)               | 被注解元素必须是一个值，并且不能大于指定的值   |
| @DecimalMin(value)        | 被注解元素必须是一个数字，并且不能小于指定的值 |
| @DecimalMax(value)        | 被注解元素必须是一个数字，并且不能大于指定的值 |
| @Size(max=,min=)          | 被注解元素的大小必须在指定范围内               |
| @Digits(integer,fraction) | 被注解元素必须是一个数字，其值必须在指定范围内 |
| @Past                     | 被注解元素必须是一个过去的日期                 |
| @Future                   | 被注解元素必须是一个将来的日期                 |
| @Pattern(regex=,flag=)    | 被注解元素必须符合指定的正则表达式             |

#### 2. Hibernate Validator 扩展的注解

| 注解名称                   | 说明                                                       |
| :------------------------- | :--------------------------------------------------------- |
| @NotBlank(message=)        | 被注解的字符串必须非 null 且`trim()`后长度大于 0           |
| @Email                     | 被注解元素必须是电子邮箱地址                               |
| @Length(min=,max=)         | 被注解的字符串的长度必须在指定范围内                       |
| @NotEmpty                  | 被注解元素（字符串、数组、集合等）必须非 null 且长度大于 0 |
| @Range(min=,max=,message=) | 被注解元素必须在合适的范围内                               |
| @URL                       | 被注解元素必须是合法的 URL                                 |

## 用法

![image-20210907175805180](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210907175805.png)

![image-20210908000351056](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210908000351.png)

![image-20210908000514247](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210908000514.png)

![image-20210908002010559](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210908002010.png)

![image-20210908000711178](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210908000711.png)

![image-20210908001321283](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210908001321.png)

![image-20210909151704468](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210909151704.png)

![image-20210909151846118](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210909151846.png)

![image-20210909151935049](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210909151935.png)

`  @Valid 注解  加在实体类上·`

 

# [java bean validation 参数验证](https://www.cnblogs.com/jpfss/p/11097753.html)

 [一、前言](https://www.cnblogs.com/jpfss/p/11097753.html#a)

[二、几种解决方案](https://www.cnblogs.com/jpfss/p/11097753.html#b)

[三、使用bean validation 自带的注解验证](https://www.cnblogs.com/jpfss/p/11097753.html#c)

[四、自定义bean validation 注解验证](https://www.cnblogs.com/jpfss/p/11097753.html#d)

 ![image-20210909013627107](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210909013627.png)

 ![image-20210909011923058](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210909011923.png)

## 一、前言

　　在后台开发过程中，对参数的校验成为开发环境不可缺少的一个环节。比如参数不能为null，email那么必须符合email的格式，如果手动进行if判断或者写正则表达式判断无意开发效率太慢，在时间、成本、质量的博弈中必然会落后。所以把校验层抽象出来是必然的结果，下面说下几种解决方案。

## 二、几种解决方案

　　1、struts2的valid可以通过配置xml，xml中描述规则和返回的信息，这种方式比较麻烦、开发效率低，不推荐

　　2、validation bean 是基于JSR-303标准开发出来的，使用注解方式实现，及其方便，但是这只是一个接口，没有具体实现.Hibernate Validator是一个hibernate独立的包，可以直接引用，他实现了validation bean同时有做了扩展，比较强大 ，实现图如下：

　　　　![img](https://images2015.cnblogs.com/blog/827161/201610/827161-20161022141655263-2092739140.png)

　　　　[点此查看中文官方手册](http://docs.jboss.org/hibernate/validator/4.2/reference/zh-CN/html_single/#preface)

　　　3、[oval](http://oval.sourceforge.net/) 是一个可扩展的Java对象数据验证框架，验证的规则可以通过配置文件、Annotation、POJOs 进行设定。可以使用纯 Java 语言、JavaScript 、Groovy 、BeanShell 等进行规则的编写，本次不过多讲解

## 三、bean validation 框架验证介绍

　　bean validation 包放在maven上维护,最新包的坐标如下：

```
<``dependency``>``  ``<``groupId``>javax.validation</``groupId``>``  ``<``artifactId``>validation-api</``artifactId``>``  ``<``version``>1.1.0.Final</``version``>``</``dependency``>
```

　　 [点击这里查看最新的坐标地址](http://search.maven.org/#search|gav|1|g%3A"javax.validation" AND a%3A"validation-api"　)

　　 下载之后打开这个包，有个package叫constraints，里面放的就是验证的的注解：

　　 ![img](https://images2015.cnblogs.com/blog/827161/201610/827161-20161022150209967-1863141332.png)

　　 下面开始用代码实践一下：

　　 1、定义一个待验证的bean:Student.java

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
 1 package com.shishang;
 2 
 3 import javax.validation.constraints.*;
 4 import java.io.Serializable;
 5 import java.math.BigDecimal;
 6 import java.util.Date;
 7 
 8 public class Student implements Serializable {
 9 
10 
11     @NotNull(message = "名字不能为空")
12     private String name;
13 
14     @Size(min = 6,max = 30,message = "地址应该在6-30字符之间")
15     private String address;
16 
17     @DecimalMax(value = "100.00",message = "体重有些超标哦")
18     @DecimalMin(value = "60.00",message = "多吃点饭吧")
19     private BigDecimal weight;
20 
21     private String friendName;
22     @AssertTrue
23     private Boolean isHaveFriend(){
24         return friendName != null?true:false;
25     }
26 
27     @Future(message = "生日必须在当前实践之前")
28     private Date birthday;
29 
30     @Pattern(regexp = "^(.+)@(.+)$",message = "邮箱的格式不合法")
31     private String email;
32 
33 
34     public String getName() {
35         return name;
36     }
37 
38     public void setName(String name) {
39         this.name = name;
40     }
41 
42     public String getAddress() {
43         return address;
44     }
45 
46     public void setAddress(String address) {
47         this.address = address;
48     }
49 
50     public BigDecimal getWeight() {
51         return weight;
52     }
53 
54     public void setWeight(BigDecimal weight) {
55         this.weight = weight;
56     }
57 
58     public String getFriendName() {
59         return friendName;
60     }
61 
62     public void setFriendName(String friendName) {
63         this.friendName = friendName;
64     }
65 
66     public Date getBirthday() {
67         return birthday;
68     }
69 
70     public void setBirthday(Date birthday) {
71         this.birthday = birthday;
72     }
73 
74     public String getEmail() {
75         return email;
76     }
77 
78     public void setEmail(String email) {
79         this.email = email;
80     }
81 }
```

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

   2、测试类:StudentTest.java

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
 1 package com.use;
 2 
 3 import javax.validation.ConstraintViolation;
 4 import javax.validation.Validation;
 5 import javax.validation.Validator;
 6 import javax.validation.ValidatorFactory;
 7 import java.io.Serializable;
 8 import java.math.BigDecimal;
 9 import java.util.ArrayList;
10 import java.util.Date;
11 import java.util.List;
12 import java.util.Set;
13 
14 public class StudentTest implements Serializable {
15     public static void main(String[] args) {
16         Student xiaoming = getBean();
17         List<String> validate = validate(xiaoming);
18         validate.forEach(row -> {
19             System.out.println(row.toString());
20         });
21 
22     }
23 
24     private static Student getBean() {
25         Student bean = new Student();
26         bean.setName(null);
27         bean.setAddress("北京");
28         bean.setBirthday(new Date());
29         bean.setFriendName(null);
30         bean.setWeight(new BigDecimal(30));
31         bean.setEmail("xiaogangfan163.com");
32         return bean;
33     }
34 
35     private static ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
36 
37     public static <T> List<String> validate(T t) {
38         Validator validator = factory.getValidator();
39         Set<ConstraintViolation<T>> constraintViolations = validator.validate(t);
40 
41         List<String> messageList = new ArrayList<>();
42         for (ConstraintViolation<T> constraintViolation : constraintViolations) {
43             messageList.add(constraintViolation.getMessage());
44         }
45         return messageList;
46     }
47 }
```

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

   3、运行testValidation()方法，输处如下：

```
地址应该在6-30字符之间
邮箱的格式不合法
生日必须在当前时间之前
多吃点饭吧
名字不能为空 
```

   4、总结 

- 像@NotNull、@Size等比较简单也易于理解，不多说
- 因为bean validation只提供了接口并未实现，使用时需要加上一个provider的包，例如hibernate-validator
- @Pattern 因为这个是正则，所以能做的事情比较多，比如中文还是数字、邮箱、长度等等都可以做
- @AssertTRue 这个与其他的校验注解有着本质的区别，这个注解适用于多个字段。例子中isHaveFriend方法依赖friendName字段校验
- 验证的api是经过我加工了一下，这样可以批量返回校验的信息
- 有时我们需要的注解可能没有提供，这时候就需要自定义注解，写实现类，下面说一下自定义注解的使用

 

## 四、自定义bean validation 注解验证

　　有时框架自带的没法满足我们的需求，这时就需要自己动手丰衣足食了，恩恩 ，这个不难，下面说下。

　　这个例子验证字符串是大写还是小写约束标注，代码如下:

　　1、**枚举类型`CaseMode`, 来表示大写或小写模式**

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 package com.defineconstrain;
 2 
 3 /**
 4  * created by xiaogangfan
 5  * on 16/10/25.
 6  */
 7 public enum CaseMode {
 8     UPPER,
 9     LOWER;
10 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　2、**定义一个CheckCase的约束标注** 

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 package com.defineconstrain;
 2 
 3 /**
 4  * created by xiaogangfan
 5  * on 16/10/25.
 6  */
 7 import static java.lang.annotation.ElementType.*;
 8 import static java.lang.annotation.RetentionPolicy.*;
 9 
10 import java.lang.annotation.Documented;
11 import java.lang.annotation.Retention;
12 import java.lang.annotation.Target;
13 
14 import javax.validation.Constraint;
15 import javax.validation.Payload;
16 
17 @Target( { METHOD, FIELD, ANNOTATION_TYPE })
18 @Retention(RUNTIME)
19 @Constraint(validatedBy = CheckCaseValidator.class)
20 @Documented
21 public @interface CheckCase {
22 
23     String message() default "{com.mycompany.constraints.checkcase}";
24 
25     Class<?>[] groups() default {};
26 
27     Class<? extends Payload>[] payload() default {};
28 
29     CaseMode value();
30 
31 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 　3、**约束条件`CheckCase`的验证器**

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 package com.defineconstrain;
 2 
 3 /**
 4  * created by xiaogangfan
 5  * on 16/10/25.
 6  */
 7 import javax.validation.ConstraintValidator;
 8 import javax.validation.ConstraintValidatorContext;
 9 
10 public class CheckCaseValidator implements ConstraintValidator<CheckCase, String> {
11 
12     private CaseMode caseMode;
13 
14     public void initialize(CheckCase constraintAnnotation) {
15         this.caseMode = constraintAnnotation.value();
16     }
17 
18     public boolean isValid(String object, ConstraintValidatorContext constraintContext) {
19 
20         if (object == null)
21             return true;
22 
23         if (caseMode == CaseMode.UPPER)
24             return object.equals(object.toUpperCase());
25         else
26             return object.equals(object.toLowerCase());
27     }
28 
29 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　4、在Student.java中增加一个属性

![img](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)

```
1 @CheckCase(value = CaseMode.LOWER,message = "名字的拼音需要小写")
2     private String spellName;
```

　　5、在StudentTest.java的getBean()方法中增加一行

```
bean.setSpellName("XIAOGANGFAN");
```

　　6、运行testValidation()方法，输处如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![复制代码](https://common.cnblogs.com/images/copycode.gif)

```
地址应该在6-30字符之间
邮箱的格式不合法
生日必须在当前时间之前
多吃点饭吧
名字的拼音需要小写
名字不能为空
```

![复制代码](https://common.cnblogs.com/images/copycode.gif)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

　　7、说明新增的约束生效了，大功告成

代码下载地址：git@github.com:xiaogangfan/vaidation.git

命令： git clone git@github.com:xiaogangfan/vaidation.git

分类: [工作总结](https://www.cnblogs.com/jpfss/category/992649.html), [开发经验](https://www.cnblogs.com/jpfss/category/992650.html)



自定义注解

![image-20210909153541303](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210909153541.png)

![image-20210910011422953](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210910011423.png)

![image-20210911220017205](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210911220017.png)

- 1.@NotNull：不能为null，但可以为empty

  ```
  (""," ","   ")      
  ```

- 2.@NotEmpty：不能为null，而且长度必须大于0

  ```
  (" ","  ")
  ```

- 3.@NotBlank：只能作用在String上，不能为null，而且调用trim()后，长度必须大于0

  ```
  ("test")    即：必须有实际字符
  ```

```
1.String name = null;

@NotNull: false
@NotEmpty:false 
@NotBlank:false 



2.String name = "";

@NotNull:true
@NotEmpty: false
@NotBlank: false



3.String name = " ";

@NotNull: true
@NotEmpty: true
@NotBlank: false



4.String name = "Great answer!";

@NotNull: true
@NotEmpty:true
@NotBlank:true
```





分组验证



![image-20210914161324993](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210914161325.png)

![image-20210914161355374](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210914161355.png)





自定义注解（复制@size注解）

![image-20210914163451676](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210914163451.png)

![image-20210914164333511](https://cdn.jsdelivr.net/gh/iswuyj/imagesFile/img/20210914164333.png)



集合分组验证

### 