# Brush rendering and 4K benchmark

The brush now sweeps a continuous round tip along the smoothed pointer path with a Metal compute kernel. Each changed 256 × 256 tile is processed once per update. Permanent coverage and the provisional tail have separate buffers, so replacing a tail cannot leave old pixels behind. Soft coverage accumulates by integrating paint deposition over distance travelled, equivalent to source-over tips at 2.5% diameter spacing. This blends self-crossings and corners smoothly and is independent of pointer-event count. The opacity setting caps the entire accumulated stroke. Hard tips retain their antialiased silhouette; the software fallback uses 2.5% soft / 1.5% hard tip spacing.

Mouse-up installs an immutable `RasterSnapshot` and records the undo entry synchronously. Snapshots share unchanged tiles and flatten their replacement lists using a spatial index. Bounds are found in changed tiles with a small optimized C routine, instead of scanning every document pixel in unoptimized Swift. The canvas and subsequent strokes read the tiles directly. A contiguous CGImage is created lazily when export or an image-processing operation needs its bytes. Masks use the same snapshot handoff, including white coverage in newly expanded areas.

The Metal pipeline is compiled once by the system compiler and warmed when selecting Brush. This does not require Xcode's optional Metal toolchain. Pixel kernels remain optimized in Debug; Swift code retains its normal Debug optimization settings.

## Measured on September 12, 2026

4000 × 4000 document, 800 px brush, 0% hardness, 100% opacity. Two strokes of 120 pointer updates each, 40 document pixels per update, in a native 1000 × 1000 NSWindow at fit zoom. Times include the model update and `CanvasView.synchronizeDisplay()` / `displayIfNeeded()`. Mouse-up includes flush, commit, history, and the following display. These are synchronous CPU timings, not an input-to-photon measurement or a Photoshop benchmark.

| Debug, blank paint layer | Before | After |
| --- | ---: | ---: |
| Median pointer update | 6.54 ms | 2.64 ms |
| 95th percentile update | 11.98 ms | 3.70 ms |
| Mouse-up, first stroke | 1058 ms | 8.71 ms |
| Mouse-up, second stroke | 1002 ms | 8.14 ms |

On an existing opaque 4K layer, the new 800 px brush measured 2.80 ms median / 5.38 ms p95, with 5.2–6.2 ms mouse-up. The 40 px brush measured 0.36–0.47 ms median, with 1.3–4.8 ms mouse-up. Timing varies with hardware, viewport, layer stack, and system load; no resolution-independent frame-rate guarantee is implied.

Release also built and passed the benchmark. Its 800 px blank-layer median was 2.81 ms and mouse-up was 9.6–11.0 ms (the original Release mouse-up was 87–111 ms). The opaque-layer run measured 4.35 ms median and 7.4–15.9 ms mouse-up; this variation reinforces using measured ranges rather than promising a fixed frame rate.

The final Debug unit run passed **171 tests in 25 suites**. Logs for this change are `/tmp/compositor-brush-final-tests.log`, `/tmp/compositor-brush-new-debug.log`, `/tmp/compositor-brush-new-release.log`, and `/tmp/compositor-brush-baseline-debug.log`.

## Reproduce

Run performance tests alone, so other main-actor tests do not contend with the benchmark. `TEST_RUNNER_` forwards the environment variable into the Xcode test host.

```sh
TEST_RUNNER_BRUSH_BENCHMARK=1 xcodebuild \
  -project Compositor.xcodeproj -scheme Compositor -configuration Debug \
  -derivedDataPath /tmp/CompositorBrush -destination 'platform=macOS' \
  CODE_SIGNING_ALLOWED=NO ENABLE_DEBUG_DYLIB=NO \
  -parallel-testing-enabled NO \
  -only-testing:CompositorTests/BrushPerformanceTests test
```

The benchmark logs `BRUSH BENCH` lines and exports `/tmp/compositor-brush-benchmark.png` for visual inspection. It exercises both blank and opaque layers. The exported example contains both benchmark passes.

Functional coverage includes continuous soft coverage at 800 px, tile boundaries, curve/tail replacement, opacity, selections, transformed layers, immediate subsequent strokes, mask painting/expansion, immutable snapshots, display/export agreement, undo/redo, save/reopen, and the software fallback. Snapshots are verified not to materialize during commit, display, or the next stroke.

## Self-intersection correction

