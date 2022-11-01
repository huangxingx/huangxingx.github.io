---
title: gin 验证器
tags:
  - golang
  - gin
categories:
  - golang
auto_excerpt:
  enable: true
  length: 150
date: 2022-11-01 15:43:04
---



[toc]

### 前言
gin的验证器底层使用的是 validator 库实现的，故gin验证器可以使用validator库中的所有功能
- 所有标签
- 翻译器
- 自定义校验器
- 其他（不一一列举）

不同的是gin使用的Tag 为 binding，而validator库使用的是 validate；
gin中在初始化validate时通过validate.SetTagName("binding")设置标签

```golang
func (v *defaultValidator) lazyinit() {
	v.once.Do(func() {
		v.validate = validator.New()
		v.validate.SetTagName("binding")
	})
}
```


本文中 gin 版本为 v1.6.3, validator版本为 v10
* [gin](https://github.com/gin-gonic/gin)
* [validator](https://github.com/go-playground/validator)

### validator 库字段
**gin 使用时把 validate tag 改为 binding 即可**

| 字段                 | 描述                                                         | 例子                                                     |
| -------------------- | ------------------------------------------------------------ | -------------------------------------------------------- |
| required             | 必填                                                         | Field或Struct validate:"required"                        |
| omitempty            | 空时忽略                                                     | Field或Struct validate:"omitempty"                       |
| len                  | 长度                                                         | Field validate:"len=0"                                   |
| eq                   | 等于                                                         | Field validate:"eq=0"                                    |
| gt                   | 大于                                                         | Field validate:"gt=0"                                    |
| gte                  | 大于等于                                                     | Field validate:"gte=0"                                   |
| lt                   | 小于                                                         | Field validate:"lt=0"                                    |
| lte                  | 小于等于                                                     | Field validate:"lte=0"                                   |
| eqfield              | 同一结构体字段相等                                           | Field validate:"eqfield=Field2"                          |
| nefield              | 同一结构体字段不相等                                         | Field validate:"nefield=Field2"                          |
| gtfield              | 大于同一结构体字段                                           | Field validate:"gtfield=Field2"                          |
| gtefield             | 大于等于同一结构体字段                                       | Field validate:"gtefield=Field2"                         |
| ltfield              | 小于同一结构体字段                                           | Field validate:"ltfield=Field2"                          |
| ltefield             | 小于等于同一结构体字段                                       | Field validate:"ltefield=Field2"                         |
| eqcsfield            | 跨不同结构体字段相等                                         | Struct1.Field validate:"eqcsfield=Struct2.Field2"        |
| necsfield            | 跨不同结构体字段不相等                                       | Struct1.Field validate:"necsfield=Struct2.Field2"        |
| gtcsfield            | 大于跨不同结构体字段                                         | Struct1.Field validate:"gtcsfield=Struct2.Field2"        |
| gtecsfield           | 大于等于跨不同结构体字段                                     | Struct1.Field validate:"gtecsfield=Struct2.Field2"       |
| ltcsfield            | 小于跨不同结构体字段                                         | Struct1.Field validate:"ltcsfield=Struct2.Field2"        |
| ltecsfield           | 小于等于跨不同结构体字段                                     | Struct1.Field validate:"ltecsfield=Struct2.Field2"       |
| min                  | 最大值                                                       | Field validate:"min=1"                                   |
| max                  | 最小值                                                       | Field validate:"max=2"                                   |
| structonly           | 仅验证结构体，不验证任何结构体字段                           | Struct validate:"structonly"                             |
| nostructlevel        | 不运行任何结构级别的验证                                     | Struct validate:"nostructlevel"                          |
| dive                 | 向下延伸验证，多层向下需要多个dive标记                       | [][]string validate:"gt=0,dive,len=1,dive,required"      |
| dive                 | Keys& EndKeys	与dive同时使用，用于对map对象的键的和值的验证，keys为键，endkeys为值 | map[string]string validate :"gt=0,dive,keys,eq=1         |
| required_with        | 其他字段其中一个不为空且当前字段不为空                       | Field validate:"required_with=Field1 Field2"             |
| required_with_all    | 其他所有字段不为空且当前字段不为空                           | Field validate:"required_with_all=Field1 Field2"         |
| required_without     | 其他字段其中一个为空且当前字段不为空                         | Field `validate:”required_without=Field1 Field2”         |
| required_without_all | 其他所有字段为空且当前字段不为空                             | Field validate:"required_without_all=Field1 Field2"      |
| isdefault            | 是默认值                                                     | Field validate:"isdefault=0"                             |
| oneof                | 其中之一                                                     | Field validate:"oneof=5 7 9"                             |
| containsfield        | 字段包含另一个字段                                           | Field validate:"containsfield=Field2"                    |
| excludesfield        | 字段不包含另一个字段                                         | Field validate:"excludesfield=Field2"                    |
| unique               | 是否唯一，通常用于切片或结构体                               | Field validate:"unique"                                  |
| alphanum             | 字符串值是否只包含                                           | ASCII 字母数字字符	Field validate:"alphanum"          |
| alphaunicode         | 字符串值是否只包含                                           | unicode 字符	Field validate:"alphaunicode"            |
| alphanumunicode      | 字符串值是否只包含                                           | unicode 字母数字字符	Field validate:"alphanumunicode" |
| numeric              | 字符串值是否包含基本的数值                                   | Field validate:"numeric"                                 |
| hexadecimal          | 字符串值是否包含有效的十六进制                               | Field validate:"hexadecimal"                             |
| hexcolor             | 字符串值是否包含有效的十六进制颜色                           | Field validate:"hexcolor"                                |
| lowercase            | 符串值是否只包含小写字符                                     | Field validate:"lowercase"                               |
| uppercase            | 符串值是否只包含大写字符                                     | Field validate:"uppercase"                               |
| email                | 字符串值包含一个有效的电子邮件                               | Field validate:"email"                                   |
| json                 | 字符串值是否为有效的                                         | JSON	Field validate:"json"                            |
| file                 | 符串值是否包含有效的文件路径，以及该文件是否存在于计算机上   | Field validate:"file"                                    |
| url                  | 符串值是否包含有效的                                         | url	Field validate:"url"                              |
| uri                  | 符串值是否包含有效的                                         | uri	Field validate:"uri"                              |
| base64               | 字符串值是否包含有效的                                       | base64值	Field validate:"base64"                      |
| contains             | 字符串值包含子字符串值                                       | Field validate:"contains=@"                              |
| containsany          | 字符串值包含子字符串值中的任何字符                           | Field validate:"containsany=abc"                         |
| containsrune         | 字符串值包含提供的特殊符号值                                 | Field validate:"containsrune=☢"                          |
| excludes             | 字符串值不包含子字符串值                                     | Field validate:"excludes=@"                              |
| excludesall          | 字符串值不包含任何子字符串值                                 | Field validate:"excludesall=abc"                         |
| excludesrune         | 字符串值不包含提供的特殊符号值                               | Field validate:"containsrune=☢"                          |
| startswith           | 字符串以提供的字符串值开始                                   | Field validate:"startswith=abc"                          |
| endswith             | 字符串以提供的字符串值结束                                   | Field validate:"endswith=abc"                            |
| ip                   | 字符串值是否包含有效的                                       | IP 地址	Field validate:"ip"                           |
| ipv4                 | 字符串值是否包含有效的                                       | ipv4地址	Field validate:"ipv4"                        |
| datetime             | 字符串值是否包含有效的                                       | 日期	Field validate:"datetime"                        |


### 常用验证Tag
```golang
type Test struct {
	ID          int    `binding:"required"`             //数字确保不为0
	Name        string `binding:"required,min=1,max=8"` //字符串确保不为""，且长度 >=1 && <=8 （min=1,max=8等于gt=0,lt=9）
	Value       string `binding:"required,gte=1,lte=8"` //字符串确保不为""，且长度 >=1 && <=8
	Status      int    `binding:"min=1,max=10"`         //最小为0，最大为10（min=0,max=10等于gt=0,lt=11）
	PhoneNumber string `binding:"required,len=11"`      //不为""且长度为11
	Time        string `binding:"datetime=2006-01-02"`  //必须如2006-01-02的datetime格式
	Color       string `binding:"oneof=red green"`      //是能是red或者green
	Size        int    `binding:"oneof=37 39 41"`       //是能是37或者39或者41
	Email       string `binding:"email"`                //必须邮件格式
	JSON        string `binding:"json"`                 //必须json格式
	URL         string `binding:"url"`                  //必须url格式
	UUID        string `binding:"uuid"`                 //必须uuid格式
}
```

### 自定义验证器1
```golang
package main

import (
	"fmt"
	"net/http"
	"time"

	"github.com/go-playground/validator/v10"

	"github.com/gin-gonic/gin"
	"github.com/gin-gonic/gin/binding"
)

// Booking contains binded and validated data.
type Booking struct {
	CheckIn  time.Time `form:"check_in" binding:"required,bookabledate" time_format:"2006-01-02"`
	CheckOut time.Time `form:"check_out" binding:"required,gtfield=CheckIn" time_format:"2006-01-02"`
}

//validator v9以上写法
func bookableDate(fl validator.FieldLevel) bool {
	if date, ok := fl.Field().Interface().(time.Time); ok {
		today := time.Now()
		fmt.Println("date:", date)
		if date.Unix() > today.Unix() {
			fmt.Println("date unix ：", date.Unix())
			return true
		}
	}
	return false
}

//validator v8写法
//func bookableDate(
//  v *validator.Validate, topStruct reflect.Value, currentStructOrField reflect.Value,
//  field reflect.Value, fieldType reflect.Type, fieldKind reflect.Kind, param string,
//) bool {
//  if date, ok := field.Interface().(time.Time); ok {
//      today := time.Now()
//      if date.Unix()>today.Unix(){
//          return true
//      }
//  }
//  return false
//}

func main() {
	route := gin.Default()
	if v, ok := binding.Validator.Engine().(*validator.Validate); ok {
		_ = v.RegisterValidation("bookabledate", bookableDate)
	}
	route.GET("/bookable", getBookable)
	route.Run()
}

func getBookable(c *gin.Context) {
	var b Booking
	if err := c.ShouldBindWith(&b, binding.Query); err != nil {
		c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
	} else {
		c.JSON(http.StatusOK, gin.H{"message": "ok", "booking": b})
	}
}

```

### 验证器自定义错误信息1
原有的错误信息如下：
```json
{
    "message": "Key: 'LoginRequest.Mobile' Error:Field validation for 'Mobile' failed on the 'required' tag\nKey: 'LoginRequest.Code' Error:Field validation for 'Code' failed on the 'required' tag"
}
```
这样的提示信息不是很友好, 在 validator 文档中也说明了这个信息只是用在开发时进行调试用的. 那么我们怎么返回自定义的验证提示呢. 参考 validator 文档, 我是这样来实现的.

```golang
import (
    "gopkg.in/go-playground/validator.v8"
)

// 绑定模型
type LoginRequest struct {
    Mobile string   `form:"mobile" json:"mobile" binding:"required"`
    Code   string   `form:"code" json:"code" binding:"required"`
}

// 绑定模型获取验证错误的方法
func (r *LoginRequest) GetError (err validator.ValidationErrors) string {

    // 这里的 "LoginRequest.Mobile" 索引对应的是模型的名称和字段
    if val, exist := err["LoginRequest.Mobile"]; exist {
        if val.Field == "Mobile" {
            switch val.Tag{
                case "required":
                    return "请输入手机号码"
            }
        }
    }
    if val, exist := err["LoginRequest.Code"]; exist {
        if val.Field == "Code" {
            switch val.Tag{
                case "required":
                    return "请输入验证码"
            }
        }
    }
    return "参数错误"
}
```
如何使用模型, 以登录方法为例
```golang
import (
    "github.com/gin-gonic/gin"
    "net/http"
    "gopkg.in/go-playground/validator.v8"
)


func Login(c *gin.Context) {
    var loginRequest LoginRequest

    if err := c.ShouldBind(&loginRequest); err == nil { 
        // 参数接收正确, 进行登录操作

        c.JSON(http.StatusOK, loginRequest)
    }else{
        // 验证错误
        c.JSON(http.StatusUnprocessableEntity, gin.H{
            "message": loginRequest.GetError(err.(validator.ValidationErrors)), // 注意这里要将 err 进行转换
        })
    }
}
```

### 验证器自定义错误信息2
目录结构
```
.
├── main.go
└── valid
    └── valid.go

```
valid.valid.go
```golang
package valid

import (
	"fmt"
	"regexp"
	"strings"

	"github.com/gin-gonic/gin/binding"
	"github.com/go-playground/locales/zh"
	ut "github.com/go-playground/universal-translator"
	"github.com/go-playground/validator/v10"
	zh_translations "github.com/go-playground/validator/v10/translations/zh"
)

var v *validator.Validate
var Trans ut.Translator

// 初始化翻译器
func InitTrans() (err error) {
	//修改gin框架中的Validator属性，实现自定制
	if v, ok := binding.Validator.Engine().(*validator.Validate); ok {

		//v.RegisterTagNameFunc(func(fld reflect.StructField) string {
		//	return fld.Tag.Get("comment") //field.Tag.Get("json")
		//})

		v.RegisterValidation("checkMobile", checkMobile)

		zhT := zh.New() //中文翻译器

		// 第一个参数是备用（fallback）的语言环境
		// 后面的参数是应该支持的语言环境（支持多个）
		// uni := ut.New(zhT, zhT) 也是可以的
		uni := ut.New(zhT, zhT)

		var ok bool
		// 也可以使用 uni.FindTranslator(...) 传入多个locale进行查找
		Trans, ok = uni.GetTranslator("zh")
		if !ok {
			return fmt.Errorf("uni.GetTranslator(%s) failed", "zh")
		}
		zh_translations.RegisterDefaultTranslations(v, Trans)

		// 添加额外翻译
		_ = v.RegisterTranslation("checkMobile", Trans, func(ut ut.Translator) error {
			return ut.Add("checkMobile", "{0} 电话错误!", true)
		}, func(ut ut.Translator, fe validator.FieldError) string {
			t, _ := ut.T("checkMobile", fe.Field())
			return t
		})

	}
	return
}
func checkMobile(fl validator.FieldLevel) bool {
	ok, _ := regexp.MatchString(`^1[3-9][0-9]{9}$`, fl.Field().String())
	return ok
}

func ParseErr(errs validator.ValidationErrors) string {
	var errList []string
	for _, e := range errs {
		// can translate each error one at a time.
		errList = append(errList, e.Translate(Trans))
		//errList = append(errList, e.Field())
	}
	return strings.Join(errList, "|")

}

```
main.go
```golang
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
	"github.com/go-playground/validator/v10"

	"my-test/gin-validator/valid"
)

func main() {
	route := gin.Default()
	_ = valid.InitTrans()
	route.POST("/valid", validHandler)
	route.Run()
}

type req struct {
	Name   string `json:"name" binding:"required"`
	Mobile string `json:"mobile" binding:"checkMobile"`
	Age    int    `json:"mobile" binding:"gt=12,lt=100" comment:"年龄大于12，小于100"`
}

func validHandler(c *gin.Context) {
	req := req{}
	err := c.ShouldBindJSON(&req)
	if err != nil {
		errmsg := err.Error()
		if e, ok := err.(validator.ValidationErrors); ok {
			errmsg = valid.ParseErr(e)
		}
		c.JSON(http.StatusOK, gin.H{
			"error": errmsg,
		})
		return
	}
	c.JSON(http.StatusOK, "Success")
}

```

### 参考文档
- [gin - validator 参数校验](https://segmentfault.com/a/1190000022527284)
- [gin框架自定义验证错误提示信息](https://www.jianshu.com/p/0183d959edf1)
- [Golang使用validator进行数据校验及自定义翻译器](https://wangyangyangisme.github.io/2020/10/20/golang-Golang%E4%BD%BF%E7%94%A8validator%E8%BF%9B%E8%A1%8C%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%8F%8A%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BF%BB%E8%AF%91%E5%99%A8/#1-%E5%AE%9A%E4%B9%89%E9%94%99%E8%AF%AF%E7%BF%BB%E8%AF%91%E5%99%A8)
- [validator Go doc](https://godoc.org/gopkg.in/go-playground/validator.v8#hdr-Baked_In_Validators_and_Tags)