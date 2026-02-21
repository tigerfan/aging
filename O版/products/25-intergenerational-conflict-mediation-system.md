# 产品25：跨代同住冲突感知调解系统（详细设计实现文档）

## 0. 文档信息
- 版本：v1.0
- 目标：在冲突升级前提供“降温+协商”机制
- 适用场景：多代同住家庭

## 1. 建设目标
1. 识别家庭冲突升温的早期信号。
2. 自动触发冷静期与协商脚本。
3. 将照护任务重新分配以减少长期内耗。

## 2. 范围边界
- **In Scope**：冲突风险识别、调解流程、社工协同。
- **Out of Scope**：语音内容监听与会话转写、强制干预。

## 3. 功能需求
### Must
- FR-01：冲突风险评分。
- FR-02：冷静期建议与协商脚本推送。
- FR-03：任务重分配看板。

### Should
- FR-04：家庭协商模板。
- FR-05：社工远程介入工单。

### Could
- FR-06：家庭关系阶段评估报告。

## 4. 非功能指标
- 高强度冲突事件下降 > 25%。
- 误判触发率 < 10%。
- 用户可随时关闭监测。

## 5. 总体架构
```text
家庭边缘盒(非语义声学+节律)
   -> 冲突事件引擎
      -> 成员App(协商流程)
         -> 社工后台
```

## 6. 硬件设计
- 环境声学传感器：仅提取非语义特征。
- 公共空间按钮终端：一键请求调解。
- 边缘盒：本地计算与规则执行。

## 7. 软件模块
1. `ConflictSignalExtractor`：声强/语速/重叠度提取。
2. `RoutineConflictDetector`：作息冲突识别。
3. `CooldownWorkflow`：冷静期流程。
4. `TaskBoardService`：家庭任务重分配。
5. `SocialWorkerBridge`：社工介入接口。

## 8. 数据模型
- `ConflictRiskScore { ts, score, factors[] }`
- `CooldownPlan { duration, script_id, participants[] }`
- `TaskRedistribution { task, owner_before, owner_after }`
- `SocialWorkerTicket { level, summary, status }`

## 9. 策略设计
- 风险判定：非语义声学峰值 + 作息冲突叠加。
- 触发策略：连续升温才触发干预。
- 协商机制：先暂停，后结构化议题讨论。

## 10. 接口定义
- `POST /api/v1/conflict/risk/upload`
- `POST /api/v1/conflict/cooldown/start`
- `POST /api/v1/conflict/task/redistribute`
- `POST /api/v1/social-worker/ticket`

## 11. 交互流程
1. 系统识别冲突升温信号。
2. 推送冷静期与协商脚本。
3. 家庭完成任务重分配并确认。
4. 多次失败时转社工介入。

## 12. 安全与合规
- 全员知情同意是启用前提。
- 默认不录音转写，不存会话文本。
- 审计日志仅用于流程追踪。

## 13. 测试与验收
- 家庭样本场景回放测试。
- 协商模板可执行性评估。
- 验收：冲突升级事件下降>25%。

## 14. 实施计划
- P1：自愿家庭小规模试点。
- P2：社工督导版上线。
- P3：社区服务包标准化。

## 15. 风险与缓解
- 风险：被误解为监控。  
  缓解：透明机制 + 本地处理 + 可关闭开关。