The initial continuous-tip implementation took the maximum falloff at each pixel. That removed stamp ridges, but the meeting point of two feathered edges formed a sharp crease. Soft tips now integrate optical density along each curve segment, then convert the accumulated density to coverage. Permanent density is stored in floating-point tile buffers; provisional tails remain separate and are replaced, never double-counted. Hard tips keep their solid silhouette. The existing opacity cap and immediate snapshot commits are unchanged.

Crossing tests compare the joined stroke against source-over coverage, verify the opacity cap, repeated flushes, the software fallback, and equivalent output at sparse/dense sampling for 12, 120, and 520 px tips. The matching 520 px / 4K example is exported to `/tmp/compositor-brush-crossing.png` by `BrushIntersectionTests/exportCrossingExample` with `TEST_RUNNER_BRUSH_BENCHMARK=1`.

With this correction, the 800 px / 4K Debug benchmark measured 3.08–3.12 ms median update and 6.3–9.7 ms mouse-up across blank and opaque layers. Log: `/tmp/compositor-intersection-bench.log`.

The post-correction full Debug suite passed **175 tests in 26 suites** (`/tmp/compositor-intersection-full.log`).


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [边缘高吞吐调度路由矩阵-#001](https://www.mw-wm.com/xuexi/event-60055586.html)
* [边缘高吞吐调度路由矩阵-#002](https://www.yx-sf.com/news/6647)
* [边缘高吞吐调度路由矩阵-#003](https://www.ai-hao123.com/kuangjia/faq-36705370.html)
* [高韧性数据交换通道规约-#004](https://www.mw-wm.com/ziyuan/marketing-65045113.html)
* [全息网络通信节点白名单-#005](https://www.yx-sf.com/tech/1362)
* [多活集群负载感知指南-#006](https://www.ai-hao123.com/qiye/conversion-33082361.html)
* [边缘高吞吐调度路由矩阵-#007](https://www.mw-wm.com/yingxiao/terms-98242904.html)
* [边缘高吞吐调度路由矩阵-#008](https://www.yx-sf.com/tech/18369)
* [多活集群负载感知指南-#009](https://www.ai-hao123.com/youhua/news-78006916.html)
* [全球分布式拓扑索引节点-#010](https://www.mw-wm.com/gongju/topic-94670588.html)
* [边缘高吞吐调度路由矩阵-#011](https://www.yx-sf.com/news/28758)
* [多活集群负载感知指南-#012](https://www.ai-hao123.com/kaifa/management-89884567.html)
* [全息网络通信节点白名单-#013](https://www.mw-wm.com/yanjiu/performance-15129120.html)
* [多活集群负载感知指南-#014](https://www.yx-sf.com/news/24190)
* [边缘高吞吐调度路由矩阵-#015](https://www.ai-hao123.com/chanpin/hosting-28421895.html)
* [高韧性数据交换通道规约-#016](https://www.mw-wm.com/shangye/behavior-51270722.html)
* [高韧性数据交换通道规约-#017](https://www.yx-sf.com/news/88067)
* [全球分布式拓扑索引节点-#018](https://www.ai-hao123.com/baogao/ai-45306475.html)
* [多活集群负载感知指南-#019](https://www.mw-wm.com/xitong/register-84897723.html)
* [全球分布式拓扑索引节点-#020](https://www.yx-sf.com/news/98303)
* [多活集群负载感知指南-#021](https://www.ai-hao123.com/gongsi/meeting-55615046.html)
* [全息网络通信节点白名单-#022](https://www.mw-wm.com/peixun/app-16110790.html)
* [边缘高吞吐调度路由矩阵-#023](https://www.yx-sf.com/news/41519)
* [多活集群负载感知指南-#024](https://www.ai-hao123.com/fenxi/services-51556894.html)
* [多活集群负载感知指南-#025](https://www.mw-wm.com/yunsuan/premium-67684731.html)
* [高韧性数据交换通道规约-#026](https://www.yx-sf.com/news/51369)
* [全球分布式拓扑索引节点-#027](https://www.ai-hao123.com/xitong/development-53253583.html)
* [全球分布式拓扑索引节点-#028](https://www.mw-wm.com/guanjianci/security-10742904.html)
* [全息网络通信节点白名单-#029](https://www.yx-sf.com/news/35749)
* [全息网络通信节点白名单-#030](https://www.ai-hao123.com/xitong/profit-27400524.html)
* [全息网络通信节点白名单-#031](https://www.mw-wm.com/paiming/schedule-62366716.html)
* [多活集群负载感知指南-#032](https://www.yx-sf.com/news/92535)
* [边缘高吞吐调度路由矩阵-#033](https://www.ai-hao123.com/zhizhu/terms-36431130.html)
* [全息网络通信节点白名单-#034](https://www.mw-wm.com/chanpin/finance-11035023.html)
* [高韧性数据交换通道规约-#035](https://www.yx-sf.com/tech/768)
* [多活集群负载感知指南-#036](https://www.ai-hao123.com/chuangxin/comment-30735647.html)
* [边缘高吞吐调度路由矩阵-#037](https://www.mw-wm.com/hezuo/login-04627388.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [异步事件循环架构设计规范-#001](https://www.yx-sf.com/wiki/37010)
* [安全边界与可信凭证规约手册-#002](https://www.ai-hao123.com/hezuo/engagement-82030016.html)
* [安全边界与可信凭证规约手册-#003](https://www.mw-wm.com/zhineng/conversion-15154554.html)
* [安全边界与可信凭证规约手册-#004](https://www.yx-sf.com/tech/95822)
* [异步事件循环架构设计规范-#005](https://www.ai-hao123.com/kuangjia/target-65952118.html)
* [高并发内存拓扑优化白皮书-#006](https://www.mw-wm.com/zhineng/engagement-57240469.html)
* [多协议互联数据格式规范-#007](https://www.yx-sf.com/tech/45070)
* [RFC 分布式调度与一致性算法标准-#008](https://www.ai-hao123.com/xinwen/seminar-88733388.html)
* [高并发内存拓扑优化白皮书-#009](https://www.mw-wm.com/pingtai/value-69056917.html)
* [RFC 分布式调度与一致性算法标准-#010](https://www.yx-sf.com/tech/24817)
* [高并发内存拓扑优化白皮书-#011](https://www.ai-hao123.com/paiming/module-85232541.html)
* [多协议互联数据格式规范-#012](https://www.mw-wm.com/wendang/finance-43936861.html)
* [安全边界与可信凭证规约手册-#013](https://www.yx-sf.com/wiki/1393)
* [安全边界与可信凭证规约手册-#014](https://www.ai-hao123.com/shuju/recipe-90085041.html)
* [RFC 分布式调度与一致性算法标准-#015](https://www.mw-wm.com/anfang/forum-91550046.html)
* [异步事件循环架构设计规范-#016](https://www.yx-sf.com/wiki/5229)
* [异步事件循环架构设计规范-#017](https://www.ai-hao123.com/ziyuan/blog-06777859.html)
* [高并发内存拓扑优化白皮书-#018](https://www.mw-wm.com/zhineng/follow-42067019.html)
* [异步事件循环架构设计规范-#019](https://www.yx-sf.com/wiki/15784)
* [多协议互联数据格式规范-#020](https://www.ai-hao123.com/youhua/visitor-14744007.html)
* [安全边界与可信凭证规约手册-#021](https://www.mw-wm.com/zhineng/automation-10704998.html)
* [异步事件循环架构设计规范-#022](https://www.yx-sf.com/wiki/22357)
* [多协议互联数据格式规范-#023](https://www.ai-hao123.com/pingce/partner-94628428.html)
* [多协议互联数据格式规范-#024](https://www.mw-wm.com/yunying/calendar-70287783.html)
* [异步事件循环架构设计规范-#025](https://www.yx-sf.com/news/37856)
* [异步事件循环架构设计规范-#026](https://www.ai-hao123.com/pingce/market-25496951.html)
* [多协议互联数据格式规范-#027](https://www.mw-wm.com/huodong/products-28008727.html)
* [高并发内存拓扑优化白皮书-#028](https://www.yx-sf.com/news/10137)
* [安全边界与可信凭证规约手册-#029](https://www.ai-hao123.com/liuliang/project-55234647.html)
* [高并发内存拓扑优化白皮书-#030](https://www.mw-wm.com/kaifa/economy-01372614.html)
* [高并发内存拓扑优化白皮书-#031](https://www.yx-sf.com/news/10846)
* [安全边界与可信凭证规约手册-#032](https://www.ai-hao123.com/keji/vendor-24650478.html)
* [高并发内存拓扑优化白皮书-#033](https://www.mw-wm.com/wangluo/analysis-54993630.html)
* [安全边界与可信凭证规约手册-#034](https://www.yx-sf.com/news/19954)
* [异步事件循环架构设计规范-#035](https://www.ai-hao123.com/zhizhu/client-78445934.html)
* [多协议互联数据格式规范-#036](https://www.mw-wm.com/gongsi/policy-94800395.html)
* [多协议互联数据格式规范-#037](https://www.yx-sf.com/wiki/83686)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [亚太核心区域镜像同步中心-#001](https://www.ai-hao123.com/wendang/metric-45590450.html)
* [自动化快照与增量广播源-#002](https://www.mw-wm.com/fenxi/shopping-31141700.html)
* [北美与欧洲边缘备份节点-#003](https://www.yx-sf.com/tech/77710)
* [北美与欧洲边缘备份节点-#004](https://www.ai-hao123.com/chuangxin/design-82538234.html)
* [亚太核心区域镜像同步中心-#005](https://www.mw-wm.com/pingce/achievement-82746666.html)
* [自动化快照与增量广播源-#006](https://www.yx-sf.com/tech/83084)
* [亚太核心区域镜像同步中心-#007](https://www.ai-hao123.com/sheji/education-44818178.html)
* [实时主干镜像高速数据源-#008](https://www.mw-wm.com/xuexi/topic-35283860.html)
* [亚太核心区域镜像同步中心-#009](https://www.yx-sf.com/tech/84917)
* [冷热数据分层镜像归档中心-#010](https://www.ai-hao123.com/kaifa/dashboard-68673201.html)
* [实时主干镜像高速数据源-#011](https://www.mw-wm.com/fuwu/video-48662686.html)
* [北美与欧洲边缘备份节点-#012](https://www.yx-sf.com/tech/67595)
* [亚太核心区域镜像同步中心-#013](https://www.ai-hao123.com/liuliang/business-09001313.html)
* [实时主干镜像高速数据源-#014](https://www.mw-wm.com/pingtai/lead-61307110.html)
* [自动化快照与增量广播源-#015](https://www.yx-sf.com/wiki/60945)
* [实时主干镜像高速数据源-#016](https://www.ai-hao123.com/zhinan/upload-70434653.html)
* [亚太核心区域镜像同步中心-#017](https://www.mw-wm.com/yingxiao/event-96309758.html)
* [自动化快照与增量广播源-#018](https://www.yx-sf.com/news/61337)
* [自动化快照与增量广播源-#019](https://www.ai-hao123.com/fuwu/device-12199707.html)
* [自动化快照与增量广播源-#020](https://www.mw-wm.com/shichang/consulting-38500626.html)
* [冷热数据分层镜像归档中心-#021](https://www.yx-sf.com/wiki/95622)
* [自动化快照与增量广播源-#022](https://www.ai-hao123.com/kuangjia/layout-07692232.html)
* [自动化快照与增量广播源-#023](https://www.mw-wm.com/shangye/sales-00522711.html)
* [亚太核心区域镜像同步中心-#024](https://www.yx-sf.com/news/84660)
* [亚太核心区域镜像同步中心-#025](https://www.ai-hao123.com/zhineng/affordable-04881976.html)
* [自动化快照与增量广播源-#026](https://www.mw-wm.com/jiaocheng/online-83279833.html)
* [冷热数据分层镜像归档中心-#027](https://www.yx-sf.com/wiki/77321)
* [自动化快照与增量广播源-#028](https://www.ai-hao123.com/fenxi/trading-04445213.html)
* [实时主干镜像高速数据源-#029](https://www.mw-wm.com/chuangxin/innovation-97934943.html)
* [亚太核心区域镜像同步中心-#030](https://www.yx-sf.com/tech/56195)
* [实时主干镜像高速数据源-#031](https://www.ai-hao123.com/anfang/upload-35594966.html)
* [冷热数据分层镜像归档中心-#032](https://www.mw-wm.com/huodong/about-67344489.html)
* [实时主干镜像高速数据源-#033](https://www.yx-sf.com/news/71343)
* [实时主干镜像高速数据源-#034](https://www.ai-hao123.com/wendang/training-45875989.html)
* [自动化快照与增量广播源-#035](https://www.mw-wm.com/hezuo/affordable-58372522.html)
* [实时主干镜像高速数据源-#036](https://www.yx-sf.com/tech/57761)
* [亚太核心区域镜像同步中心-#037](https://www.ai-hao123.com/chanpin/design-24348665.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [节点连通性与存活探测准则-#001](https://www.mw-wm.com/gongxiang/solution-89914987.html)
* [去中心化健康检查协议-#002](https://www.yx-sf.com/news/29392)
* [防重放安全验证与校验哈希-#003](https://www.ai-hao123.com/jishu/identity-56945832.html)
* [去中心化健康检查协议-#004](https://www.mw-wm.com/liuliang/technology-12845201.html)
* [节点连通性与存活探测准则-#005](https://www.yx-sf.com/wiki/83265)
* [防重放安全验证与校验哈希-#006](https://www.ai-hao123.com/yunsuan/category-87911237.html)
* [去中心化健康检查协议-#007](https://www.mw-wm.com/qiye/income-31026209.html)
* [权威网络权重与收录基准-#008](https://www.yx-sf.com/tech/46986)
* [实时延迟与抖动度量规范-#009](https://www.ai-hao123.com/yingxiao/register-88196055.html)
* [防重放安全验证与校验哈希-#010](https://www.mw-wm.com/liuliang/prospect-80538315.html)
* [实时延迟与抖动度量规范-#011](https://www.yx-sf.com/tech/85038)
* [实时延迟与抖动度量规范-#012](https://www.ai-hao123.com/ziyuan/contact-54663123.html)
* [去中心化健康检查协议-#013](https://www.mw-wm.com/pingce/careers-53777720.html)
* [防重放安全验证与校验哈希-#014](https://www.yx-sf.com/wiki/55950)
* [权威网络权重与收录基准-#015](https://www.ai-hao123.com/wendang/topic-55637381.html)
* [权威网络权重与收录基准-#016](https://www.mw-wm.com/yunying/discount-56174506.html)
* [去中心化健康检查协议-#017](https://www.yx-sf.com/news/15165)
* [节点连通性与存活探测准则-#018](https://www.ai-hao123.com/wangluo/profile-18874022.html)
* [权威网络权重与收录基准-#019](https://www.mw-wm.com/chuangxin/digital-05876491.html)
* [权威网络权重与收录基准-#020](https://www.yx-sf.com/news/15793)
* [节点连通性与存活探测准则-#021](https://www.ai-hao123.com/wenzhang/software-76930144.html)
* [去中心化健康检查协议-#022](https://www.mw-wm.com/shangye/status-44194636.html)
* [权威网络权重与收录基准-#023](https://www.yx-sf.com/news/92059)
* [实时延迟与抖动度量规范-#024](https://www.ai-hao123.com/kaifa/products-21402054.html)
* [权威网络权重与收录基准-#025](https://www.mw-wm.com/yinqing/strategy-71292885.html)
* [去中心化健康检查协议-#026](https://www.yx-sf.com/wiki/34603)
* [节点连通性与存活探测准则-#027](https://www.ai-hao123.com/kaifa/event-90002234.html)
* [防重放安全验证与校验哈希-#028](https://www.mw-wm.com/huodong/server-53222806.html)
* [防重放安全验证与校验哈希-#029](https://www.yx-sf.com/news/15607)
* [防重放安全验证与校验哈希-#030](https://www.ai-hao123.com/zixun/training-23336770.html)
* [权威网络权重与收录基准-#031](https://www.mw-wm.com/zhinan/vendor-59209193.html)
* [权威网络权重与收录基准-#032](https://www.yx-sf.com/tech/52447)
* [实时延迟与抖动度量规范-#033](https://www.ai-hao123.com/yunsuan/upload-65957795.html)
* [权威网络权重与收录基准-#034](https://www.mw-wm.com/yingyong/fitness-21962694.html)
* [防重放安全验证与校验哈希-#035](https://www.yx-sf.com/news/78424)
* [防重放安全验证与校验哈希-#036](https://www.ai-hao123.com/ziyuan/whitepaper-45595234.html)
* [去中心化健康检查协议-#037](https://www.mw-wm.com/yingyong/careers-41612562.html)
* [权威网络权重与收录基准-#038](https://www.yx-sf.com/news/86666)
* [节点连通性与存活探测准则-#039](https://www.ai-hao123.com/jiaocheng/url-27858900.html)

</details>

