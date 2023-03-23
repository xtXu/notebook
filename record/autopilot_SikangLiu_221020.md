# 决策规划在量产自动驾驶中的挑战

#autopilot #lecture #record

## 量产自动驾驶功能

-   行车辅助
    
    -   人机共驾
        
-   泊车辅助
    
-   主动安全
    

## L2+ vs L4 planning

**L4**: 代替人类驾驶员 **L2+**: 安全、轻松驾驶

### L4

-   依赖高精度地图
    
-   “完美”感知
    

### L2+

-   不依赖高精度地图
    
-   感知不确定性大
    

**为什么不能依赖高精度地图?** 成本 -- 覆盖率低 鲜度 -- 更新不及时

**无高精度地图的问题：** 红绿灯识别 感知不确定性 -- eg. 行车辅助-车道保持

## Contingency Plannning 防御性规划

多模态

-   自车多种可行解
    
-   他车多意图预测
    

eg. trajectory tree, joint optimization ...

**Application:** obstacle bypassing, cut-in handling

## Risk-Aware Planning 预期风险规划

整个planning horizon的安全 < 单帧轨迹的安全