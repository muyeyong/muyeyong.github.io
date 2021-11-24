---
title: 快捷化-VSCode中React代码块快速生成
top: false
cover: false
toc: true
mathjax: true
date: 2020-05-07 14:55:42
password:
summary:
tags: VSCode快捷化
categories: 快捷化
---

# VSCode快速生成代码块--以React为例

> 写React项目的时候，有时候需要快速生成模板代码，比如生成一个组件，打出几个关键字就可以引入基本依赖和基本代码块。

## 利用Snippets，设置用户代码块

1. 起步

   文件 --> 首选项 --> 代码块

   ![](Snipaste_2020-05-07_15-31-48.png)

   选择创建或修改代码段

   ![](Snipaste_2020-05-07_15-36-06.png)

   + 创建全局代码片段，所有语言环境都可以触发
   + 创建对应语言的代码片段，特定语言环境下才能触发
   + 创建当前项目文件夹的代码片段

   输入代码段文件名

2. 使用Snippet语法，设置需要的代码块

   里面会有一个示例：

   ```
   "Print to console": {
   		"scope": "javascript,typescript",
   	 	"prefix": "log",
   	 	"body": [
   	 		"console.log('$1');",
   	 		"$2"
   	 	],
   	 	"description": "Log output to console"
   	 }
   ```

   1. `Print to console` 代码段的名称
   2. `scope` 代码段作用的范围
   3. `prefix` 对应触发代码片段的字符
   4. `body`代码段的内容
   5. `description` 对代码段的描述

3. 使用

   输入触发代码段的字符串

![](Snipaste_2020-05-07_15-58-44.png)

+ 占位符$

  + 使用

    `$ + 数字` 表示代码段落入编辑器后光标的位置，光标位置按从小到大排序。

  修改自定义的代码段

  ```
  	"Print to console": {
  		"scope": "javascript,typescript",
  		"prefix": "log",
  		"body": [
  			"console.log('$1');",
  			" $3 ",
  			"console.log('$2')",
  		],
  		"description": "Log output to console"
  	}
  ```

  ![](1.gif)

  ​	按`Tab`会按照数字的顺序（从小到大）跳转，但是注意`$0`是终止`Tab`跳转，（`$0`是最后执行的），数字相同会同时输入。

  + 占位符可选项

    `${1|可选项1|可选项2|可选项3}` 数字指的是光标的落入顺序

    ```
    	"方法注释": {
    		"prefix": "zs-Function",
    		"body": [
    			"/**",
    			" * @param name... { ${1|Boolean,Number,String,Object,Array|} }",
    			" * @description description...",
    			" * @return name... { ${2|Boolean,Number,String,Object,Array|} }",
    			" */",
    			"$0"
    		],
    		"description": "添加方法注释"
    	}
    ```

    ![](2.gif)

    ​	

