# googletranlate

//别的项目上传到自己的github账号上当 库使用

Install: go get github.com/szkjtv/googletranlate
或执行 
go mod init 
go mod tidy 


```go
//原生执行案例
package main
	import (
		"fmt"
		gt "github.com/bas24/googletranslatefree"
	)
	func main(){
		const text string = `Hello, World!`
		// you can use "auto" for source language
		// so, translator will detect language
		result, _ := gt.Translate(text, "en", "es")
		fmt.Println(result)
		// Output: "Hola, Mundo!"
	}
  

```
  
```go
//gin框架执行案例  使用post方法
package main

// https://github.com/bas24/googletranslatefree  案例改成gin框架web形式开发

import (
	// gt "github.com/bas24/googletranslatefree"
	// gt "tranlate/translategooglefree"
	// gt "example.com/m/translategooglefree"

	"github.com/gin-gonic/gin"
	gt "github.com/szkjtv/googletranlate"
)

func Tranlate(c *gin.Context) {
	tranlate := c.PostForm("tranlate")
	// 先把中文翻译成英文，不要这个结果
	result, _ := gt.Translate(tranlate, "zh", "en")
	// 把翻译成英文的结果，再次翻译成中文
	resultchienese, _ := gt.Translate(result, "en", "zh")

	if tranlate == "" {
		c.JSON(200, "请输入内容再生成")
		return
	}

	c.String(200, resultchienese) //接收最终结果
	// fmt.Println(resultchienese)
	// c.String(200, resultchienese)
	// c.HTML(200, "index.html", resultchienese)
}
func GetPage(c *gin.Context) {
	c.HTML(200, "index.html", nil)
}

func Router() {
	r := gin.Default()
	r.LoadHTMLGlob("view/*")
	// router.Static("/static", "./static")
	r.GET("/", GetPage)
	// r.POST("/", Tranlate)
	r.POST("/tr", Tranlate)
	r.Run(":9090")
}

func main() {
	Router()

}

```


