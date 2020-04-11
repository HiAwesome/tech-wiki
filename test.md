[[_TOC_]]

# 测试 Gitlab Markdown 语法

* [官方文档](https://docs.gitlab.com/ee/user/markdown.html#gitlab-flavored-markdown-gfm)
* [官方热更新的文档](https://gitlab.com/gitlab-org/gitlab/blob/master/doc/user/markdown.md)

### 颜色测试
- `#F00`
- `#F00A`
- `#FF0000`
- `#FF0000AA`
- `RGB(0,255,0)`
- `RGB(0%,100%,0%)`
- `RGBA(0,255,0,0.3)`
- `HSL(540,70%,50%)`
- `HSLA(540,70%,50%,0.3)`

### 图表和流程图

#### Mermaid

1. 流程图

```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```

2. 类图

```mermaid
classDiagram
	Animal <|-- Duck
	Animal <|-- Fish
	Animal <|-- Zebra
	Animal : +int age
	Animal : +String gender
	Animal: +isMammal()
	Animal: +mate()
	class Duck{
		+String beakColor
		+swim()
		+quack()
	}
	class Fish{
		-int sizeInFeet
		-canEat()
	}
	class Zebra{
		+bool is_wild
		+run()
	}
```

### 表情

有时您想稍微 :monkey: 一下，然后在您的💬上加上一些🌟。 好吧，我们有礼物送给您：
⚡您可以在支持GFM的任何地方使用表情符号。 ✌
您可以使用它指出🐛或警告🙊补丁。 如果有人改进了您的really代码，请向他们发送一些🎂。 人们会为此而嘘你。
如果您是新手，请不要be。 您可以轻松加入表情符号👪。 您需要做的只是查找受支持的代码之一。
有关所有受支持的表情符号代码的列表，请查阅表情符号备忘单。 👍

Sometimes you want to :monkey: around a bit and add some :star2: to your :speech_balloon:. Well we have a gift for you:

:zap: You can use emoji anywhere GFM is supported. :v:

You can use it to point out a :bug: or warn about :speak_no_evil: patches. And if someone improves your really :snail: code, send them some :birthday:. People will :heart: you for that.

If you're new to this, don't be :fearful:. You can easily join the emoji :family:. All you need to do is to look up one of the supported codes.

Consult the [Emoji Cheat Sheet](https://www.emojicopy.com) for a list of all supported emoji codes. :thumbsup:

