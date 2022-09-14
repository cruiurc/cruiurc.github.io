产品项目PLANTUML源文件
=====================
Z200
```plantuml
@startuml
left to right direction

title Z200

object 开始 #lightgreen {
2022-2-21
}
object 样机组装完毕 #lightgreen {
2022-4-25
}
object 基本功能调试完毕 #lightgreen {
2022-5-10
}
object 双回路开发完毕{
2022-7-26
}
object 旁路开发完毕{
2022-7-30
}
object 支柱绝缘子开发完毕{
2022-8-7
}
object 双回路演示 {
2022-8
}
object 旁路演示 {
2022-8
}
object 支柱绝缘子演示 {
2022-8
}
开始 --> 样机组装完毕
样机组装完毕 --> 基本功能调试完毕
基本功能调试完毕 --> 双回路开发完毕
双回路开发完毕 --> 双回路演示
基本功能调试完毕 --> 旁路开发完毕
旁路开发完毕 --> 旁路演示
基本功能调试完毕 --> 支柱绝缘子开发完毕
支柱绝缘子开发完毕 --> 支柱绝缘子演示
@enduml
```

N100
```plantuml
@startuml
left to right direction

title N100
object 开始 #lightgreen {
2022-7-7
}

object 仁恒设备更换 #lightgreen {
2022-7-30
}
object 硬件设计优化 #lightgreen{
2022-8-10
}
object 硬件设计冻结 #lightgreen{
2022-8-30
}

object 软件715版本开发 #lightgreen {
2022-7-15
}

object 软件730版本开发 #lightgreen{
2022-7-30
}

object 软件815版本开发 #lightgreen{
2022-8-15
}
object 软件830版本开发 {
2022-8-30
}
object 完成试制 {
2022-9-30
}
object 完成北京试点 {
2022-9-30
}

开始 --> 仁恒设备更换
仁恒设备更换 --> 硬件设计优化
硬件设计优化 --> 硬件设计冻结
开始 --> 软件715版本开发
软件715版本开发 --> 软件730版本开发
软件730版本开发 --> 软件815版本开发
软件815版本开发 --> 软件830版本开发
硬件设计冻结 --> 完成试制
完成试制 --> 完成北京试点
软件830版本开发 --> 完成北京试点
@enduml
```
M100
```plantuml
@startuml
left to right direction

title M100
object 开始 #lightgreen {
2021-12-30
}

object TR1 #lightgreen {
2021-12-30
}

object TR2 #lightgreen {
2022-4-15
}

object TR3 {
2022-10-30
}

object TR4 {
}

object 试点验收 {
2022-8-30
}

开始 --> TR1
TR1 --> TR2
TR2 --> TR3
TR3 --> TR4
TR2 --> 试点验收
```
消防 
```plantuml
@startuml
left to right direction

title 40D
object 开始 #lightgreen {
2021-12-30
}

object 拍摄视频 #lightgreen {
2022-3-10
}

object 站内调试完成 #lightgreen {
2022-5-30
}

object 第二次拍摄视频 #lightgreen {
2022-6-20
}

object 演示 #red {
2022-7-15
}
开始 --> 拍摄视频
拍摄视频 --> 站内调试完成
站内调试完成 --> 第二次拍摄视频
第二次拍摄视频 --> 演示

```