+ 变量

  ***1）文档相关：***

  | 变量               | 变量含义                       |
  | ------------------ | ------------------------------ |
  | `TM_SELECTED_TEXT` | 当前选定的文本或空字符串       |
  | `TM_CURRENT_LINE`  | 当前行的内容                   |
  | `TM_CURRENT_WORD`  | 光标下的单词内容或空字符串     |
  | `TM_LINE_INDEX`    | 基于零索引的行号               |
  | `TM_LINE_NUMBER`   | 基于单索引的行号               |
  | `TM_FILENAME`      | 当前文档的文件名               |
  | `TM_FILENAME_BASE` | 当前文档没有扩展名的文件名     |
  | `TM_DIRECTORY`     | 当前文档的目录                 |
  | `TM_FILEPATH`      | 当前文档的完整文件路径         |
  | `CLIPBOARD`        | 剪贴板的内容                   |
  | `WORKSPACE_NAME`   | 已打开的工作空间或文件夹的名称 |

  ***2）当前日期和时间：***

  | 变量                       | 变量含义                                        |
  | -------------------------- | ----------------------------------------------- |
  | `CURRENT_YEAR`             | 当前年份                                        |
  | `CURRENT_YEAR_SHORT`       | 当前年份的最后两位数                            |
  | `CURRENT_MONTH`            | 月份为两位数（例如'02'）                        |
  | `CURRENT_MONTH_NAME`       | 月份的全名（例如'June'）（中文语言对应六月）    |
  | `CURRENT_MONTH_NAME_SHORT` | 月份的简称（例如'Jun'）（中文语言对应是6月）    |
  | `CURRENT_DATE`             | 这个月的哪一天                                  |
  | `CURRENT_DAY_NAME`         | 当天是星期几（例如'星期一'）                    |
  | `CURRENT_DAY_NAME_SHORT`   | 当天是星期几的简称（例如'Mon'）（中文对应周一） |
  | `CURRENT_HOUR`             | 24小时时钟格式的当前小时                        |
  | `CURRENT_MINUTE`           | 当前分                                          |
  | `CURRENT_SECOND`           | 当前秒                                          |

  ***3）要插入行或块注释，请遵循当前语言：***

  | 变量                  | 变量含义                   |
  | --------------------- | -------------------------- |
  | `BLOCK_COMMENT_START` | 输出：PHP /*或HTML格式<!-- |
  | `BLOCK_COMMENT_END`   | 输出：PHP */或HTML格式-->  |
  | `LINE_COMMENT`        | 输出：PHP //或HTML格式     |

  ​	项目中常用的用户代码块：

  ```
  {
    // Place your snippets for javascriptreact here. Each snippet is defined under a snippet name and has a prefix, body and 
    // description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
    // $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the 
    // same ids are connected.
    // Example:
    "Print to console": {
      "prefix": "log",
      "body": [
        "console.log('$1');",
        "$2"
      ],
      "description": "Log output to console"
    },
  	"db": {
  		"prefix": "db",
  		"body": [
  			"data-behavior=\"$0\""
  		],
  		"description": "data-behavior"
  	},
    "input getDerivedStateFromProps": {
      "prefix": "sfp",
      "body": [
        "static getDerivedStateFromProps(props, states) {",
        "\tconst {  } = props;",
        "\treturn {",
        "\t\t...states,",
        "\t};",
        "}"
      ],
      "description": "input getDerivedStateFromProps"
    },
    "const {} = this.props": {
      "prefix": "cp",
      "body": [
        "const { $0 } = this.props;",
      ],
      "description": "const {} = this.props"
    },
    "create a component": {
      "prefix": "component",
      "body": [
        "import React from 'react';",
        "import classnames from 'classnames';\n",
        "class $1 extends React.Component<I$1Props, I$1States> {\n",
        "\trender() {\n\t\t$0\n\t}\n}\n",
        "interface I$1Props {\n}\n",
        "interface I$1States {\n}\n",
        "export default $1;"
      ],
      "description": "create a component"
    },
    "create a container": {
      "prefix": "container",
      "body": [
        "import React from 'react';",
        "import { inject, observer } from 'mobx-react';",
        "import { $2, I$2Props } from '@business/header/header';\n",
        "@inject($2)",
        "@observer",
        "class $1 extends React.Component<I$1Props, I$1States> {\n",
        "\trender() {\n\t\t$0\n\t}\n}\n",
        "interface I$1Props extends I$2Props {\n}\n",
        "interface I$1States {\n}\n",
        "export default $1;"
      ],
      "description": "create a container"
    },
    "create a hoc": {
      "prefix": "hoc",
      "body": [
        "import React from 'react';",
        "import { observer } from 'mobx-react';\n",
        "function with$2(WrappedComponent: React.ComponentClass) {\n",
        "\t@observer",
        "\tclass $2Wrapper extends React.Component<I$2WrapperProps, {}> {\n",
        "\t\trender() {",
        "\t\t\treturn <WrappedComponent {...this.props} />;",
        "\t\t}",
        "\t}",
        "\treturn $2Wrapper;",
        "}\n",
        "interface I$2WrapperProps extends IKeyValueMap {\n",
        "}\n",
        "export default with$2;"
      ],
      "description": "create a hoc"
    },
    "create a intl": {
      "prefix": "intl",
      "body": [
        "<FormattedMessage id=\"$0\" />"
      ],
      "description": "create a intl"
    },
    "import a intl": {
      "prefix": "intlimport",
      "body": [
        "import { FormattedMessage } from 'react-intl';"
      ],
      "description": "import a intl"
    },
    "create classnames": {
      "prefix": "classnames",
      "body": [
        "{classnames(\"\", $1)}"
      ],
      "description": "create classnames"
    },
    "create a SCU": {
      "prefix": "SCU",
      "body": "\nshouldComponentUpdate(nextProps, nextState){\n\t$1\n}\n",
      "description": "create a SCU"
    },
    "create a componentWillReceiveProps": {
      "prefix": "CWRP",
      "body": "\ncomponentWillReceiveProps(nextProps, nextState){\n\t$1\n}\n",
      "description": "create a componentWillReceiveProps"
    },
    "create a componentDidMount": {
      "prefix": "CDM",
      "body": "\ncomponentDidMount(){\n\t$1\n}\n",
      "description": "create a ncomponentDidMount"
    },
    "create a componentWillUnmount": {
      "prefix": "CWUM",
      "body": "\ncomponentWillUnmount(){\n\t$1\n}\n",
      "description": "create a componentWillUnmount"
    },
    "create a component test": {
      "prefix": "test",
      "body": [
        "import React from 'react';",
        "import { shallow, mount, render } from 'enzyme';",
        "import renderer from 'react-test-renderer';\n",
        "describe('$1', () => {\n\t$0\n});",
      ],
      "description": "create a component test"
    },
    "create a it test": {
      "prefix": "it",
      "body": [
        "it('$1', () => {\n\t$0\n});"
      ],
    },
      "description": "create a it test"
  }
  ```

  

  # 使用VSCode插件--ES7 React/Redux/GraphQL/React-Native snippets

  ​	安装这个插件后，使用内置的命令实现快速生成代码块

  ![](3.gif)

  详情可查看github:https://github.com/dsznajder/vscode-es7-javascript-react-snippets