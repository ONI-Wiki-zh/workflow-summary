# ONI zh Wiki Workflow——Technical details in nutshells
本文档旨在总结[缺氧中文wiki](https://oni.fandom.com/zh)的组成架构，介绍部分技术细节，以供编辑者参考。

由于内容架构复杂，小编精力有限，本文档组织形式可能较为混乱，也难以囊括各编辑者所需要的一切信息。欢迎各位编辑者一起补充、完善本文档！

## 资源分布简介
缺氧 wiki 的相关资源，按是否储存在[站点](oxygennotincluded.fandom.com/zh)内可粗略分为**站内资源**和**站外资源**。一般而言，**站内资源**是所有人都可直接看到并修改的，而**站外资源**通常由个人或团体开发，不总是面向所有人开放。

### 站内资源
站内资源主要包括：

- 文案：wiki的核心内容。
- 图片：主要为游戏贴图。
- 模板/模块：用于辅助页面的渲染/内容生成。

作为使用 MediaWiki 架构的 wiki，缺氧 wiki 的站内资源均以页面的形式呈现。例如，图片通常位于“File:”命名空间，模板和模块位于“Template:”和“Module:”命名空间。

### 站外资源

## 站点内容/资源构成
本站站点主要由以下内容/资源构成：


- 其他依赖文件：包括CSS、JS文件等，以及

目前，这些资源的改动/获取方式分别是：
- 文案内容：以wikitext格式直接储存在站点（Fandom服务器）上，可从站点直接获取和改动。可使用[Special:导出页面](https://oxygennotincluded.fandom.com/zh/wiki/Special:Export)功能以及[Special:统计](https://oxygennotincluded.fandom.com/zh/wiki/Special:Statistic)中的数据库转储功能批量导出。
- 图片内容：直接储存在站点上，可以直接在站点获取和上传。同时由BotNotIncluded项目不定期（*当前不可用*）备份在oni-zh-cn@outlook.com的OneDrive上。批量导出方法未知。
- 模板：以wikitext格式直接储存在站点上，