```plantuml
@startuml
left to right direction

title FG100

object 开始 #lightgreen {
2021-12-30
}


object 无线充电测试完成 #red {
2022-7-15
}

object 38套生产完毕 {
2022-9-15
}

object 38套交付验收 {
2022-9-15
}

object 组网方案确认和超声波需求提出 #lightgreen {
2022-3-10
}

object 超声波改造完成 #lightgreen {
2022-4-30
}

object 双确认功能开发完成 #lightgreen {
2022-6-24
}

object 演示验收 #red {
2022-7-11
}

开始 --> 无线充电测试完成
无线充电测试完成 --> 38套生产完毕
38套生产完毕 --> 38套交付验收
开始 --> 组网方案确认和超声波需求提出
组网方案确认和超声波需求提出 --> 超声波改造完成
超声波改造完成 --> 双确认功能开发完成
双确认功能开发完成 --> 演示验收

```
复合机器人
```plantuml
@startuml
left to right direction

title H100
object 开始 #lightgreen {
2022-6-2
}
object TR2详细设计完成 #lightgreen {
2022-7-5
}
object TR2组装调试完成 {
2022-8-19
}
object TR2样机测试完成 {
2022-9-2
}
object TR2样机交付 {
2022-9-7
}
object TR2评审 {
2022-9-23
}
开始 --> TR2详细设计完成
TR2详细设计完成 --> TR2组装调试完成
TR2组装调试完成 --> TR2样机测试完成
TR2样机测试完成 --> TR2样机交付
TR2样机交付 --> TR2评审
@enduml
```
C100
```plantuml
@startuml
left to right direction

title C100
object 开始 #lightgreen {
2022-4-1
}
object 下图完成 #lightgreen {
2022-4-30
}
object 组装完成 #lightgreen {
2022-5-27
}
object 演示 #lightgreen {
2022-6-15
}
object TR2评审 #lightgreen {
2022-6-30
}
object TR3评审 {
2022-9-15
}
object 取得订单 {
-
}
开始 --> 下图完成
下图完成 --> 组装完成
组装完成 --> 演示
演示 --> TR2评审
TR2评审 --> TR3评审
演示 --> 取得订单
@enduml
```
Q100
```plantuml
@startuml
left to right direction

title Q100
object 开始 #lightgreen {
2022-2-27
}
object 草莓机巢测试 #lightgreen {
2022-3-12
}
object 牧龙变首次演示 #lightgreen {
2022-5-20
}
object 牧龙变一体化巡检演示 #lightgreen {
2022-7-15
}
object 取得订单 #lightgreen {
2022-7-7
}
object 首单发货 #lightgreen {
2022-7-20
}
object 调试完成 {
2022-9-30
}
object 验收 {
2022-10-30
}


开始 --> 草莓机巢测试
草莓机巢测试 --> 牧龙变首次演示
牧龙变首次演示 --> 牧龙变一体化巡检演示
牧龙变一体化巡检演示 --> 取得订单
取得订单 --> 首单发货
首单发货 --> 调试完成
调试完成 --> 验收
@enduml
```
OC100
```plantuml
@startuml
left to right direction

title OC100
object 开始 #lightgreen {
2022-6-2
}
object 江宁农网试用 #lightgreen {
2022-6-30
}
object 自研产品具备演示条件 #red {
2022-8-20
}
object 试点运行 {
2022-9-30
}
object 具备销售条件 {
2022-10-30
}
object 具备批量交付能力 {
2022-11-30
}


开始 --> 江宁农网试用
江宁农网试用 --> 自研产品具备演示条件
自研产品具备演示条件 --> 试点运行
试点运行 --> 具备销售条件
具备销售条件 --> 具备批量交付能力
@enduml
```
V100
```plantuml
@startuml
left to right direction

title V100
object 开始TR4 #lightgreen {
2022-4-14
}
object 试制完成 #lightgreen {
2022-3-31
}
object 获得订单 {
2022-8-15
}
object TR4评审 {
2022-9-30
}
object 试点数据收集完毕 {
2022-8-30
}
object 系统测试完毕 {
2022-9-14
}

开始TR4 --> 试制完成
试制完成 --> TR4评审
开始TR4 --> 获得订单
获得订单 --> TR4评审
开始TR4 --> 试点数据收集完毕
试点数据收集完毕 --> 系统测试完毕
系统测试完毕 --> TR4评审
@enduml
```
巡检
```plantuml
@startuml

title 量产（A200&SI100&E100&SA200)
object 开始 #lightgreen {
-
}
object A200优化版硬件发布 {
2022-10-30
}
object A200优化版软件发布 {
2022-11-30
}
object A200V3.3发布 {
2022-8-31
}
object SI100V2.7发布 {
2022-8-31
}
object E100V4.2发布 {
2022-7-29
}
object SA200V1.1.7发布 {
2022-7-29
}

开始 --> A200优化版硬件发布
A200优化版硬件发布 --> A200优化版软件发布
开始 --> A200V3.3发布
开始 --> SI100V2.7发布
开始 --> E100V4.2发布
开始 --> SA200V1.1.7发布
@enduml
```

EP100
```plantuml
@startuml
left to right direction

title EP100
object 关键元器件选型 #red {
2022-8-3
}
object 方案设计出图 {
2022-8-30
}
object 完成调试 {
2022-9-27
}
object 系统测试 {
2022-12-30
}
object 小批试制 {
2023-3-30
}
object 防爆证排队 #lightgreen {
2022-7-31
}
object 送检 {
2022-9-19
}
object 获取放爆证 {
2022-11-30
}
object 结项 {
2023-3-30
}
关键元器件选型 --> 方案设计出图
方案设计出图 --> 完成调试
完成调试 --> 系统测试
系统测试 --> 小批试制
小批试制 --> 结项
防爆证排队 --> 送检
送检 --> 获取放爆证
获取放爆证 --> 结项
@enduml
```