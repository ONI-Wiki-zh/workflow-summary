# ONI zh Wiki Workflow——Technical details in nutshells
本文档旨在总结[缺氧中文 wiki](https://oni.fandom.com/zh) 的组成架构，介绍部分技术细节，以供编辑者参考。

由于内容架构复杂，小编精力有限，本文档组织形式可能较为混乱，也难以囊括各编辑者所需要的一切信息。欢迎各位编辑者一起补充、完善本文档！

## TL;DR
![工作流示意图](workflow.drawio.svg)

## MediaWiki 简介
目前，缺氧中文 wiki 搭建在 Fandom 维基农场上，其底层软件为 MediaWiki。简单来说，wiki 的绝大多数功能都是由 MediaWiki 提供的。它负责数据的存储、读出和解析等等。严格来说，MediaWiki 负责了站点内部的所有底层工作流，如解析 wikitext 等。本文档不会介绍这些底层工作。关于 MediaWiki 的更多信息，可参见 [Mediawiki.org](https://mediawiki.org)。

> 小知识：MediaWiki 是为 Wikipedia 项目开发的软件，也是（几乎？）所有维基农场使用的维基软件。其他的维基软件有 [TWiki](https://en.wikipedia.org/wiki/TWiki) 等等。各个维基软件之间差异极大。

## 资源分布简介
缺氧 wiki 的相关资源，按是否储存在[站点](oxygennotincluded.fandom.com/zh)内可粗略分为**站内资源**和**站外资源**。一般而言，**站内资源**是所有人都可直接看到并修改的，而**站外资源**通常由个人或团体开发，不总是面向所有人开放。

## 站内资源
**站内资源**指的是直接储存在站点上（即 Fandom 服务器中）、对使用者可见的资源（即不包括仅服务器管理者可见的内部文件，比如配置文件等）。它们在底层由站点的 MediaWiki 软件管理。站内资源主要包括：

- 文案：wiki的核心内容。
- 图片：主要为游戏贴图。
- 模板/模块：用于辅助页面的渲染/内容生成。
- 其他资源：如 JavaScript 脚本、CSS 等。

作为使用 MediaWiki 的 wiki，缺氧 wiki 的站内资源均以页面的形式呈现。例如，图片通常位于“File:”命名空间，模板和模块位于“Template:”和“Module:”命名空间。MediaWiki 也包含版本控制功能，所有页面的历史版本均有保存。

下文中关于站内资源的导出方式，主要参照了 MediaWiki.org 的相关页面，如 [mw:Manual:Backing_up_a_wiki](https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki)。

### 文案
作为使用 MediaWiki 的 wiki，缺氧 wiki 的文案均以 wikitext 格式撰写。wikitext 可由 MediaWiki 解析为 HTML，进而显示在网页上。所有人均可阅读任一页面的文案，有权限者可编辑文案。

> 小提示：wikitext 是 MediaWiki 特有的标记语言。

> 小提示：编辑权限是可设置项。在目前（2023.1），缺氧中文 wiki 允许所有人编辑所有页面（除个别保护页面外），包括未注册用户。

> 小提示：具体地说，文案内容是 MediaWiki 数据库（database）的一部分。MediaWiki 数据库负责储存站点的页面内容、用户信息、元信息等。

MediaWiki 提供了页面内容导出功能（位于特殊页面“[Special:导出页面](https://oxygennotincluded.fandom.com/zh/wiki/Special:Export)”），能够以 XML 格式导出给定页面列表的**内容**（包括文案和其他页面信息，如历史版本）。不过，据称此方法一次最多只能导出 35 个页面（见[此](https://www.mediawiki.org/wiki/Manual:Parameters_to_Special:Export)），且 XML 文件大小不超过 1.8MB（见[此](https://community.fandom.com/zh/wiki/Help:導出頁面)）。

> 关于导出页面功能，可以参见[此页面](https://www.mediawiki.org/wiki/Help:Export)

MediaWiki 亦以 php 脚本的形式内置了批量导出全站点页面内容的功能，但需要能够访问服务器 Shell 才能运行，因此建立于维基农场上的 wiki 无法使用此功能。因此，一般而言批量导出所有页面内容需要使用（第三方）工具，如 [MediaWiki Dump Generator](https://github.com/mediawiki-client-tools/mediawiki-dump-generator)（可参见[此页面](https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki)）。Fandom 在 [Special:统计](https://oxygennotincluded.fandom.com/zh/wiki/Special:Statistic)页面提供了一键导出所有页面内容的功能（Fandom 称为**数据库转储**）。

### 图片
如上所述，站内图片作为 MediaWiki 文件系统的一部分也储存在站点服务器内。浏览者可以自由上传、浏览和下载站内图片。这些图片可被文案调用，进而显示在页面上。

> 小提示：站内图片、软件设置文件、MediaWiki 扩展等，储存在 MediaWiki 文件系统（File System）中。[此页面](https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki)简略提到了 MediaWiki 的数据库与文件系统。

MediaWiki 没有内置的图片批量导出功能。批量导出图片需要使用第三方工具或自行制作的脚本。

### 模板/模块
**模板**（Template）是一段 wikitext，一般是多个页面同时使用的同质内容。通过在这些页面中调用同一个模板，可以达到简化文案，增加文案可读性的作用。

**模块**（Module）是一个（Lua）程序。在启用了 MediaWiki 的 [Scribunto](https://www.mediawiki.org/wiki/Extension:Scribunto) 扩展后，可以在 wikitext 中调用编程语言程序（目前只支持 Lua）来辅助生成页面内容。模块之间也可以互相调用。

### 其他资源
其他资源包括自定义 Javascript、CSS、由 MediaWiki 调用的页面等。

## 站外资源
截至目前（2023.1），站外资源主要有以下几个项目/组成部分：

- BotNotIncluded：由 DDElephant 开发的项目，主要包含以下功能：
    - 读取部分游戏数据，转化为 Lua 格式后上传至站内模块
    - 读取站内图片并备份至 oni-zh-cn@outlook.com 的 OneDrive 中
    - 比对中、英文站点，记录潜在的过时页面和有误的跨语言链接
    - 修正站内文案格式
- （其他站外项目）（WIP）
- 其他站外内容：主要包括被直接调用而未导入到站内的站外脚本，比如 Lua 模块、JS 脚本和 CSS 文件等。

> 小提示：现阶段（2023.1），一部分站外脚本来自于 [Fandom dev wiki](https://dev.fandom.com/wiki/Fandom_Developers_Wiki)。

## 站内工作流
（WIP）

## 站外工作流
### 游戏数据提取

> 解包游戏资产，提取出图片、表格和游戏内数据

![工作流示意图](workflow_data_extract.drawio.svg)



### wiki机器人

> 管理wiki站点的页面、数据库和代码文件

部分自动化代码托管至Github Actions

![工作流示意图](workflow_wiki_bot.drawio.svg)

