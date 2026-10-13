# REA 重建依据与覆盖范围

日期：2026-10-13。新版：https://arsnick.github.io/beisen/rea-v2.html 。

本次真实调用 REA MCP，而不是仅采用其思路。流程为：公开页面运行时采集 → 从采集结果导出脚本 → 选定第一方脚本静态分析 → 对照官方公开截图独立实现 → 使用 REA 对新原型执行浏览器验收。

## 研究调用

| REA 工具 | 观察 | Evidence ID |
| --- | --- | --- |
| capture_browser_scenario | 官网 DOM、可访问性、截图和网络资源；部分请求在采集结束时未完成 | ev_cadbf44aac033eeb4f55f89f62ca9a778518aaf5e580ab96aca87950c2f77cd4 |
| export_web_scripts | 导出已经采集的脚本，记录摘要；没有运行导出的代码 | ev_b32493fd953630f6117f993c31a4b0ede42e25be127460c70b35df2afeb9524a |
| analyze_javascript_application | 6 个选定文件、60,668 字节、8,365 AST 节点、0 解析失败；语义覆盖 partial / unknown，2,476 项语义未知 | ev_12605cf6ea5047c83bac3630108fa7d8638f497b5cb88cee1741aa5c4c269959 |
| capture_browser_scenario | 公开 Demo 入口是预约演示表单，未填写或提交 | ev_d43031a739708ab59d6c50f57dc8275ba54c0edd4f7dba24dd86c8b54dcaacf5 |

第一方静态分析范围：官网 bs.js、header.js、public.js、c-public.js、scroll.js，以及提取合并的内联脚本。它们解释的是营销网页，不能用于推断真实学习后台的接口、数据库或权限。完整采集中的第三方统计脚本没有加入重建产品。

## 公开界面依据

- [学习产品总介绍](https://www.beisen.com/product/elearning/)：模块范围与五类 AI 学习助手。
- [培养计划公开截图](https://www.beisen.com/public/mobile/res_image0415/zuoshenmeimg1.png)：阶段任务、简介与目录、进度、结业证书、资料和排名布局。
- [岗位学习公开截图](https://www.beisen.com/public/mobile/res_image0415/cexueimfaz1-new.jpg)：阶段、关联能力和 P1–P15 任职序列。
- [师资认证公开截图](https://www.beisen.com/public/mobile/res_image0415/goujieximg1-new.jpg)：活动列表、三步配置抽屉，以及报名、试讲、审批、证书开关。
- [公开演示入口](https://www.beisen.com/demo/elearning)：预约表单，不能进入业务后台。

截图来源在官网采集 DOM 中可核对。本版重建其结构与视觉语言，没有声称逐像素还原某一真实租户。历史视频版本的蓝色侧栏与当前截图不同，本版统一采用公开白灰界面的方向。

## 独立实现与未知项

认证状态流转、80 分试讲门槛、顺序解锁、演示证书、积分、操作记录，以及 UI 中的演示业务数据均由本项目实现。复用了旧原型课程、考试等已存在的基础交互，未导入北森营销脚本作为应用代码。

未取得真实登录页面后的业务系统、API、服务端规则、权限矩阵或数据库；AI 是本地流程模拟，证书是演示记录，操作记录可在浏览器修改，不是生产审计。自助报名之外的管理员批量安排、失败后重新申请、多学员并发审批等仍未实现。原始 REA Evidence bundle 留存本地，不向公开仓库上传网络记录。
