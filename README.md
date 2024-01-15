# ONI zh Wiki Workflow——Technical details in nutshells
本文档旨在总结[缺氧中文wiki](https://oni.fandom.com/zh)的组成架构，介绍部分技术细节，以供编辑者参考。

由于内容架构复杂，小编精力有限，本文档组织形式可能较为混乱，也难以囊括各编辑者所需要的一切信息。欢迎各位编辑者一起补充、完善本文档！

## 资源分布简介
缺氧 wiki 的相关资源，按是否储存在[站点](oxygennotincluded.fandom.com/zh)内可粗略分为**站内资源**和**站外资源**。一般而言，**站内资源**是所有人都可直接看到并修改的，而**站外资源**通常由个人或团体开发，不总是面向所有人开放。

## 站内资源
站内资源主要包括：

- 文案：wiki的核心内容。
- 图片：主要为游戏贴图。
- 模板/模块：用于辅助页面的渲染/内容生成。
- 其他资源：如 JavaScript 脚本、CSS 等。

作为使用 MediaWiki 的 wiki，缺氧 wiki 的站内资源均以页面的形式呈现。例如，图片通常位于“File:”命名空间，模板和模块位于“Template:”和“Module:”命名空间。MediaWiki 也包含版本控制功能，所有页面的历史版本均有保存。

下文中关于站内资源的导出方式，主要参照了 MediaWiki.org 的相关页面，如 [mw:Manual:Backing_up_a_wiki](https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki)。

### 文案
作为使用 MediaWiki 的 wiki，缺氧 wiki 的文案均以 wikitext 格式撰写，储存在站点上（即 Fandom 服务器中）。wikitext 可由 MediaWiki 解析为 HTML，进而显示在网页上。

> 小提示：具体地说，文案内容是 MediaWiki 数据库（database）的一部分。MediaWiki 数据库负责储存站点的页面内容、用户信息、元信息等。另一部分重要文件，如图片、软件设置文件、MediaWiki 扩展等，储存在 MediaWiki 文件系统（File System）中。可参见[此处](https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki)。

MediaWiki 提供了在站点上直接编辑任意页面的功能。所有人均可通过此功能阅读、编辑（受保护页面除外）单个页面的文案。

MediaWiki 提供了页面内容导出功能（位于特殊页面“[Special:导出页面](https://oxygennotincluded.fandom.com/zh/wiki/Special:Export)”），能够以 XML 格式导出给定页面列表的**内容**（包括文案和其他页面信息，如历史版本）。不过，据称此方法一次最多只能导出 35 个页面（见[此](https://www.mediawiki.org/wiki/Manual:Parameters_to_Special:Export)），且 XML 文件大小不超过 1.8MB（见[此](https://community.fandom.com/zh/wiki/Help:導出頁面)）。

> 关于导出页面功能，可以参见[此页面](https://www.mediawiki.org/wiki/Help:Export)

MediaWiki 亦以 php 脚本的形式内置了批量导出全站点页面内容的功能，但需要能够访问服务器 Shell 才能运行，因此建立于维基农场上的 wiki 无法使用此功能。因此，一般而言批量导出所有页面内容需要使用（第三方）工具，如 [MediaWiki Dump Generator](https://github.com/mediawiki-client-tools/mediawiki-dump-generator)（可参见[此页面](https://www.mediawiki.org/wiki/Manual:Backing_up_a_wiki)）。Fandom 在 [Special:统计](https://oxygennotincluded.fandom.com/zh/wiki/Special:Statistic)页面提供了一键导出所有页面内容的功能（Fandom 称为**数据库转储**）。

## 站外资源
截至目前（2023.1），站外资源主要有以下几个项目/组成部分：

- BotNotIncluded：由 DDElephant 开发的项目，主要包含以下功能：
    - 读取部分游戏数据，转化为 Lua 格式后上传至站内模块
    - 读取站内图片并备份至 oni-zh-cn@outlook.com 的 OneDrive 中
    - 比对中、英文站点，记录潜在的过时页面和有误的跨语言链接
    - 修正站内文案格式