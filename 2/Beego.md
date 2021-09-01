# Beego

### Beego 生成路由问题

#### [Beego Github Issues: commentsRouter](https://github.com/beego/beego/issues?q=commentsRouter)

[fail to generate routers/commentsRouter_xxx.go file when the project outside of $GOPATH/src](https://github.com/beego/beego/issues/4330)

经测试 2.0 以上可以生成路由信息。

#### 测试路由代码

```golang
// Get123
// @router /get123 [get]
func (c *MainController) Get123() {
	c.Data["Website"] = "beego.me"
	c.Data["Email"] = "astaxie@gmail.com"
	c.TplName = "index.tpl"
}

// Get456
// @router /get456 [get]
func (c *MainController) Get456() {
	c.Data["Website"] = "beego.me"
	c.Data["Email"] = "astaxie@gmail.com"
	c.TplName = "index.tpl"
}
```

### Beego 1.12.3 生成路由问题

Beego 1.12.3 放置项目在 src 下无法生成路由信息，原因待查。
