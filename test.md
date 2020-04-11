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

