# 站点说明

**线上地址：<https://woyongyuanxihuanbanya-cpu.github.io/pathfinder-v2.24-sc/>**

本目录是 **Pathfinder v2.24 SC + 速查** 的静态网站版本（由 `Pathfinder-v2.24-SC-Compendium.chm` 转换而来），
可直接作为 GitHub Pages 站点发布。

## 结构

| 文件 | 说明 |
|---|---|
| `index.html` | 站点外壳：顶部导航 + 左侧目录 + 右侧内容（iframe 双栏，模仿 CHM 查看器） |
| `toc.html` | 由 CHM 的 `.hhc` 生成的目录树（2406 节点，可折叠、可过滤，点击在右栏打开） |
| `welcome.html` | 首页：三套速查入口与使用说明 |
| `spellindex.html` + `class-*.html` | **万法大全速查**：1919 条法术 + 27 个职业速查页 |
| `featindex.html` | **专长大全速查**：1226 条专长 |
| `creatureindex.html` | **生物大全速查**：379 条生物 |
| `*.lookup.html` | 21 个原文伴生页（原文逐字节副本 + 条目锚点） |
| 其余 `.html` / `.htm` | 原书全部页面（2148 个），内容一字未改 |

## 与原 CHM 的差异（仅编码与内部文件名）

1. **编码**：全部页面由 GBK 转为 **UTF-8**。GitHub Pages 对 `.html` 一律以
   `Content-Type: text/html; charset=utf-8` 响应，HTTP 头优先于页面内的 `<meta charset>`，
   若保持 GBK 会整页乱码，故必须转码；文字内容不变，且解码无一字节替换字符（U+FFFD = 0）。
2. **内部文件名**：含中文的 706 个文件名改为 ASCII（`cjk_0001.htm` 等）。
   原因：`chmcmd` 在「ASCII 名 + 中文名」混排时会产出无法打开的 CHM（已实测复现）。
   站内引用已同步改写，**目录标题与页面内容保持不变**。
3. 工程文件（`.hhc` / `.hhk` / `.hhp`）不发布。

## 已知的原有断链

站内有 1540 处链接指向不存在的文件，**这些在原始 CHM 与原解包里同样不存在**，属原页面自带的
Word 导出元数据引用（`_template.css`、`filelist.xml`、`themeData.thmx`、`preview.wmf`、
`clip_image00N.*` 等）以及 3 条原目录的悬空条目（`$$unsavedpage1.htm`、`$$unsavedpage2.htm`、`page_1373.html`）。
本次转换未新增任何断链，站内链接 **0 处大小写不一致**（GitHub Pages 大小写敏感）。

## 版权

内容来自 Pathfinder 中文合集与原译者，版权归原作者与译者所有，仅供个人查阅使用。

## 重新生成

```powershell
$env:PF_PROFILE='pf224'
node ..\_build\chm\build-web.js        # 生成/更新本目录
node ..\_build\chm\verify-web.js       # 链接与大小写校验
node ..\_build\chm\deploy-pages.js --owner=<用户> --repo=<仓库>   # 推送并开启 Pages
```

## 已部署信息

| 项目 | 值 |
|---|---|
| GitHub 仓库 | `woyongyuanxihuanbanya-cpu/pathfinder-v2.24-sc`（public） |
| Pages 源 | `main` 分支根目录，`.nojekyll` 已放置（避免 Jekyll 忽略文件） |
| 首次构建 | Pages API 状态 `built`；10/10 探活 HTTP 200；线上内容校验 9/9 通过（合法 UTF-8、0 替换字符） |
| 站点体积 | 302.5 MB（2201 个文件；Pages 站点上限 1 GB，单文件上限 100 MB，均在限内） |

更新站点：改完文件后在本目录执行 `git add -A; git commit -m "更新"; git push`，Pages 会自动重建（约 1–2 分钟）。

