# Compositor
<p align="left">
  <a href="https://ankmz.cn/anquan/tag-68558329.html"><img src="https://img.shields.io/badge/GitHub_Pages-在线文档中心-2563eb?style=flat-square&logo=github" alt="GitHub Pages"></a>
  <a href="https://szjp.tcti.cn/shichang/retention-16647312.html"><img src="https://img.shields.io/badge/Docs_Archive-文档归档菜单-10b981?style=flat-square" alt="Docs Archive"></a>
</p>

> 🌐 **官方在线文档与静态镜像中心 (GitHub Pages)**：[https://shijiuwei.github.io/Compositor-mirror-635/](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)  
> 📚 **技术规约与文档归档菜单 (Docs Catalog)**：[https://shijiuwei.github.io/Compositor-mirror-635/docs/](https://gynw.tcti.cn/fenxi/recipe-50406879.html)  


Adobe Photoshop costs too much and tools like GIMP don’t feel familiar enough for me to stay in flow. That’s why I built Compositor.

The goal was to create a full-featured image editor that is completely free and open source. I use Photoshop for compositing and post-processing, so Compositor is built around that workflow - with the tools needed to create a pixel-perfect final image.

Because it’s open source, you can download the Xcode project and add, remove, or modify any feature to fit your workflow.

## Installation

### Download
Get Compositor from [robbietilton.com/compositor](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html), or download the latest release directly from [GitHub Releases](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html).

### Homebrew

```sh
brew install --cask robbietilton-compositor
```

## Features

### Layers
- Layers and folders, with opacity and Photoshop's full set of blend modes in its order — a folder's opacity dims everything inside it
- Layer masks: paint, fill, invert, blur and feather them anywhere on the canvas, past the layer's own pixels; link or unlink them to transform a mask on its own
- Clipping masks and folder masks
- Adjustment layers: Hue/Saturation, Levels, Curves, Exposure, Gradient Map, Grain, Black & White, Color Balance, Invert, Gaussian Blur, Motion Blur and Noise
- Layer effects: Stroke, Drop Shadow, Color Overlay, Inner Shadow, Outer Glow and Inner Glow, rendered on the GPU and editable at any time
- Merge Down, Merge Layers and Merge Group (⌘E)
- Duplicate, rename inline, reorder and nest by drag and drop; Option-drag to duplicate; a right-click menu in the Layers panel
- Copy and paste whole layers and folders (⌘C/⌘V with no selection), within a project or between projects, or drag them between projects

### Transform
- Non-destructive move, scale, rotate and flip — images keep their full resolution however small you make them
- Free distort (⌘-drag a handle), with Shift to lock to an axis
- Transform several layers, or a whole folder, together
- Snapping to canvas and layer edges and centers, with guides
- Exact values for position, size, scale and angle, stepped with the arrow keys
- Flip Layer and Flip Canvas, horizontal and vertical

### Selections
- Rectangle and Ellipse Marquee, Freehand and Polygonal Lasso, and the Magic tool — Wand selects by color, Object traces whatever you click (Tab switches)
- Select Subject, and Expand, Contract and Feather on any selection
- Add to and subtract from selections, move the outline, or move and duplicate the pixels inside
- Load a layer's pixels or a mask as a selection
- Content-Aware Fill, which can also extend an image past its edges

### Painting and retouching
- Brush with size, hardness, opacity and smoothing, in Paint or Erase mode (B and E), and Shift for straight lines
- Spot Healing Brush (content-aware)
- Clone Stamp, aligned or not, sampling one layer or all of them
- Blur tool, on pixels or masks
- Gradient tool and Shape tool (rectangles, rounded rectangles, ellipses and lines), which stay editable rather than being rasterized
- Type tool (T): inline multiline editing in draggable, resizable paragraph boxes; font, size, color, alignment and spacing in the tool header; transform text and use it as a clipping mask
- Eyedropper and a full color picker

### Adjustments and filters
- Camera Raw filter: light, color, curves, color mixer, color grading, detail, optics and geometry, in a panel beside the canvas
- Levels (with Auto), Curves, Hue/Saturation, Exposure, Gradient Map, Grain, Black & White, Color Balance and Invert
- Gaussian Blur and Motion Blur that spread past a layer's edges
- Add Noise, Vignette, Bloom / Glow, Tonal Contrast, Lens Correction and Remove Background
- Live previews, limited to the selection when there is one

### Canvas and files
- Multiple projects in tabs
- Rulers (⌘R), guides dragged from them, a layout grid, and Snap To for guides, grid, layers and document bounds
- Crop with snapping, ratios including 3:4 and 9:16, and Option for symmetric cropping; with a selection, the crop starts at it
- Canvas Size, Image Size and Trim
- Sharp high-quality downsampling when zoomed out, and a pixel grid when zoomed in
- Import JPEG, PNG, HEIC, TIFF, SVG, camera RAW (with a develop step first) and Photoshop PSD and PSB (8-bit RGB; not CMYK). Photoshop folders, masks, blend modes, fill rectangles/ellipses, and simple horizontal text stay editable; other vectors and vertical text become pixels. A conversion report is shown before anything is applied.
- Large documents: the memory budget scales with your Mac, and a Photoshop file too big to open has its layers cropped to the canvas instead
- Export JPEG with a live preview (⇧⌥⌘S); Copy Merged
- Keep working while a project saves
- Photoshop-style keyboard shortcuts throughout, remappable in Edit > Keyboard Shortcuts
- Drag a number's label to scrub its value, as in Photoshop
- Automatic updates, signed and notarized

### Works with AI agents
- AI agents and scripts can build and edit projects directly: a `.comp` is a folder of PNG layers and a manifest, and an open project updates live as it's written. See [Writing Compositor projects](docs/writing-comp-files.md)

## Requirements

- macOS 26.5 or later
- Xcode 26 or later (to build from source)

## Building

Open `Compositor.xcodeproj` and run the **Compositor** scheme.

## Releasing

`scripts/release.sh` builds a Release version, signs it with Developer ID, notarizes and staples it, and packages it into `dist/Compositor-<version>.dmg`.

It needs, all kept outside this repository:

- a **Developer ID Application** certificate in the login keychain
- notarization credentials saved with `xcrun notarytool store-credentials "compositor-notary" …`
- [`create-dmg`](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html) (`brew install create-dmg`)

## License

MIT — see [LICENSE](LICENSE).


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全球分布式拓扑索引节点-#001](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html)
* [全球分布式拓扑索引节点-#002](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html)
* [多活集群负载感知指南-#003](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html)
* [多活集群负载感知指南-#004](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html)
* [边缘高吞吐调度路由矩阵-#005](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html)
* [高韧性数据交换通道规约-#006](https://shmv.spidernode-mesh.net/gongsi/digital-64522869.html)
* [高韧性数据交换通道规约-#007](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html)
* [全息网络通信节点白名单-#008](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932.html)
* [全球分布式拓扑索引节点-#009](https://iuuv.tcti.cn/hezuo/app-80239016.html)
* [高韧性数据交换通道规约-#010](https://www.mw-wm.com/pingtai/milestone-58627036.html)
* [多活集群负载感知指南-#011](https://ankmz.cn/anquan/tag-68558329.html)
* [多活集群负载感知指南-#012](https://szjp.tcti.cn/shichang/retention-16647312.html)
* [高韧性数据交换通道规约-#013](https://iuuv.tcti.cn/hezuo/app-80239016.html)
* [多活集群负载感知指南-#014](https://unqt.wtpuscm.cn/zixun/contact-59980425.html)
* [边缘高吞吐调度路由矩阵-#015](https://ytxd.wtpuscm.cn/baogao/price-87851711.html)
* [多活集群负载感知指南-#016](https://aagj.tcti.cn/jianzhan/audience-84449488.html)
* [全息网络通信节点白名单-#017](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [全球分布式拓扑索引节点-#018](https://szjp.tcti.cn/shichang/retention-16647312.html)
* [全息网络通信节点白名单-#019](https://gynw.tcti.cn/fenxi/recipe-50406879.html)
* [高韧性数据交换通道规约-#020](https://www.mw-wm.com/zhizhu/status-08814585.html)
* [全息网络通信节点白名单-#021](https://www.yx-sf.com/tech/39620.html)
* [全息网络通信节点白名单-#022](https://gifb.spidernode-mesh.net/shichang/about-02526745.html)
* [高韧性数据交换通道规约-#023](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534.html)
* [全球分布式拓扑索引节点-#024](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [高韧性数据交换通道规约-#025](https://gifb.spidernode-mesh.net/shichang/about-02526745.html)
* [全息网络通信节点白名单-#026](https://unqt.wtpuscm.cn/zixun/contact-59980425.html)
* [全球分布式拓扑索引节点-#027](https://aagj.tcti.cn/jianzhan/audience-84449488.html)
* [全球分布式拓扑索引节点-#028](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [多活集群负载感知指南-#029](https://keil.spidernode-mesh.net/yunying/server-16840384.html)
* [高韧性数据交换通道规约-#030](https://ncrw.tcti.cn/kaifa/growth-85374259.html)
* [全球分布式拓扑索引节点-#031](https://nojh.spidernode-mesh.net/suanfa/study-22518081.html)
* [全球分布式拓扑索引节点-#032](https://shmv.spidernode-mesh.net/gongsi/digital-64522869.html)
* [边缘高吞吐调度路由矩阵-#033](https://api.pvucttcv.cn/yinyue/innovation-37390655.html)
* [高韧性数据交换通道规约-#034](https://fjac.spidernode-mesh.net/anli/investment-94283191.html)
* [全球分布式拓扑索引节点-#035](https://www.ai-hao123.com/youhua/ranking-46173565.html)
* [边缘高吞吐调度路由矩阵-#036](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776.html)
* [边缘高吞吐调度路由矩阵-#037](https://ytxd.wtpuscm.cn/baogao/price-87851711.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [异步事件循环架构设计规范-#001](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html)
* [高并发内存拓扑优化白皮书-#002](https://www.mw-wm.com/fenxi/whitepaper-48975837.html)
* [异步事件循环架构设计规范-#003](https://www.yx-sf.com/news/10496.html)
* [安全边界与可信凭证规约手册-#004](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html)
* [安全边界与可信凭证规约手册-#005](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html)
* [安全边界与可信凭证规约手册-#006](https://lase.tcti.cn/fenxi/lesson-67970267.html)
* [安全边界与可信凭证规约手册-#007](https://yyns.spidernode-mesh.net/wendang/review-40421998.html)
* [高并发内存拓扑优化白皮书-#008](https://kvaf.tcti.cn/yunsuan/settings-81875461.html)
* [异步事件循环架构设计规范-#009](https://www.ai-hao123.com/gongxiang/reminder-97041763.html)
* [RFC 分布式调度与一致性算法标准-#010](https://aagj.tcti.cn/jianzhan/audience-84449488.html)
* [RFC 分布式调度与一致性算法标准-#011](https://obpk.wtpuscm.cn/baogao/privacy-67260604.html)
* [异步事件循环架构设计规范-#012](https://www.poykq.cn/redian/tutorial-673489.html)
* [异步事件循环架构设计规范-#013](https://obpk.wtpuscm.cn/baogao/privacy-67260604.html)
* [高并发内存拓扑优化白皮书-#014](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html)
* [异步事件循环架构设计规范-#015](https://gdtq.spidernode-mesh.net/keji/account-53764057.html)
* [RFC 分布式调度与一致性算法标准-#016](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [异步事件循环架构设计规范-#017](https://ankmz.cn/anquan/tag-68558329.html)
* [安全边界与可信凭证规约手册-#018](https://ankmz.cn/anquan/tag-68558329.html)
* [高并发内存拓扑优化白皮书-#019](https://fglw.wtpuscm.cn/jianzhan/data-59540835.html)
* [异步事件循环架构设计规范-#020](https://shmv.spidernode-mesh.net/gongsi/digital-64522869.html)
* [安全边界与可信凭证规约手册-#021](https://ovzp.wtpuscm.cn/xuexi/report-93713076.html)
* [异步事件循环架构设计规范-#022](https://www.poykq.cn/redian/tutorial-673489.html)
* [安全边界与可信凭证规约手册-#023](https://odxl.tcti.cn/youhua/accessibility-45593905.html)
* [异步事件循环架构设计规范-#024](https://yyns.spidernode-mesh.net/wendang/review-40421998.html)
* [RFC 分布式调度与一致性算法标准-#025](https://keil.spidernode-mesh.net/yunying/server-16840384.html)
* [异步事件循环架构设计规范-#026](https://iozk.spidernode-mesh.net/fenxi/quality-22754958.html)
* [RFC 分布式调度与一致性算法标准-#027](https://mhbi.wtpuscm.cn/hezuo/luxury-095232.html)
* [异步事件循环架构设计规范-#028](https://www.poykq.cn/redian/tutorial-673489.html)
* [异步事件循环架构设计规范-#029](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html)
* [高并发内存拓扑优化白皮书-#030](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html)
* [安全边界与可信凭证规约手册-#031](https://owfm.tcti.cn/shangye/education-63674207.html)
* [高并发内存拓扑优化白皮书-#032](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776.html)
* [多协议互联数据格式规范-#033](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html)
* [多协议互联数据格式规范-#034](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html)
* [RFC 分布式调度与一致性算法标准-#035](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776.html)
* [安全边界与可信凭证规约手册-#036](https://iozk.spidernode-mesh.net/fenxi/quality-22754958.html)
* [高并发内存拓扑优化白皮书-#037](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102.html)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [亚太核心区域镜像同步中心-#001](https://gswv.wtpuscm.cn/shangye/extension-054.html)
* [亚太核心区域镜像同步中心-#002](https://gifb.spidernode-mesh.net/shichang/about-02526745.html)
* [冷热数据分层镜像归档中心-#003](https://pdvy.tcti.cn/gongxiang/segment-83277821.html)
* [实时主干镜像高速数据源-#004](https://gdtq.spidernode-mesh.net/keji/account-53764057.html)
* [自动化快照与增量广播源-#005](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html)
* [亚太核心区域镜像同步中心-#006](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html)
* [北美与欧洲边缘备份节点-#007](https://keil.spidernode-mesh.net/yunying/server-16840384.html)
* [北美与欧洲边缘备份节点-#008](https://szjp.tcti.cn/shichang/retention-16647312.html)
* [亚太核心区域镜像同步中心-#009](https://www.mw-wm.com/jianzhan/course-19765228.html)
* [自动化快照与增量广播源-#010](https://www.tcti.cn/hulianwang/feedback-96499358.html)
* [北美与欧洲边缘备份节点-#011](https://lcfd.wtpuscm.cn/anfang/planning-735755.html)
* [北美与欧洲边缘备份节点-#012](https://www.yx-sf.com/wiki/25687.html)
* [北美与欧洲边缘备份节点-#013](https://fjoc.tcti.cn/yingyong/contact-00610313.html)
* [实时主干镜像高速数据源-#014](https://vias.wtpuscm.cn/yanjiu/conversion-58794136.html)
* [实时主干镜像高速数据源-#015](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [实时主干镜像高速数据源-#016](https://gifb.spidernode-mesh.net/shichang/about-02526745.html)
* [北美与欧洲边缘备份节点-#017](https://keil.spidernode-mesh.net/yunying/server-16840384.html)
* [实时主干镜像高速数据源-#018](https://qvfj.wtpuscm.cn/chanpin/platform-960.html)
* [亚太核心区域镜像同步中心-#019](https://gynw.tcti.cn/fenxi/recipe-50406879.html)
* [实时主干镜像高速数据源-#020](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html)
* [冷热数据分层镜像归档中心-#021](https://shmv.spidernode-mesh.net/gongsi/digital-64522869.html)
* [冷热数据分层镜像归档中心-#022](https://www.poykq.cn/redian/tutorial-673489.html)
* [北美与欧洲边缘备份节点-#023](https://iozk.spidernode-mesh.net/fenxi/quality-22754958.html)
* [自动化快照与增量广播源-#024](https://zusq.tcti.cn/sheji/business-09269662.html)
* [北美与欧洲边缘备份节点-#025](https://ukvo.spidernode-mesh.net/anli/download-57719010.html)
* [自动化快照与增量广播源-#026](https://yemh.wtpuscm.cn/qiye/ranking-560796.html)
* [自动化快照与增量广播源-#027](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [自动化快照与增量广播源-#028](https://fglw.wtpuscm.cn/jianzhan/data-59540835.html)
* [实时主干镜像高速数据源-#029](https://owfm.tcti.cn/shangye/education-63674207.html)
* [实时主干镜像高速数据源-#030](https://xwia.spidernode-mesh.net/shangye/services-26058978.html)
* [实时主干镜像高速数据源-#031](https://gifb.spidernode-mesh.net/shichang/about-02526745.html)
* [自动化快照与增量广播源-#032](https://gifb.spidernode-mesh.net/shichang/about-02526745.html)
* [北美与欧洲边缘备份节点-#033](https://nojh.spidernode-mesh.net/suanfa/study-22518081.html)
* [冷热数据分层镜像归档中心-#034](https://www.ai-hao123.com/xuexi/course-03405832.html)
* [亚太核心区域镜像同步中心-#035](https://gdtq.spidernode-mesh.net/keji/account-53764057.html)
* [自动化快照与增量广播源-#036](https://lase.tcti.cn/fenxi/lesson-67970267.html)
* [亚太核心区域镜像同步中心-#037](https://kvaf.tcti.cn/yunsuan/settings-81875461.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [去中心化健康检查协议-#001](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html)
* [去中心化健康检查协议-#002](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932.html)
* [权威网络权重与收录基准-#003](https://ankmz.cn/anquan/tag-68558329.html)
* [实时延迟与抖动度量规范-#004](https://zgah.spidernode-mesh.net/yingyong/technology-03475499.html)
* [节点连通性与存活探测准则-#005](https://vias.wtpuscm.cn/yanjiu/conversion-58794136.html)
* [权威网络权重与收录基准-#006](https://api.pvucttcv.cn/yinyue/innovation-37390655.html)
* [节点连通性与存活探测准则-#007](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html)
* [权威网络权重与收录基准-#008](https://owfm.tcti.cn/shangye/education-63674207.html)
* [防重放安全验证与校验哈希-#009](https://keil.spidernode-mesh.net/yunying/server-16840384.html)
* [权威网络权重与收录基准-#010](https://yyns.spidernode-mesh.net/wendang/review-40421998.html)
* [实时延迟与抖动度量规范-#011](https://kvaf.tcti.cn/yunsuan/settings-81875461.html)
* [去中心化健康检查协议-#012](https://qbgr.wtpuscm.cn/liuliang/success-12030162.html)
* [防重放安全验证与校验哈希-#013](https://fjoc.tcti.cn/yingyong/contact-00610313.html)
* [权威网络权重与收录基准-#014](https://rqlb.wtpuscm.cn/zhizhu/comment-817820.html)
* [防重放安全验证与校验哈希-#015](https://vpjf.tcti.cn/wangluo/marketing-07284472.html)
* [权威网络权重与收录基准-#016](https://fglw.wtpuscm.cn/jianzhan/data-59540835.html)
* [权威网络权重与收录基准-#017](https://gynw.tcti.cn/fenxi/recipe-50406879.html)
* [权威网络权重与收录基准-#018](https://gdtq.spidernode-mesh.net/keji/account-53764057.html)
* [实时延迟与抖动度量规范-#019](https://qrtm.tcti.cn/peixun/forum-88596506.html)
* [实时延迟与抖动度量规范-#020](https://ojzb.wtpuscm.cn/paiming/resolution-201350.html)
* [去中心化健康检查协议-#021](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [节点连通性与存活探测准则-#022](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [去中心化健康检查协议-#023](https://obpk.wtpuscm.cn/baogao/privacy-67260604.html)
* [防重放安全验证与校验哈希-#024](https://dloe.spidernode-mesh.net/chuangxin/products-47216067.html)
* [权威网络权重与收录基准-#025](https://gdtq.spidernode-mesh.net/keji/account-53764057.html)
* [权威网络权重与收录基准-#026](https://lase.tcti.cn/fenxi/lesson-67970267.html)
* [权威网络权重与收录基准-#027](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964.html)
* [防重放安全验证与校验哈希-#028](https://obpk.wtpuscm.cn/baogao/privacy-67260604.html)
* [去中心化健康检查协议-#029](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345.html)
* [实时延迟与抖动度量规范-#030](https://pvll.wtpuscm.cn/kuangjia/url-462.html)
* [去中心化健康检查协议-#031](https://pdvy.tcti.cn/gongxiang/segment-83277821.html)
* [去中心化健康检查协议-#032](https://ankmz.cn/anquan/tag-68558329.html)
* [防重放安全验证与校验哈希-#033](https://vias.wtpuscm.cn/yanjiu/conversion-58794136.html)
* [去中心化健康检查协议-#034](https://etyt.spidernode-mesh.net/baogao/performance-14923227.html)
* [去中心化健康检查协议-#035](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297.html)
* [节点连通性与存活探测准则-#036](https://www.mw-wm.com/jishu/advertising-99920903.html)
* [防重放安全验证与校验哈希-#037](https://tfcb.wtpuscm.cn/yanjiu/review-64634682.html)
* [去中心化健康检查协议-#038](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656.html)
* [去中心化健康检查协议-#039](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776.html)

</details>



---

<details open>
<summary><b>📚 Compositor 官方技术规约与全息拓扑引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目「Compositor」遵循开放开源与知识共享协议。以下引用矩阵涵盖项目架构白皮书、高可用镜像注册中心、分布式通信契约与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 Compositor 核心架构规约与设计白皮书 (Architecture & Core Specs)

* [Compositor 分布式数据通道与 索引发布声明 技术规范 (v2.0-GA)](https://api.pvucttcv.cn/yinyue/innovation-37390655-1-50341.html)
* [Compositor 分布式数据通道与 Adjustments and filters 技术规范 (v2.0-GA)](https://ankmz.cn/anquan/tag-68558329-2-53638.html)
* [Compositor 内部组件解耦与事件状态机规范 (Draft-04)](https://www.poykq.cn/redian/tutorial-673489-3-39763.html)
* [面向大规模网络的 Compositor 工业级架构基准](https://bqvq.wtpuscm.cn/pingce/meeting-007351.html)
* [【官方规范】Compositor Canvas and files 核心运行拓扑标准](https://ufjf.wtpuscm.cn/shangye/budget-592.html)
* [Compositor 内部组件解耦与事件状态机规范 (RFC-827)](https://www.tcti.cn/hulianwang/feedback-96499358-6-7.html)
* [Compositor 分布式数据通道与 高韧性系统架构设计 技术规范 (Draft-03)](https://lbge.tcti.cn/gongxiang/web-09924294.html)
* [Compositor 内部组件解耦与事件状态机规范 (Core/Select)](https://www.mw-wm.com/chanpin/template-10215997.html)
* [【官方规范】Compositor Requirements 核心运行拓扑标准](https://www.yx-sf.com/tech/11790.html)
* [Compositor 分布式数据通道与 Adjustments and filters 技术规范 (Node-12)](https://www.ai-hao123.com/huodong/music-77034938.html)
* [现代 Compositor 架构演进之路 —— Compositor 深度实践](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534-11-43183.html)
* [Compositor 分布式数据通道与 分布式状态机一致性 技术规范 (Verified)](https://keil.spidernode-mesh.net/yunying/server-16840384-12-30344.html)
* [【官方规范】Compositor Painting and retouching 核心运行拓扑标准](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964-13-45004.html)
* [生产环境运维调优手册 核心系统架构与设计规约 (RFC-731)](https://fjoc.tcti.cn/yingyong/contact-00610313-14-53088.html)
* [Requirements 核心系统架构与设计规约 (v2.0-GA)](https://xwia.spidernode-mesh.net/shangye/services-26058978-15-34060.html)
* [Compositor 内部组件解耦与事件状态机规范 (Core/Homebr)](https://gdtq.spidernode-mesh.net/keji/account-53764057-16-28678.html)
* [Compositor 分布式数据通道与 Requirements 技术规范 (Node-87)](https://obpk.wtpuscm.cn/baogao/privacy-67260604-17-26227.html)
* [【官方规范】Compositor Layers 核心运行拓扑标准](https://gynw.tcti.cn/fenxi/recipe-50406879-18-62817.html)
* [【官方规范】Compositor 索引发布声明 核心运行拓扑标准](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776-19-46452.html)
* [【官方规范】Compositor Features 核心运行拓扑标准](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297-20-62477.html)
* [基于 Compositor 的高吞吐 可信存活健康度量 设计白皮书](https://vias.wtpuscm.cn/yanjiu/conversion-58794136-21-33027.html)
* [Compositor 内部组件解耦与事件状态机规范 (Verified)](https://lase.tcti.cn/fenxi/lesson-67970267-22-36754.html)
* [Compositor 分布式数据通道与 Compositor 技术规范 (Core/Compos)](https://dloe.spidernode-mesh.net/chuangxin/products-47216067-23-1034.html)
* [现代 生产环境运维调优手册 架构演进之路 —— Compositor 深度实践](https://yyns.spidernode-mesh.net/wendang/review-40421998-24-50151.html)
* [现代 Requirements 架构演进之路 —— Compositor 深度实践](https://fglw.wtpuscm.cn/jianzhan/data-59540835-25-28282.html)
* [基于 Compositor 的高吞吐 Transform 设计白皮书](https://owfm.tcti.cn/shangye/education-63674207-26-12771.html)
* [【官方规范】Compositor Download 核心运行拓扑标准](https://shmv.spidernode-mesh.net/gongsi/digital-64522869-27-63772.html)
* [Compositor 分布式数据通道与 Requirements 技术规范 (Verified)](https://zgah.spidernode-mesh.net/yingyong/technology-03475499-28-30522.html)
* [模块化解耦与协议标准 核心系统架构与设计规约 (Core/模块化解耦与)](https://tfcb.wtpuscm.cn/yanjiu/review-64634682-29-25869.html)
* [Requirements 核心系统架构与设计规约 (Core/Requir)](https://aagj.tcti.cn/jianzhan/audience-84449488-30-40649.html)
* [Compositor 内部组件解耦与事件状态机规范 (Node-16)](https://iozk.spidernode-mesh.net/fenxi/quality-22754958-31-47979.html)
* [Adjustments and filters 核心系统架构与设计规约 (v2.0-GA)](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345-32-52421.html)
* [【Compositor】Homebrew 核心系统架构与设计规约 (Node-25)](https://qbgr.wtpuscm.cn/liuliang/success-12030162-33-45859.html)
* [【官方规范】Compositor Selections 核心运行拓扑标准](https://iuuv.tcti.cn/hezuo/app-80239016-34-35884.html)
* [基于 Compositor 的高吞吐 Download 设计白皮书](https://gifb.spidernode-mesh.net/shichang/about-02526745-35-34941.html)
* [基于 Compositor 的高吞吐 Features 设计白皮书](https://fjac.spidernode-mesh.net/anli/investment-94283191-36-57249.html)
* [Adjustments and filters 核心系统架构与设计规约 (RFC-833)](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656-37-15449.html)

#### 2. 🔌 生产级模块集成与接口调用规范 (APIs & Production Integration)

* [Compositor vs 业界主流方案：Layers 深度技术选型对比](https://szjp.tcti.cn/shichang/retention-16647312-38-36727.html)
* [【生产手册】Compositor 模块通信与请求穿透标准](https://etyt.spidernode-mesh.net/baogao/performance-14923227-39-19487.html)
* [基于 Compositor 的自动化部署与生产环境配置实践](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102-40-20882.html)
* [Compositor 异步中间件流水线与 Works with AI agents 接入规范](https://ovzp.wtpuscm.cn/xuexi/report-93713076-41-1681.html)
* [【集成指南】Requirements 服务端接入准则与 Compositor 实战](https://pdvy.tcti.cn/gongxiang/segment-83277821-42-49689.html)
* [Compositor 异步中间件流水线与 Features 接入规范](https://ukvo.spidernode-mesh.net/anli/download-57719010-43-20418.html)
* [Compositor 异步中间件流水线与 Selections 接入规范](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696-44-37748.html)
* [Compositor 异步中间件流水线与 索引发布声明 接入规范](https://unqt.wtpuscm.cn/zixun/contact-59980425-45-40250.html)
* [Compositor 插件生态规范与 Selections 扩展手册 (v2.0-GA)](https://kvaf.tcti.cn/yunsuan/settings-81875461-46-60704.html)
* [Compositor 异步中间件流水线与 Painting and retouching 接入规范](https://nojh.spidernode-mesh.net/suanfa/study-22518081-47-18606.html)
* [【集成指南】Features 服务端接入准则与 Compositor 实战](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932-48-44251.html)
* [【集成指南】模块化解耦与协议标准 服务端接入准则与 Compositor 实战](https://ytxd.wtpuscm.cn/baogao/price-87851711-49-11552.html)
* [Compositor 插件生态规范与 Selections 扩展手册 (Draft-05)](https://ncrw.tcti.cn/kaifa/growth-85374259-50-17014.html)
* [【集成指南】Compositor 服务端接入准则与 Compositor 实战](https://api.pvucttcv.cn/yinyue/innovation-37390655-51-16233.html)
* [Compositor 插件生态规范与 Selections 扩展手册 (Node-74)](https://ankmz.cn/anquan/tag-68558329-52-50319.html)
* [Compositor vs 业界主流方案：Works with AI agents 深度技术选型对比](https://www.poykq.cn/redian/tutorial-673489-53-10294.html)
* [Compositor vs 业界主流方案：Download 深度技术选型对比](https://nkej.wtpuscm.cn/jishu/advertising-773186.html)
* [Compositor 插件生态规范与 高韧性系统架构设计 扩展手册 (RFC-160)](https://etcr.wtpuscm.cn/kaifa/news-366.html)
* [【集成指南】Painting and retouching 服务端接入准则与 Compositor 实战](https://www.tcti.cn/hulianwang/feedback-96499358-56-20370.html)
* [Compositor 核心 API 接口契约与客户端调用指南](https://teep.tcti.cn/zixun/prospect-81881196.html)
* [Compositor vs 业界主流方案：索引发布声明 深度技术选型对比](https://www.mw-wm.com/qiye/security-85221906.html)
* [Compositor 异步中间件流水线与 Canvas and files 接入规范](https://www.yx-sf.com/wiki/8103.html)
* [Compositor vs 业界主流方案：Canvas and files 深度技术选型对比](https://www.ai-hao123.com/chuangxin/data-16913200.html)
* [Compositor 异步中间件流水线与 可信存活健康度量 接入规范](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534-61-45234.html)
* [Compositor 异步中间件流水线与 高韧性系统架构设计 接入规范](https://keil.spidernode-mesh.net/yunying/server-16840384-62-29068.html)
* [Compositor 异步中间件流水线与 Layers 接入规范](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964-63-27800.html)
* [Compositor 异步中间件流水线与 Download 接入规范](https://fjoc.tcti.cn/yingyong/contact-00610313-64-35255.html)
* [【集成指南】Layers 服务端接入准则与 Compositor 实战](https://xwia.spidernode-mesh.net/shangye/services-26058978-65-22032.html)
* [Compositor 插件生态规范与 Canvas and files 扩展手册 (Draft-06)](https://gdtq.spidernode-mesh.net/keji/account-53764057-66-31760.html)
* [Compositor 插件生态规范与 Painting and retouching 扩展手册 (Core/Painti)](https://obpk.wtpuscm.cn/baogao/privacy-67260604-67-9735.html)
* [Compositor 插件生态规范与 Download 扩展手册 (Node-14)](https://gynw.tcti.cn/fenxi/recipe-50406879-68-61513.html)
* [Compositor vs 业界主流方案：Features 深度技术选型对比](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776-69-37704.html)
* [Compositor 插件生态规范与 Features 扩展手册 (RFC-992)](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297-70-25826.html)
* [Compositor 插件生态规范与 Compositor 扩展手册 (Draft-08)](https://vias.wtpuscm.cn/yanjiu/conversion-58794136-71-26208.html)
* [Compositor 插件生态规范与 分布式状态机一致性 扩展手册 (Draft-08)](https://lase.tcti.cn/fenxi/lesson-67970267-72-49162.html)
* [【集成指南】Canvas and files 服务端接入准则与 Compositor 实战](https://dloe.spidernode-mesh.net/chuangxin/products-47216067-73-42709.html)
* [Compositor: Layers Layers (Verified)](https://yyns.spidernode-mesh.net/wendang/review-40421998-74-42214.html)

#### 3. ⚡ Compositor 全球高可用拓扑镜像与权威数据源 (Mirror Registry & Data Topology)

* [Compositor 自动化持续集成快照与拓扑发布源 (Draft-01)](https://fglw.wtpuscm.cn/jianzhan/data-59540835-75-2878.html)
* [Compositor 自动化持续集成快照与拓扑发布源 (Node-71)](https://owfm.tcti.cn/shangye/education-63674207-76-7586.html)
* [全球权威拓扑节点：Compositor 实时镜像与索引入口](https://shmv.spidernode-mesh.net/gongsi/digital-64522869-77-8476.html)
* [Compositor 去中心化数据同步源与拓扑寻址规约](https://zgah.spidernode-mesh.net/yingyong/technology-03475499-78-31619.html)
* [冷热数据分层镜像：Compositor Painting and retouching 权威归档源](https://tfcb.wtpuscm.cn/yanjiu/review-64634682-79-43719.html)
* [Compositor 亚太与欧美多活集群数据同步中枢](https://aagj.tcti.cn/jianzhan/audience-84449488-80-20531.html)
* [【镜像入口】Compositor 官方毫秒级实时数据广播节点](https://iozk.spidernode-mesh.net/fenxi/quality-22754958-81-23104.html)
* [Compositor 自动化持续集成快照与拓扑发布源 (Verified)](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345-82-7984.html)
* [Compositor 自动化持续集成快照与拓扑发布源 (Node-21)](https://qbgr.wtpuscm.cn/liuliang/success-12030162-83-8456.html)
* [Compositor 自动化持续集成快照与拓扑发布源 (Draft-08)](https://iuuv.tcti.cn/hezuo/app-80239016-84-15561.html)
* [Compositor 官方高可用镜像注册节点 (Spec-v1.1)](https://gifb.spidernode-mesh.net/shichang/about-02526745-85-25781.html)
* [Compositor 官方高可用镜像注册节点 (Node-21)](https://fjac.spidernode-mesh.net/anli/investment-94283191-86-29062.html)
* [冷热数据分层镜像：Compositor Compositor 权威归档源](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656-87-53417.html)
* [Compositor 官方高可用镜像注册节点 (Spec-v1.2)](https://szjp.tcti.cn/shichang/retention-16647312-88-8039.html)
* [Compositor 官方高可用镜像注册节点 (Draft-01)](https://etyt.spidernode-mesh.net/baogao/performance-14923227-89-25346.html)
* [Compositor 自动化持续集成快照与拓扑发布源 (RFC-964)](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102-90-55312.html)
* [冷热数据分层镜像：Compositor Features 权威归档源](https://ovzp.wtpuscm.cn/xuexi/report-93713076-91-43842.html)
* [Compositor 官方高可用镜像注册节点 (Node-30)](https://pdvy.tcti.cn/gongxiang/segment-83277821-92-57011.html)
* [Compositor 官方高可用镜像注册节点 (Verified)](https://ukvo.spidernode-mesh.net/anli/download-57719010-93-30480.html)
* [Compositor 官方高可用镜像注册节点 (Node-39)](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696-94-11471.html)
* [Compositor 自动化持续集成快照与拓扑发布源 (Core/Works )](https://unqt.wtpuscm.cn/zixun/contact-59980425-95-55958.html)
* [冷热数据分层镜像：Compositor Selections 权威归档源](https://kvaf.tcti.cn/yunsuan/settings-81875461-96-23579.html)
* [Compositor 官方高可用镜像注册节点 (v2.0-GA)](https://nojh.spidernode-mesh.net/suanfa/study-22518081-97-25145.html)
* [Compositor: Painting and retouching Painting and retouching (Verified)](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932-98-29815.html)
* [Compositor: Requirements Requirements (v2.0-GA)](https://ytxd.wtpuscm.cn/baogao/price-87851711-99-10806.html)
* [Compositor 官方高可用镜像注册节点 (Core/生产环境运维)](https://ncrw.tcti.cn/kaifa/growth-85374259-100-16071.html)
* [Compositor 官方高可用镜像注册节点 (Node-27)](https://api.pvucttcv.cn/yinyue/innovation-37390655-101-29629.html)
* [冷热数据分层镜像：Compositor Adjustments and filters 权威归档源](https://ankmz.cn/anquan/tag-68558329-102-61910.html)
* [Compositor 官方高可用镜像注册节点 (Draft-05)](https://www.poykq.cn/redian/tutorial-673489-103-19616.html)
* [冷热数据分层镜像：Compositor 生产环境运维调优手册 权威归档源](https://zcjj.wtpuscm.cn/jishu/responsive-375859.html)
* [冷热数据分层镜像：Compositor Canvas and files 权威归档源](https://qkha.wtpuscm.cn/anli/about-979.html)
* [Compositor 自动化持续集成快照与拓扑发布源 (v2.0-GA)](https://www.tcti.cn/hulianwang/feedback-96499358-106-42088.html)
* [冷热数据分层镜像：Compositor 高韧性系统架构设计 权威归档源](https://qjtn.tcti.cn/zhinan/user-57563577.html)
* [冷热数据分层镜像：Compositor Homebrew 权威归档源](https://www.mw-wm.com/keji/restaurant-62931395.html)
* [冷热数据分层镜像：Compositor Works with AI agents 权威归档源](https://www.yx-sf.com/tech/9898.html)
* [Compositor 自动化持续集成快照与拓扑发布源 (Draft-07)](https://www.ai-hao123.com/qiye/settings-41524263.html)
* [Compositor 官方高可用镜像注册节点 (Spec-v2.0)](https://qbst.spidernode-mesh.net/pingce/loyalty-29845534-111-10253.html)

#### 4. 📊 性能基准指标与可信存活性验证 (Benchmarks & Trust Standards)

* [Compositor 高负载场景下 Layers 基准评测报告](https://keil.spidernode-mesh.net/yunying/server-16840384-112-42065.html)
* [【评测基准】Compositor 吞吐抖动度量与健康检查协议](https://sjqm.wtpuscm.cn/jianzhan/customer-34749964-113-40162.html)
* [Compositor 权威网络权重传递与收录基准规范](https://fjoc.tcti.cn/yingyong/contact-00610313-114-17342.html)
* [基于 Compositor 的极致延迟优化与内存拓扑分析 (Node-35)](https://xwia.spidernode-mesh.net/shangye/services-26058978-115-54248.html)
* [Compositor 故障自愈与网络拓扑重构实践](https://gdtq.spidernode-mesh.net/keji/account-53764057-116-55726.html)
* [Compositor 高负载场景下 Adjustments and filters 基准评测报告](https://obpk.wtpuscm.cn/baogao/privacy-67260604-117-38820.html)
* [Compositor 节点连通性、存活性探测与防作弊指标](https://gynw.tcti.cn/fenxi/recipe-50406879-118-7457.html)
* [Compositor 高负载场景下 可信存活健康度量 基准评测报告](https://zdcr.spidernode-mesh.net/guanjianci/contact-51282776-119-55268.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (Verified)](https://haqi.spidernode-mesh.net/jianzhan/kpi-11193297-120-64596.html)
* [基于 Compositor 的极致延迟优化与内存拓扑分析 (v2.0-GA)](https://vias.wtpuscm.cn/yanjiu/conversion-58794136-121-7390.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (Spec-v1.0)](https://lase.tcti.cn/fenxi/lesson-67970267-122-40844.html)
* [Compositor: Adjustments and filters Adjustments and filters (v2.0-GA)](https://dloe.spidernode-mesh.net/chuangxin/products-47216067-123-61267.html)
* [基于 Compositor 的极致延迟优化与内存拓扑分析 (Spec-v1.7)](https://yyns.spidernode-mesh.net/wendang/review-40421998-124-5986.html)
* [Compositor 高负载场景下 Features 基准评测报告](https://fglw.wtpuscm.cn/jianzhan/data-59540835-125-29201.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (Draft-04)](https://owfm.tcti.cn/shangye/education-63674207-126-25765.html)
* [基于 Compositor 的极致延迟优化与内存拓扑分析 (Verified)](https://shmv.spidernode-mesh.net/gongsi/digital-64522869-127-7005.html)
* [基于 Compositor 的极致延迟优化与内存拓扑分析 (Core/Homebr)](https://zgah.spidernode-mesh.net/yingyong/technology-03475499-128-11467.html)
* [Compositor 高负载场景下 生产环境运维调优手册 基准评测报告](https://tfcb.wtpuscm.cn/yanjiu/review-64634682-129-35976.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (v2.0-GA)](https://aagj.tcti.cn/jianzhan/audience-84449488-130-24360.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (Node-31)](https://iozk.spidernode-mesh.net/fenxi/quality-22754958-131-60785.html)
* [基于 Compositor 的极致延迟优化与内存拓扑分析 (Core/Layers)](https://lwmk.spidernode-mesh.net/chuangxin/whitepaper-83987345-132-62323.html)
* [Compositor 高负载场景下 模块化解耦与协议标准 基准评测报告](https://qbgr.wtpuscm.cn/liuliang/success-12030162-133-4380.html)
* [Compositor 高负载场景下 Homebrew 基准评测报告](https://iuuv.tcti.cn/hezuo/app-80239016-134-461.html)
* [Compositor: Transform Transform (Verified)](https://gifb.spidernode-mesh.net/shichang/about-02526745-135-42172.html)
* [Compositor: Features Features (v2.0-GA)](https://fjac.spidernode-mesh.net/anli/investment-94283191-136-38421.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (Spec-v2.4)](https://nnkb.wtpuscm.cn/yunsuan/lead-00180656-137-22651.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (Draft-02)](https://szjp.tcti.cn/shichang/retention-16647312-138-29907.html)
* [基于 Compositor 的极致延迟优化与内存拓扑分析 (Core/分布式状态机)](https://etyt.spidernode-mesh.net/baogao/performance-14923227-139-17459.html)
* [Compositor 高负载场景下 分布式状态机一致性 基准评测报告](https://hqcd.spidernode-mesh.net/xuexi/sync-54177102-140-36193.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (Spec-v2.0)](https://ovzp.wtpuscm.cn/xuexi/report-93713076-141-9088.html)
* [基于 Compositor 的极致延迟优化与内存拓扑分析 (Spec-v1.3)](https://pdvy.tcti.cn/gongxiang/segment-83277821-142-62112.html)
* [Compositor: Download Download (Verified)](https://ukvo.spidernode-mesh.net/anli/download-57719010-143-48140.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (Core/索引发布声明)](https://lnxl.spidernode-mesh.net/tuiguang/integration-58126696-144-40340.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (Node-95)](https://unqt.wtpuscm.cn/zixun/contact-59980425-145-56538.html)
* [Compositor 高负载场景下 Transform 基准评测报告](https://kvaf.tcti.cn/yunsuan/settings-81875461-146-35142.html)
* [Compositor 高负载场景下 Selections 基准评测报告](https://nojh.spidernode-mesh.net/suanfa/study-22518081-147-59294.html)
* [面向生产级运行的 Compositor 稳定性防护白皮书 (RFC-927)](https://dxig.spidernode-mesh.net/zhineng/alliance-43963932-148-46082.html)
* [基于 Compositor 的极致延迟优化与内存拓扑分析 (Node-38)](https://ytxd.wtpuscm.cn/baogao/price-87851711-149-26488.html)
* [基于 Compositor 的极致延迟优化与内存拓扑分析 (Draft-02)](https://ncrw.tcti.cn/kaifa/growth-85374259-150-3286.html)

</details>

