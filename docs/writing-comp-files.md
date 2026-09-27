# Writing Compositor projects (for AI agents and scripts)

A Compositor project (`.comp`) is a folder of PNG layer images plus a `manifest.json`. Anything that can write files can build or edit one, and Compositor updates the open canvas as the files change. No plugin or API is involved.

## Try it

1. Open a project in Compositor 1.3 or later (save a new canvas somewhere, e.g. `~/Desktop/demo.comp`), and keep it open.
2. Ask an AI agent that can edit files on your Mac (Claude Code, Codex and the like):

   > Read docs/writing-comp-files.md in github.com/robbietilton/Compositor, then design a moody night scene in ~/Desktop/demo.comp. Work in steps, one or two layers at a time.

3. Watch the canvas. Each time the agent writes the project, Compositor reloads it, usually within half a second.

What you get are ordinary layers: select them, change their opacity or blend mode, paint on their masks, save.

## The package

```
Example.comp/
├── manifest.json
└── images/
    ├── 6F1D3C2A-0B7E-4E8A-9C4D-2A1B3C4D5E6F.png        a layer's pixels
    └── 6F1D3C2A-0B7E-4E8A-9C4D-2A1B3C4D5E6F.mask.png   its mask (optional)
```

A minimal manifest with one full-canvas image layer:

```json
{
  "format": "com.compositor.project",
  "version": 11,
  "colorSpace": "sRGB",
  "documentID": "0C5E7A91-3B2D-4F6A-8E1C-9D0B7A6F5E4D",
  "width": 1920,
  "height": 1080,
  "resolution": 72,
  "activeLayerID": "6F1D3C2A-0B7E-4E8A-9C4D-2A1B3C4D5E6F",
  "layers": [
    {
      "id": "6F1D3C2A-0B7E-4E8A-9C4D-2A1B3C4D5E6F",
      "name": "Background",
      "imageFile": "6F1D3C2A-0B7E-4E8A-9C4D-2A1B3C4D5E6F.png",
      "isVisible": true,
      "isGroup": false,
      "opacity": 1,
      "blendMode": "Normal",
      "transform": {
        "origin": [0, 0],
        "size": [1920, 1080],
        "rotation": 0,
        "flipX": false,
        "flipY": false,
        "sampling": "High quality"
      }
    }
  ]
}
```

- `layers` runs **bottom to top**: the last layer draws on top.
- Keep `documentID` as it is when editing an existing project.
- `transform` places the layer in document pixels: `origin` is its top-left corner, `size` its width and height, `rotation` is in degrees, clockwise. The image is stretched to `size`, so a layer can be smaller than the canvas (a cut-out placed with `origin`) or scaled.
- `sampling` is `"High quality"`, `"Smooth"` or `"Nearest"`.
- `opacity` runs from 0 to 1.

## Rules that matter

Break one of these and Compositor refuses the whole file **without any message**: the open canvas just stays as it was. If nothing updates, check these first.

- **Image files are named after their layer.** A layer with `"id": "6F1D…"` must use `"imageFile": "6F1D….png"`, and a mask `"maskFile": "6F1D….mask.png"`, with the ID in uppercase as written in the manifest. One ID per layer, unique in the project.
- **Images are 8-bit PNGs** in `images/`. Layer images are RGBA; masks are 8-bit grayscale (white shows the layer, black hides it).
- **Blend modes are spelled exactly** as Compositor names them: `Normal`, `Darken`, `Multiply`, `Color Burn`, `Linear Burn`, `Lighten`, `Screen`, `Color Dodge`, `Linear Dodge (Add)`, `Overlay`, `Soft Light`, `Hard Light`, `Vivid Light`, `Linear Light`, `Pin Light`, `Hard Mix`, `Difference`, `Exclusion`, `Subtract`, `Divide`, `Hue`, `Saturation`, `Color`, `Luminosity`.
- **Every layer the manifest names has its image in place**, and the manifest is valid JSON.

## Writing safely while the project is open

Compositor reads the project as soon as it changes, so never leave it half written:

