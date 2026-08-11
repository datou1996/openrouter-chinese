# OpenRouter 中文化插件

> 让 [OpenRouter](https://openrouter.ai) 界面全面中文化 | 实现方式参考 [github-chinese](https://github.com/maboloshi/github-chinese)

## 功能特性

- 全面中文化 OpenRouter 界面元素(主导航、页脚、按钮、表单、弹窗等)
- 按页面精细化词条(首页、模型列表、模型详情、聊天、排行榜、文档、设置等)
- 智能匹配:静态词典精确匹配 + 正则规则模糊匹配
- 自动翻译动态加载内容(MutationObserver 监听 DOM 变化,适配 Next.js 客户端路由)
- 页面标题翻译
- 忽略规则保护:代码块、密钥、聊天输入框、AI 回复内容不参与翻译
- 自动检测 URL 变化,页面切换时自动应用对应词库
- Tampermonkey 菜单:切换正则翻译、开发者模式(记录未翻译词条便于完善词库)

## 安装指南

1. 安装脚本管理器 [Tampermonkey](http://tampermonkey.net/)(或 Violentmonkey)
2. 将本项目中的 `locals.js` 发布到可访问的地址(如 GitHub 仓库的 raw 链接、GreasyFork 托管文件等)
3. 修改 `main.user.js` 头部的 `@require` 地址为你的词库地址
4. 将修改后的 `main.user.js` 安装到 Tampermonkey
5. 刷新 OpenRouter 页面即可生效

本项目已托管于 GitHub,可直接安装:

- 主脚本:https://raw.githubusercontent.com/datou1996/openrouter-chinese/main/main.user.js
- 词库文件:https://raw.githubusercontent.com/datou1996/openrouter-chinese/main/locals.js

## 本地调试

参考 github-chinese 的调试方式:

1. 安装 Tampermonkey,并启用"允许访问文件网址"
2. 将 `locals.js` 下载到本地(如 `D:\openrouter-chinese\locals.js`)
3. 在脚本管理器中修改引用路径:

```js
// 原始路径
// @require https://raw.githubusercontent.com/你的GitHub用户名/openrouter-chinese/main/locals.js

// 修改为本地路径
// @require file:///D:/openrouter-chinese/locals.js
```

**若无效:**

1. 进入 Tampermonkey 插件`设置页`
2. 将`通用 - 配置模式`设置为`高级`,进入高级设置模式
3. 找到`安全 - 允许脚本访问本地文件`并设置为`外部(@require 和 @resource)`

## 词库结构

```
I18N
├── conf                                        # 全局配置
│   ├── ignoreSelectorPage                      # 忽略翻译的选择器(全局/按页面)
│   ├── ignoreMutationSelectorPage              # 忽略 DOM 变化重翻译的选择器
│   └── characterDataPage                       # 需要监视文本变化的页面
└── 'zh-CN'
    ├── public                                  # 公共词条(所有页面通用)
    │   ├── static                              # 静态词典(精确匹配)
    │   ├── regexp                              # 正则规则([模式, 替换])
    │   └── title                               # 页面标题词条
    ├── home                                    # 首页
    ├── models                                  # 模型列表页
    ├── model                                   # 模型详情页
    ├── chat                                    # 聊天页
    ├── rankings                                # 排行榜
    ├── apps                                    # 应用页
    ├── docs                                    # 文档页
    ├── settings                                # 设置页
    ├── signin                                  # 登录页
    ├── blog                                    # 博客
    └── misc                                    # 其他页面
```

页面类型通过 `main.user.js` 中的 `detectPageType()` 根据 URL 路径识别。

## 参与贡献

欢迎通过以下方式参与贡献:

1. **完善词库**:编辑 `locals.js` 添加缺失词条
2. **开启开发者模式**:Tampermonkey 菜单中启用"开发者模式",未翻译的英文词条会自动输出到控制台,方便补齐词库
3. **提交议题**:反馈翻译错误或未翻译的内容

## 免责声明

- 本插件为第三方开发,与 OpenRouter 官方无关
- 翻译仅修改页面显示文本,不涉及任何账户、密钥等敏感数据
- 词库基于词典匹配,长文内容(如博客文章、文档正文、AI 回复)不会逐句翻译

## 许可证

[MIT](LICENSE)
