# 产品28：多病共存药食配送联动系统（详细设计实现文档）

## 0. 文档信息
- 版本：v1.0
- 目标：把“复杂建议”转化为“可执行的一周采购与做饭计划”
- 适用对象：糖尿病+肾病+高血压等多病共存家庭

## 1. 建设目标
1. 统一管理药食限制与营养约束。
2. 自动生成可执行菜单并联动下单。
3. 降低照护者决策压力与出错率。

## 2. 范围边界
- **In Scope**：约束建模、菜单生成、订单编排、烹饪引导。
- **Out of Scope**：处方制定、诊疗决策。

## 3. 功能需求
### Must
- FR-01：个体限制建模。
- FR-02：周菜单生成。
- FR-03：一键下单与替代品自动推荐。

### Should
- FR-04：价格优化。
- FR-05：库存与断货重规划。

### Could
- FR-06：社区团购协同。

## 4. 非功能指标
- 计划执行率 > 70%。
- 冲突饮食事件下降。
- 规则热更新不中断服务。

## 5. 总体架构
```text
家庭终端 + 规则引擎 + 约束求解器
   -> 菜单生成器
      -> 电商/药房接口
         -> 烹饪引导终端
```

## 6. 硬件设计
- 厨房显示终端：展示每日菜单与步骤。
- 可选智能秤：菜量校准与营养估算。

## 7. 软件模块
1. `ConstraintProfileService`：限制建模。
2. `MenuOptimizer`：多目标菜单求解。
3. `OrderOrchestrator`：多平台下单编排。
4. `SubstituteEngine`：断货替代规则。
5. `CookingGuideService`：步骤与用量指导。

## 8. 数据模型
- `ConstraintProfile { disease_rules, drug_food_rules, preferences }`
- `WeeklyMenuPlan { meals[], nutrition_summary }`
- `OrderOrchestration { vendors[], status, substitutions }`
- `SubstituteRule { source_sku, target_sku, rationale }`

## 9. 算法策略
- 求解目标：营养达标、冲突最小、预算可控、口味偏好满足。
- 断货策略：优先同营养层级替代。
- 学习机制：根据执行反馈调整下一周计划。

## 10. 接口定义
- `POST /api/v1/profile/constraint/update`
- `POST /api/v1/menu/generate-weekly`
- `POST /api/v1/order/place-batch`
- `POST /api/v1/menu/replan`

## 11. 交互流程
1. 同步处方与饮食限制。
2. 系统生成周菜单并解释关键约束。
3. 一键下单，缺货自动替代。
4. 每日按步骤烹饪，周末复盘执行率。

## 12. 安全与合规
- 医疗与消费数据逻辑隔离。
- 建议来源可追踪、可解释。
- 用户可导出或删除个人饮食档案。

## 13. 测试与验收
- 约束冲突测试（多病种叠加）。
- 供应链异常测试（断货、延迟）。
- 验收：计划执行率>70%，冲突事件下降。

## 14. 实施计划
- P1：一线城市商超合作版。
- P2：药房联动版。
- P3：社区团购版。

## 15. 风险与缓解
- 风险：供应不稳定。  
  缓解：备选菜单与替代规则库。
