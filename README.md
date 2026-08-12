# OpenRouter 中文化插件

> 让 [OpenRouter](https://openrouter.ai) 界面全面中文化 | 实现方式参考 [github-chinese](https://github.com/maboloshi/github-chinese)

## 一键安装

[![安装脚本](https://img.shields.io/badge/一键安装-OpenRouter%20中文化插件-blue)](https://raw.githubusercontent.com/datou1996/openrouter-chinese/main/main.user.js)

点击上方按钮(或下方链接),Tampermonkey 会自动弹出安装确认:

- **一键安装(推荐)**:https://raw.githubusercontent.com/datou1996/openrouter-chinese/main/main.user.js

> 需要先安装 [Tampermonkey](http://tampermonkey.net/)(或 Violentmonkey),安装后刷新 OpenRouter 页面即可生效。

## 功能特性

- 全面中文化 OpenRouter 界面(导航、页脚、按钮、表单、弹窗、模型列表/详情、聊天、排行榜、文档、定价、提供商、工作区、设置、活动、日志等 20+ 页面类型)
- 按页面精细化词条:静态词典精确匹配 + 正则规则模糊匹配(数字单位、日期、价格、百分比等动态内容)
- 自动翻译动态加载内容:MutationObserver + 滚动防抖重扫 + 6 秒周期清扫 + body 替换检测,适配 Next.js 懒加载与客户端路由
- 处理 React 拆分文本节点(如 `90` + `% off`、`$15/M` + `UTF-8 bytes` 等数字与单位分离的情况)
- 页面标题翻译(含路由后标题重置的处理)
- 忽略规则保护:代码块、密钥、聊天输入框、AI 回复内容不参与翻译
- 日期本地化:`Aug 11, 2026` → `2026年8月11日`
- Tampermonkey 菜单:切换正则翻译、开发者模式(记录未翻译词条)、诊断扫描(定位未翻译节点的精确内容)
- 启动时在控制台输出脚本/词库版本号,便于排查缓存问题

## 安装指南

1. 安装脚本管理器 [Tampermonkey](http://tampermonkey.net/)(或 Violentmonkey)
2. 点击上方"一键安装"按钮(或直接访问下面的链接),Tampermonkey 会弹出安装确认页,点击"安装"即可:

```
https://raw.githubusercontent.com/datou1996/openrouter-chinese/main/main.user.js
```

3. 刷新 OpenRouter 页面即可生效

> 若需本地调试或修改词库地址,可编辑脚本头部的 `@require` 行。

## 常见问题排查

### 1. 更新后翻译未生效(词库缓存)

Tampermonkey 会缓存 `@require` 的外部文件,更新词库后需要强制刷新:

1. 打开 Tampermonkey 面板 → 找到脚本 → 点击"编辑"
2. 切换到"外部"标签页
3. 点击 `locals.js?v版本号` 旁的**刷新**按钮(或直接重新安装主脚本)
4. 刷新 OpenRouter 页面

**验证当前版本**:打开页面按 F12,控制台应显示:

```
[OpenRouter 中文化插件] 脚本 v1.3.8 / 词库 v1.3.8 / 公共词条数: ...
```

如果脚本与词库版本号不一致,说明外部资源缓存未刷新。

### 2. 仍有未翻译内容

- Tampermonkey 菜单 → **"诊断:扫描未翻译词条"**,把控制台输出的节点内容(含字节码与匹配结果)发到 [Issues](https://github.com/datou1996/openrouter-chinese/issues),即可精确修复
- 或启用**开发者模式**,刷新页面后控制台会记录未翻译词条

## 本地调试

1. 安装 Tampermonkey,并启用"允许访问文件网址"
2. 将 `locals.js` 下载到本地(如 `D:\openrouter-chinese\locals.js`)
3. 在脚本管理器中修改引用路径:

```js
// 原始路径
// @require https://raw.githubusercontent.com/datou1996/openrouter-chinese/main/locals.js

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
├── version                                     # 词库版本号
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
    ├── model                                   # 模型详情页(含 /厂商/模型 直链)
    ├── fusion                                  # 融合页
    ├── chat                                    # 聊天页
    ├── rankings                                # 排行榜(含各模态子页)
    ├── apps                                    # 应用页(含分类页)
    ├── compare                                 # 模型对比页
    ├── discover                                # 发现页
    ├── docs                                    # 文档页
    ├── workspaces                              # 工作区(密钥/护栏/BYOK/路由等)
    ├── settings                                # 设置页
    ├── activity                                # 活动记录(趋势/探索)
    ├── logs                                    # 日志页
    ├── pricing                                 # 定价页
    ├── providers                               # 提供商页
    ├── labs                                    # 实验室(成本模拟器)
    ├── spawn                                   # Spawn 页
    ├── signin                                  # 登录页
    ├── blog                                    # 博客
    └── misc                                    # 其他页面
```

页面类型通过 `main.user.js` 中的 `detectPageType()` 根据 URL 路径识别。

## 参与贡献

欢迎通过以下方式参与贡献:

1. **完善词库**:编辑 `locals.js` 添加缺失词条
2. **诊断未翻译内容**:Tampermonkey 菜单 → "诊断:扫描未翻译词条",把输出发到 Issues
3. **开启开发者模式**:未翻译的英文词条会自动记录到控制台
4. **提交议题**:反馈翻译错误或未翻译的内容

## 免责声明

- 本插件为第三方开发,与 OpenRouter 官方无关
- 翻译仅修改页面显示文本,不涉及任何账户、密钥等敏感数据
- 词库基于词典匹配,长文内容(如博客文章、文档正文、AI 回复)不会逐句翻译

## 许可证

[MIT](LICENSE)