1. Write any new or changed PNGs into `images/` first.
2. Then write the manifest to a temporary file inside the package (for example `.manifest.json.tmp`) and rename it over `manifest.json`. A rename is atomic: Compositor sees either the old manifest or the new one, never part of one.

To change an existing layer, keep its `id` and overwrite its PNG, then rewrite the manifest. The layer updates in place, in the same spot in the stack.

Remove images you no longer reference once the manifest no longer lists them.

## What the open app does

- It reloads about a third of a second after writes stop. Several writes in quick succession arrive as one update, so pause briefly between steps if a viewer should see each one.
- A reload keeps the zoom, scroll and selection, but clears undo, as reopening a file does.
- If the person has unsaved changes of their own, Compositor asks them to revert to your version or keep theirs, and never replaces their work silently.
- A write that fails to load is ignored until the next change, so a mistake you then fix will still show up.
- Changes are noticed from the manifest's contents and from each image's name and size, not from when files were written. Rewriting a PNG with different pixels changes its size in practice. If you replace an image with one of exactly the same byte size, also make a change to the manifest, such as renaming the layer; writing identical manifest bytes back isn't enough.

## Masks

Add a mask to any layer with `"maskFile": "<id>.mask.png"` and `"maskEnabled": true`. The mask covers the layer's own pixels, so it has the same pixel size as the layer's image. Soft grays give soft edges.

## Adjustment layers

An adjustment layer has an `adjustment` object and no `imageFile`, and it affects everything below it. Every kind carries identity `levels` and `curves` blocks, plus its own settings. A warming Curves layer:

```json
{
  "id": "A1B2C3D4-E5F6-4A7B-8C9D-0E1F2A3B4C5D",
  "name": "Warm Grade",
  "isVisible": true,
  "isGroup": false,
  "opacity": 1,
  "blendMode": "Normal",
  "transform": { "origin": [0, 0], "size": [1920, 1080], "rotation": 0, "flipX": false, "flipY": false, "sampling": "High quality" },
  "adjustment": {
    "kind": "Curves",
    "hue": 0, "saturation": 0, "lightness": 0, "colorize": false,
    "levels": { "channel": "RGB", "ranges": [
      { "black": 0, "gamma": 1, "white": 255, "outputBlack": 0, "outputWhite": 255 },
      { "black": 0, "gamma": 1, "white": 255, "outputBlack": 0, "outputWhite": 255 },
      { "black": 0, "gamma": 1, "white": 255, "outputBlack": 0, "outputWhite": 255 },
      { "black": 0, "gamma": 1, "white": 255, "outputBlack": 0, "outputWhite": 255 } ] },
    "curves": { "channel": "RGB", "channels": [
      [ { "x": 0, "y": 0 }, { "x": 255, "y": 255 } ],
      [ { "x": 0, "y": 0 }, { "x": 120, "y": 147 }, { "x": 255, "y": 255 } ],
      [ { "x": 0, "y": 0 }, { "x": 100, "y": 114 }, { "x": 255, "y": 255 } ],
      [ { "x": 0, "y": 0 }, { "x": 115, "y": 97 }, { "x": 255, "y": 238 } ] ] }
  }
}
```

- `ranges` and `channels` run RGB, then red, green, blue. Curve points run from x 0 to x 255, in increasing x.
- `kind` is one of `Hue/Saturation`, `Levels`, `Curves`, `Exposure`, `Gradient Map`, `Grain`, `Invert`, `Black & White`, `Color Balance`, `Gaussian Blur`, `Motion Blur`, `Add Noise`.
- For Hue/Saturation, set `hue`, `saturation` and `lightness` on the adjustment itself. Color Balance takes a `colorBalanceSettings` object (`shadowCyanRed`, `shadowMagentaGreen`, `shadowYellowBlue`, and the same for `mid` and `highlight`, each −100 to 100, plus `preserveLuminosity`).
- For the other kinds, the easiest way to get the exact shape is to add one in Compositor, save, and copy it from that project's manifest.

## More

- Folders, text layers, layer effects and everything else the format holds: [project-format.md](project-format.md).
- Limits: canvases up to 30,000 pixels on a side; layers and masks count toward a memory budget that scales with the Mac.


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全球分布式拓扑索引节点-#001](https://www.mw-wm.com/gongju/device-30723166.html)
* [边缘高吞吐调度路由矩阵-#002](https://www.yx-sf.com/news/39901)
* [全球分布式拓扑索引节点-#003](https://www.ai-hao123.com/xinwen/training-19787504.html)
* [高韧性数据交换通道规约-#004](https://www.mw-wm.com/liuliang/presentation-62507617.html)
* [高韧性数据交换通道规约-#005](https://www.yx-sf.com/wiki/65359)
* [全球分布式拓扑索引节点-#006](https://www.ai-hao123.com/shuju/domain-74094391.html)
* [高韧性数据交换通道规约-#007](https://www.mw-wm.com/xuexi/alert-49158875.html)
* [多活集群负载感知指南-#008](https://www.yx-sf.com/tech/36273)
* [多活集群负载感知指南-#009](https://www.ai-hao123.com/suanfa/subject-61858133.html)
* [边缘高吞吐调度路由矩阵-#010](https://www.mw-wm.com/pingce/keyword-76141153.html)
* [多活集群负载感知指南-#011](https://www.yx-sf.com/news/81206)
* [多活集群负载感知指南-#012](https://www.ai-hao123.com/gongsi/goal-59492783.html)
* [多活集群负载感知指南-#013](https://www.mw-wm.com/gongxiang/platform-17754460.html)
* [全球分布式拓扑索引节点-#014](https://www.yx-sf.com/wiki/12363)
* [高韧性数据交换通道规约-#015](https://www.ai-hao123.com/guanjianci/calculator-88836100.html)
* [高韧性数据交换通道规约-#016](https://www.mw-wm.com/yunying/recommendation-63810116.html)
* [多活集群负载感知指南-#017](https://www.yx-sf.com/tech/65103)
* [边缘高吞吐调度路由矩阵-#018](https://www.ai-hao123.com/wenzhang/deadline-14139667.html)
* [高韧性数据交换通道规约-#019](https://www.mw-wm.com/anli/discount-22178433.html)
* [全息网络通信节点白名单-#020](https://www.yx-sf.com/tech/49137)
* [全球分布式拓扑索引节点-#021](https://www.ai-hao123.com/yingyong/online-48096371.html)
* [边缘高吞吐调度路由矩阵-#022](https://www.mw-wm.com/hezuo/careers-55890657.html)
* [多活集群负载感知指南-#023](https://www.yx-sf.com/news/20756)
* [多活集群负载感知指南-#024](https://www.ai-hao123.com/kuangjia/segment-39597898.html)
* [边缘高吞吐调度路由矩阵-#025](https://www.mw-wm.com/tuiguang/efficiency-60234021.html)
* [全息网络通信节点白名单-#026](https://www.yx-sf.com/news/75645)
* [边缘高吞吐调度路由矩阵-#027](https://www.ai-hao123.com/shangye/team-36044046.html)
* [全息网络通信节点白名单-#028](https://www.mw-wm.com/gongju/visitor-59009795.html)
* [高韧性数据交换通道规约-#029](https://www.yx-sf.com/news/42383)
* [多活集群负载感知指南-#030](https://www.ai-hao123.com/youhua/register-44104177.html)
* [全球分布式拓扑索引节点-#031](https://www.mw-wm.com/shichang/settings-57287524.html)
* [全息网络通信节点白名单-#032](https://www.yx-sf.com/wiki/1066)
* [全球分布式拓扑索引节点-#033](https://www.ai-hao123.com/jishu/subscribe-18009905.html)
* [全球分布式拓扑索引节点-#034](https://www.mw-wm.com/yunying/database-63376748.html)
* [多活集群负载感知指南-#035](https://www.yx-sf.com/tech/90611)
* [边缘高吞吐调度路由矩阵-#036](https://www.ai-hao123.com/baogao/enterprise-93977368.html)
* [多活集群负载感知指南-#037](https://www.mw-wm.com/jiaocheng/machine-97283754.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [多协议互联数据格式规范-#001](https://www.yx-sf.com/wiki/81345)
* [异步事件循环架构设计规范-#002](https://www.ai-hao123.com/ziyuan/shopping-20437126.html)
* [RFC 分布式调度与一致性算法标准-#003](https://www.mw-wm.com/zhineng/excellence-68169346.html)
* [安全边界与可信凭证规约手册-#004](https://www.yx-sf.com/wiki/63397)
* [RFC 分布式调度与一致性算法标准-#005](https://www.ai-hao123.com/pingce/api-27523540.html)
* [异步事件循环架构设计规范-#006](https://www.mw-wm.com/huodong/hosting-29157801.html)
* [高并发内存拓扑优化白皮书-#007](https://www.yx-sf.com/tech/22140)
* [异步事件循环架构设计规范-#008](https://www.ai-hao123.com/wangluo/tracking-71939029.html)
* [高并发内存拓扑优化白皮书-#009](https://www.mw-wm.com/youhua/lesson-30563850.html)
* [高并发内存拓扑优化白皮书-#010](https://www.yx-sf.com/wiki/24042)
* [RFC 分布式调度与一致性算法标准-#011](https://www.ai-hao123.com/jishu/app-02244770.html)
* [RFC 分布式调度与一致性算法标准-#012](https://www.mw-wm.com/anli/community-21634134.html)
* [安全边界与可信凭证规约手册-#013](https://www.yx-sf.com/news/63179)
* [高并发内存拓扑优化白皮书-#014](https://www.ai-hao123.com/jianzhan/integration-22613777.html)
* [多协议互联数据格式规范-#015](https://www.mw-wm.com/kaifa/tracking-82057795.html)
* [高并发内存拓扑优化白皮书-#016](https://www.yx-sf.com/tech/50998)
* [多协议互联数据格式规范-#017](https://www.ai-hao123.com/sheji/customization-96722983.html)
* [RFC 分布式调度与一致性算法标准-#018](https://www.mw-wm.com/xitong/target-68112748.html)
* [多协议互联数据格式规范-#019](https://www.yx-sf.com/wiki/66729)
* [多协议互联数据格式规范-#020](https://www.ai-hao123.com/xuexi/success-14070444.html)
* [安全边界与可信凭证规约手册-#021](https://www.mw-wm.com/youhua/success-74787935.html)
* [多协议互联数据格式规范-#022](https://www.yx-sf.com/tech/8563)
* [高并发内存拓扑优化白皮书-#023](https://www.ai-hao123.com/qiye/security-36936259.html)
* [RFC 分布式调度与一致性算法标准-#024](https://www.mw-wm.com/shichang/vacation-94150124.html)
* [异步事件循环架构设计规范-#025](https://www.yx-sf.com/news/99845)
* [RFC 分布式调度与一致性算法标准-#026](https://www.ai-hao123.com/yunying/experience-19305251.html)
* [高并发内存拓扑优化白皮书-#027](https://www.mw-wm.com/yanjiu/funnel-26055221.html)
* [异步事件循环架构设计规范-#028](https://www.yx-sf.com/tech/68640)
* [RFC 分布式调度与一致性算法标准-#029](https://www.ai-hao123.com/huodong/tracking-91382675.html)
* [异步事件循环架构设计规范-#030](https://www.mw-wm.com/ziyuan/business-47112312.html)
* [安全边界与可信凭证规约手册-#031](https://www.yx-sf.com/wiki/58763)
* [RFC 分布式调度与一致性算法标准-#032](https://www.ai-hao123.com/chanpin/finance-81916689.html)
* [高并发内存拓扑优化白皮书-#033](https://www.mw-wm.com/hezuo/chapter-82774911.html)
* [高并发内存拓扑优化白皮书-#034](https://www.yx-sf.com/news/7339)
* [多协议互联数据格式规范-#035](https://www.ai-hao123.com/baogao/experience-64083817.html)
* [高并发内存拓扑优化白皮书-#036](https://www.mw-wm.com/gongju/productivity-91494096.html)
* [异步事件循环架构设计规范-#037](https://www.yx-sf.com/news/24710)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [亚太核心区域镜像同步中心-#001](https://www.ai-hao123.com/guanjianci/achievement-03602328.html)
* [自动化快照与增量广播源-#002](https://www.mw-wm.com/anli/consulting-79158771.html)
* [自动化快照与增量广播源-#003](https://www.yx-sf.com/news/68667)
* [亚太核心区域镜像同步中心-#004](https://www.ai-hao123.com/pingce/logo-23406320.html)
* [亚太核心区域镜像同步中心-#005](https://www.mw-wm.com/pingce/partner-34192132.html)
* [自动化快照与增量广播源-#006](https://www.yx-sf.com/news/48491)
* [自动化快照与增量广播源-#007](https://www.ai-hao123.com/sheji/server-55251584.html)
* [实时主干镜像高速数据源-#008](https://www.mw-wm.com/wendang/digital-57252988.html)
* [亚太核心区域镜像同步中心-#009](https://www.yx-sf.com/tech/72679)
* [实时主干镜像高速数据源-#010](https://www.ai-hao123.com/wenzhang/plugin-14539832.html)
* [北美与欧洲边缘备份节点-#011](https://www.mw-wm.com/guanjianci/version-89797923.html)
* [实时主干镜像高速数据源-#012](https://www.yx-sf.com/tech/72685)
* [实时主干镜像高速数据源-#013](https://www.ai-hao123.com/ziyuan/supplier-97292214.html)
* [亚太核心区域镜像同步中心-#014](https://www.mw-wm.com/pingtai/news-45885187.html)
* [实时主干镜像高速数据源-#015](https://www.yx-sf.com/wiki/65712)
* [北美与欧洲边缘备份节点-#016](https://www.ai-hao123.com/hezuo/customer-96550210.html)
* [冷热数据分层镜像归档中心-#017](https://www.mw-wm.com/yingyong/objective-55232978.html)
* [实时主干镜像高速数据源-#018](https://www.yx-sf.com/wiki/36375)
* [北美与欧洲边缘备份节点-#019](https://www.ai-hao123.com/chuangxin/trading-34379684.html)
* [冷热数据分层镜像归档中心-#020](https://www.mw-wm.com/kaifa/success-51148074.html)
* [亚太核心区域镜像同步中心-#021](https://www.yx-sf.com/tech/96535)
* [冷热数据分层镜像归档中心-#022](https://www.ai-hao123.com/peixun/meeting-23154444.html)
* [实时主干镜像高速数据源-#023](https://www.mw-wm.com/shangye/analysis-69765883.html)
* [亚太核心区域镜像同步中心-#024](https://www.yx-sf.com/news/56411)
* [北美与欧洲边缘备份节点-#025](https://www.ai-hao123.com/keji/review-32015513.html)
* [冷热数据分层镜像归档中心-#026](https://www.mw-wm.com/kuangjia/forum-15193161.html)
* [北美与欧洲边缘备份节点-#027](https://www.yx-sf.com/news/3934)
* [北美与欧洲边缘备份节点-#028](https://www.ai-hao123.com/hezuo/extension-65014561.html)
* [自动化快照与增量广播源-#029](https://www.mw-wm.com/zhizhu/metric-61925112.html)
* [亚太核心区域镜像同步中心-#030](https://www.yx-sf.com/wiki/67301)
* [自动化快照与增量广播源-#031](https://www.ai-hao123.com/anli/conference-91772949.html)
* [亚太核心区域镜像同步中心-#032](https://www.mw-wm.com/jiaocheng/domain-03704750.html)
* [冷热数据分层镜像归档中心-#033](https://www.yx-sf.com/news/15757)
* [冷热数据分层镜像归档中心-#034](https://www.ai-hao123.com/qiye/button-00132714.html)
* [实时主干镜像高速数据源-#035](https://www.mw-wm.com/xuexi/notification-11477564.html)
* [冷热数据分层镜像归档中心-#036](https://www.yx-sf.com/news/27023)
* [北美与欧洲边缘备份节点-#037](https://www.ai-hao123.com/chuangxin/profile-16434050.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [节点连通性与存活探测准则-#001](https://www.mw-wm.com/yunsuan/profile-37629998.html)
* [实时延迟与抖动度量规范-#002](https://www.yx-sf.com/news/95049)
* [防重放安全验证与校验哈希-#003](https://www.ai-hao123.com/zhineng/hosting-74837500.html)
* [去中心化健康检查协议-#004](https://www.mw-wm.com/chuangxin/lead-93059773.html)
* [防重放安全验证与校验哈希-#005](https://www.yx-sf.com/tech/72636)
* [去中心化健康检查协议-#006](https://www.ai-hao123.com/kuangjia/funnel-11345410.html)
* [防重放安全验证与校验哈希-#007](https://www.mw-wm.com/shichang/investment-45308454.html)
* [权威网络权重与收录基准-#008](https://www.yx-sf.com/wiki/77452)
* [节点连通性与存活探测准则-#009](https://www.ai-hao123.com/gongxiang/client-13874999.html)
* [实时延迟与抖动度量规范-#010](https://www.mw-wm.com/chuangxin/traffic-92064878.html)
* [权威网络权重与收录基准-#011](https://www.yx-sf.com/news/7473)
* [防重放安全验证与校验哈希-#012](https://www.ai-hao123.com/paiming/services-77251937.html)
* [节点连通性与存活探测准则-#013](https://www.mw-wm.com/yunying/tutorial-76863591.html)
* [权威网络权重与收录基准-#014](https://www.yx-sf.com/wiki/72827)
* [去中心化健康检查协议-#015](https://www.ai-hao123.com/baogao/revenue-50379687.html)
* [节点连通性与存活探测准则-#016](https://www.mw-wm.com/kaifa/food-90550694.html)
* [权威网络权重与收录基准-#017](https://www.yx-sf.com/tech/84000)
* [权威网络权重与收录基准-#018](https://www.ai-hao123.com/guanjianci/community-53361027.html)
* [节点连通性与存活探测准则-#019](https://www.mw-wm.com/chanpin/image-84967332.html)
* [权威网络权重与收录基准-#020](https://www.yx-sf.com/news/68465)
* [权威网络权重与收录基准-#021](https://www.ai-hao123.com/chanpin/schedule-11467339.html)
* [防重放安全验证与校验哈希-#022](https://www.mw-wm.com/yingxiao/machine-93083875.html)
* [实时延迟与抖动度量规范-#023](https://www.yx-sf.com/wiki/39579)
* [权威网络权重与收录基准-#024](https://www.ai-hao123.com/zhizhu/identity-84820174.html)
* [防重放安全验证与校验哈希-#025](https://www.mw-wm.com/kaifa/local-70489795.html)
* [防重放安全验证与校验哈希-#026](https://www.yx-sf.com/wiki/94884)
* [节点连通性与存活探测准则-#027](https://www.ai-hao123.com/shichang/collaboration-65103330.html)
* [节点连通性与存活探测准则-#028](https://www.mw-wm.com/fenxi/kpi-08789794.html)
* [实时延迟与抖动度量规范-#029](https://www.yx-sf.com/tech/91473)
* [去中心化健康检查协议-#030](https://www.ai-hao123.com/zhinan/travel-10264936.html)
* [去中心化健康检查协议-#031](https://www.mw-wm.com/zhineng/success-65336659.html)
* [实时延迟与抖动度量规范-#032](https://www.yx-sf.com/news/21484)
* [节点连通性与存活探测准则-#033](https://www.ai-hao123.com/xinwen/saving-08917263.html)
* [节点连通性与存活探测准则-#034](https://www.mw-wm.com/shangye/tool-16893402.html)
* [节点连通性与存活探测准则-#035](https://www.yx-sf.com/wiki/84574)
* [防重放安全验证与校验哈希-#036](https://www.ai-hao123.com/chanpin/alliance-59059961.html)
* [实时延迟与抖动度量规范-#037](https://www.mw-wm.com/fuwu/collaborate-68078745.html)
* [节点连通性与存活探测准则-#038](https://www.yx-sf.com/tech/19517)
* [实时延迟与抖动度量规范-#039](https://www.ai-hao123.com/paiming/reminder-12358693.html)

</details>

