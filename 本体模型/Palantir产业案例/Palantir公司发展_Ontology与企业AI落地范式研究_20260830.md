# Palantir 公司发展、Ontology 与企业 AI 落地范式研究

> 版本：v1.1  
> 日期：2026-08-30  
> 研究主题：Palantir 从 Gotham、Foundry、Ontology 到 AIP 的产品演进；Ontology 的内部结构；Palantir FDE 范式与常见企业 RAG/Agent 落地方法的差异；开发者生态、市场推广与平台战略。  
> 配套案例研究：[Palantir产业AI案例拆解_审核修订版_v0.2_20260829.md](./Palantir产业AI案例拆解_审核修订版_v0.2_20260829.md)

---

## 摘要

Palantir 二十多年的产品演进，可以理解为围绕同一个问题不断扩大适用范围：**怎样把分散、异构、敏感的数据，转化为一个与现实业务相对应、能够支持判断、受权限约束并可以驱动实际行动的数字世界。**

它最初在反恐和情报场景中解决“如何从大量割裂的数据中识别现实对象、关系和威胁”，随后通过 Gotham 把分析与任务执行连接起来；2016 年推出 Foundry，将这一方法推广到能源、制造、航空、医药、金融等大型组织；Ontology 逐渐成为整个架构的核心，用对象、关系、逻辑、动作和安全策略描述企业的运行世界；2023 年推出 AIP 后，原先主要供人和软件使用的 Ontology 又成为 AI Agent 获得企业上下文、调用业务能力和受控采取行动的基础。

因此，Palantir 的演进不是简单的产品替代：

```text
Gotham → Foundry → Ontology → AIP
```

更准确的关系是：

```text
Gotham 验证方法
        ↓
Foundry 将方法平台化、企业化
        ↓
Ontology 成为数据、逻辑、行动和安全的共同抽象
        ↓
AIP 让 LLM 和 Agent 可以在这个业务世界中受控地理解、推理和行动
```

本研究的核心判断是：

1. **Foundry 不是一个“企业知识图谱”，而是一个数据与运营平台；Ontology 才是其中最接近企业业务世界模型的核心层。**
2. **把 Ontology 称为“AI 可以理解的企业知识图谱”基本正确，但仍然低估了它。**传统知识图谱主要表达对象和关系，Palantir Ontology 还表达 Function、Action、权限、实时状态、写回和决策历史。
3. **AIP 的价值不只是把大模型连接到企业数据，而是把企业的数据、确定性逻辑、模型、业务动作和权限转化为 Agent 可以安全使用的上下文与工具。**
4. **普通企业 RAG/Agent 团队与 Palantir FDE 的主要区别，不在于是否使用向量库，而在于前者通常围绕一个 AI 应用连接上下文，后者围绕一个真实业务决策建立可运行的业务世界和行动闭环。**
5. **Palantir 的优势并非某一种单独技术不可复制，而是将数据平台、Ontology、应用、AI、权限、审计、部署和 FDE 交付方式集成为一套体系。**其代价是平台投入高、Ontology 治理复杂、实施依赖强，并存在供应商锁定风险。
6. **Palantir 正在把这种高度依赖 FDE 的交付体系扩展为一个以 Ontology 为共同底座的开发和分发生态。**其增长机制逐渐从“Palantir 工程师为客户建设应用”，扩展为“客户开发者、合作伙伴和 AI FDE 共同建设、封装、复用和分发应用与业务能力”；但公开开发者经济、独立 ISV 商业模式和 Marketplace 的开放度仍处于早中期。

---

## 一、研究范围、证据边界与基本概念

### 1.1 研究范围

本文集中回答五个问题：

1. Palantir 从 2003 年成立到今天，产品能力为什么会从 Gotham 演进到 Foundry、Ontology 和 AIP？
2. Ontology 的对象、关系、Function、Action、安全和应用，怎样构成同一个业务闭环？
3. 常见企业 RAG/Agent 落地范式与 Palantir Ontology/FDE 范式，有哪些本质区别？
4. Palantir 方法真正值得借鉴的是什么，又有哪些边界和风险？
5. Palantir 如何建设开发者、产品和合作伙伴生态，又如何通过活动、Bootcamp、FDE 与客户案例推广这套方法？

本文暂不对 Palantir 的估值、股票投资价值、政治立场和具体政府项目作系统研究；客户案例的事实审核见配套案例文件。

### 1.2 证据类型

本文主要使用三类资料：

- **监管与财务文件**：SEC 上市文件、Palantir 年报，用于确认成立时间、产品发布时间、客户和收入等事实。
- **Palantir 官方技术文档**：用于理解 Foundry、Ontology、AIP、Action、Function、安全和应用的当前产品定义。
- **分析性判断**：基于产品结构和案例形成的方法论推论，不等同于 Palantir 已经在所有客户中实现的结果。

需要特别说明：Palantir 官方技术文档具有较强的厂商叙事色彩。本文可以依据这些文档判断“Palantir 如何定义和设计自己的产品”，但不能仅据此证明所有客户都获得了相应业务价值。客户价值必须结合合同、客户证言、独立材料和可验证指标分别判断。

### 1.3 几个容易混淆的概念

| 概念 | 本文中的含义 |
|---|---|
| Gotham | Palantir 最早面向情报、国防和政府行动场景的平台，强调多源数据融合、实体关系、态势理解和任务执行。 |
| Foundry | 面向企业和政府的数据运营平台，涵盖数据接入、转换、治理、分析、模型、Ontology、应用和工作流。 |
| Ontology | 将现实业务映射为对象、关系、逻辑、动作和安全策略的运行系统，是 Foundry/AIP 架构的核心。 |
| AIP | 将 LLM、生成式 AI、Agent、自动化、评估和可观测能力连接到 Foundry/Ontology 的 AI 平台。 |
| Apollo | 管理 Palantir 及客户软件在云、私有环境、边缘和隔离网络中持续部署和运行的平台。 |
| FDE | Forward Deployed Engineering，工程师贴近客户真实业务现场，快速构建软件并把现场反馈传回核心产品团队的交付方法。 |

---

## 二、Palantir 的产品演进

### 2.1 2003—2008：从反恐问题出发，形成最初的产品基因

Palantir 于 2003 年成立，最初目标是为美国情报机构和反恐行动开发软件。Palantir 在 2020 年上市文件中把当时的问题描述为：分析人员不是在一个草堆中找一根针，而是在成百上千个草堆中寻找相关线索。

数据可能来自：

- 信号情报；
- 人工报告；
- 地理位置；
- 交易记录；
- 组织和人物信息；
- 不同机构、不同安全等级的数据库。

这类问题不能只靠建立一个更大的数据库解决。它要求系统同时处理：

1. **异构数据融合**：不同数据结构、不同来源、不同质量的数据需要共同分析。
2. **实体解析**：多个系统中的不同记录是否指向同一个人、组织、地点或事件。
3. **关系网络**：对象之间怎样关联，某条线索怎样沿关系网络扩散。
4. **来源与可信度**：信息从哪里来，是否经过修改，哪些结论依赖了哪些数据。
5. **细粒度权限**：一个分析员可以看到什么、为什么能看到、能否分享。
6. **人机协同**：算法可以发现模式，但重大判断和行动仍需要人的参与。
7. **分析到行动**：发现威胁之后，如何把分析结果交给实际执行人员。

这些要求后来一直保留在 Gotham、Foundry、Ontology 和 AIP 中。换言之，Palantir 最早形成的并不是“大数据可视化”能力，而是一套关于**如何用软件表示复杂现实、支持人在约束条件下作出行动决定**的方法。

### 2.2 2008—2015：Gotham 将实体关系分析连接到现实行动

Palantir 于 2008 年发布 Gotham。早期 Gotham 的代表性能力包括：

- **Graph**：探索实体、属性和关系网络；
- **Gaia**：在实时地图上组织对象、态势和任务；
- **Dossier**：以动态协作文档沉淀分析过程；
- **Stencil**：用结构化表单采集现场输入；
- 对历史和实时视频、地理信息等多模态数据进行分析。

Gotham 的重要意义不只在于“把数据画成图”，而在于形成了一条完整链路：

```text
多源数据
  ↓
识别现实对象和关系
  ↓
形成共同态势
  ↓
分析人员作出判断
  ↓
行动人员规划和执行
  ↓
现场信息重新进入系统
```

Ontology 的早期基因已经存在于这里：现实中的人、地点、事件和装备被抽象成对象；关系被显式建模；分析和执行围绕这些对象展开。只是此时产品主要服务于国防和情报工作。

### 2.3 从政府到商业：发现大型企业面临同构问题

在随后进入能源、交通、金融、医疗和制造等行业时，Palantir 发现这些企业虽然不从事情报行动，却面临结构相似的问题：

- 数据散落在 ERP、MES、CRM、供应链、实验室、传感器和大量自建系统中；
- 不同部门对订单、客户、设备、产品等核心概念定义不一致；
- 数据团队建设了数据仓库和报表，业务人员仍无法据此完成实际工作；
- 模型停留在数据科学环境，不能进入日常运营；
- 决策过程和人工经验没有回到数据系统，难以持续学习；
- 旧系统不能轻易替换，但跨系统决策必须发生。

这些问题说明，企业需要的不只是分析工具，而是一个连接数据、模型、人员、应用和现有执行系统的共同操作层。

### 2.4 2016—2022：Foundry 将方法平台化、企业化

Palantir 于 2016 年发布 Foundry。根据其上市文件，Foundry 是为大型企业普遍存在的数据和运营问题而建立的第二个平台。

Foundry 大致可以分为以下能力层：

#### 数据接入与管理

- 连接数据库、SaaS、文件、数据湖和实时数据流；
- 建设数据流水线；
- 进行数据清洗、转换和质量监控；
- 维护数据血缘、版本和依赖关系；
- 支持开放格式、API和外部计算环境。

#### 分析与逻辑

- 指标、规则和计算；
- Python、TypeScript 等代码逻辑；
- 传统机器学习模型；
- 预测、优化和仿真；
- 外部模型和计算系统。

#### Ontology

- 把表和字段映射为业务对象、属性和关系；
- 把规则和模型绑定到对象；
- 把业务动作定义为 Action；
- 为对象、逻辑和动作配置安全策略。

#### 应用与工作流

- 对象搜索和对象档案；
- 业务分析和可视化；
- 低代码运营应用；
- 自定义应用和外部 API；
- 将业务决定写回 Ontology 或外部系统。

因此，Foundry 不是只服务数据工程师的数据平台，也不是单纯的 BI 或机器学习平台。Palantir 试图让数据工程师、分析师、数据科学家、业务人员和管理者在同一业务模型上协作。

### 2.5 Ontology 为什么成为 Foundry 的核心

传统数据平台常以表、字段、数据集和文件为主要操作单位，但业务人员思考的是：

- 哪个客户；
- 哪张订单；
- 哪台设备；
- 哪个航班；
- 哪批产品；
- 哪个异常；
- 可以采取什么行动。

Ontology 的作用是把底层技术结构翻译成现实业务结构，并进一步把业务动作和权限也纳入同一个模型。

它解决的不是单纯“数据怎样统一”，而是：

> 数据工程师、业务人员、应用、模型和 AI Agent，怎样围绕同一个现实对象和同一套业务能力工作。

### 2.6 Apollo：从内部交付基础设施发展为平台

Palantir 的客户环境包括公有云、私有云、客户数据中心、边缘设备和与互联网隔离的网络。为了让 Gotham 和 Foundry 在这些差异巨大的环境中持续升级，Palantir 建立了自动化的软件发布和运行管理能力，后来将其产品化为 Apollo。

Apollo 不是业务语义层，而是整个产品体系的持续交付和运行底座。它使 Palantir 能够将持续更新的软件部署到对安全、稳定性和环境隔离要求极高的场景中。

### 2.7 2023—至今：AIP 让 AI 进入企业运营

Palantir 于 2023 年推出 AIP。AIP 不是一个自研基础大模型，也不是一个单独覆盖在数据库上的聊天机器人。它主要提供：

- 安全接入商业模型、开源模型和客户自有模型；
- 将结构化和非结构化上下文连接到 Ontology；
- 建设 LLM Function、Agent 和自动化；
- 将 Ontology 中的 Function 和 Action作为 Agent 工具；
- 对 Agent 工作流进行日志、追踪、评估和监控；
- 配置人工确认、审批和交接；
- 将 AI 生成的建议连接到实际业务动作。

AIP 的战略意义在于：Palantir 不需要从零开始为 AI 构建一个企业业务模型。此前为了人、应用和传统模型建设的 Ontology，恰好可以成为 Agent 的业务上下文和行动边界。

这说明 Ontology 最初并不是专门为大模型设计的。它首先解决的是人与数据、应用与现实业务之间的语义断层；大模型出现之后，同一套抽象又解决了 AI 与企业现实之间的断层。

### 2.8 当前产品体系：四个平台与三层架构

Palantir 2025 年年报仍将产品列为四个主要平台：

- Gotham；
- Foundry；
- Apollo；
- AIP。

但其当前技术架构文档又经常将标准架构概括为三部分：

- Foundry：Data Operations；
- AIP：Generative AI；
- Apollo：Continuous Delivery。

这两种说法并不矛盾。前者是产品口径，后者是通用技术架构口径。Gotham 可以理解为在共同技术底座之上，面向国防、情报和政府任务的领域产品。

截至 2025 年底，Palantir 年报披露客户数为 954，2025 年收入为 45 亿美元，其中政府客户收入占 54%，商业客户收入占 46%。这些数字可以说明 Palantir 已经从早期高度集中的政府软件公司发展为政府与商业并重的平台企业，但不能单独证明某一技术方法的优越性。

---

## 三、Ontology 的准确定位

### 3.1 Foundry 能否理解为“让 AI 理解企业的知识图谱”

这个说法可以作为入门类比，但需要两处修正：

第一，真正接近“企业知识图谱”的是 Ontology，而不是整个 Foundry。Foundry 还包括数据工程、模型开发、应用、治理、运维等大量能力。

第二，“知识图谱”通常主要表达对象和关系，而 Palantir Ontology 还表达动作、逻辑、安全和实时运行状态。

更准确的表述是：

> **Foundry 是构建和运行企业数字世界的平台；Ontology 是这个数字世界的语义与行动模型；AIP 让 AI 可以在其中受控地理解、推理和行动。**

### 3.2 Ontology 不是静态数据模型

传统知识图谱重点回答：

- 有哪些实体；
- 实体有什么属性；
- 实体之间有什么关系。

Palantir Ontology 还要回答：

- 对象现在处于什么状态；
- 哪些状态来自哪些数据源；
- 可以对对象采取什么动作；
- 动作由什么规则、模型或优化器支持；
- 谁能看到哪些对象和属性；
- 谁能执行哪些动作；
- 动作如何写回现有系统；
- 决策之后的结果如何重新进入系统。

因此可以使用以下近似公式：

```text
Palantir Ontology
= 企业知识图谱
+ 实时业务状态
+ 规则/模型/优化器
+ 业务动作API
+ 权限与审计
+ 应用与写回机制
```

Palantir 当前更强调 Ontology 是一个“decision-centric system”，即以企业决策而不是单纯数据为中心。

### 3.3 Ontology 的三个系统层面

Palantir 当前将 Ontology 概括为 Language、Engine 和 Toolchain：

#### Ontology Language

用于定义：

- 对象类型；
- 属性；
- 关系；
- Interface；
- Function；
- Action；
- 自动化；
- 安全策略。

它相当于企业业务世界的表达语言。

#### Ontology Engine

用于让这些定义成为可运行的系统，包括：

- 对象查询；
- 关系遍历；
- 实时订阅；
- 批量和流式更新；
- 原子、持久的 Ontology 修改；
- CDC 和外部系统同步；
- 权限运行时计算。

#### Ontology Toolchain

用于让开发者和业务人员建设、管理和使用 Ontology，包括：

- Ontology Manager；
- Function 开发环境；
- Ontology SDK；
- Workshop 等应用构建工具；
- 版本、分支、测试和发布；
- 可观测和审计工具。

---

## 四、Ontology 的内部结构

### 4.1 Object Type、Object 与 Property

Ontology 首先用对象类型表示现实实体或事件。例如：

- `Flight` 是对象类型；
- 某天的 CA123 航班是一个对象实例；
- `Aircraft` 是对象类型；
- 编号 B-1234 的飞机是一个对象实例。

对象可以是实体，也可以是事件、交易、告警和任务。每个对象类型通常包含：

- 主键；
- 显示名称；
- 属性及其数据类型；
- 背后的数据源；
- 可见性和状态；
- 是否允许写回。

例如：

```text
Flight
├── flightId
├── origin
├── destination
├── scheduledDeparture
├── estimatedDeparture
├── status
├── passengerCount
└── disruptionRisk
```

Property 不一定直接来自一张原始表。它可以来自：

- 业务系统中的字段；
- 多个数据源整合后的结果；
- 实时数据流；
- 用户输入；
- Function 的计算；
- Action 产生的修改。

所以 Ontology 对象不是简单把数据库行换一个名字，而是围绕现实业务对象组织事实、状态、计算结果和人工决策。

### 4.2 Link Type 与 Link

Link Type 定义两个对象类型之间存在什么业务关系。例如：

```text
Flight ──使用──> Aircraft
Flight ──配置──> Crew
Flight ──产生──> FlightAlert
Passenger ──预订──> Flight
Aircraft ──受到──> MaintenanceRestriction
```

关系可以是一对一、一对多或多对多，背后可能来自外键、关联表或其他对象数据源。

它与普通数据库 Join 的区别是：Join 说明两组记录在技术上怎样连接，Link Type 还显式说明这条连接在业务上意味着什么。相同两类对象之间可以存在多种不同关系，应用和 Function 可以按照明确业务名称访问这些关系。

### 4.3 Interface：跨对象类型复用共同能力

Interface 描述不同对象类型共有的结构和能力。例如 Flight Alert、Maintenance Alert、Weather Alert 都可以实现 `Alert` Interface：

```text
Alert
├── severity
├── status
├── createdTime
├── assignedTo
└── resolve 能力
```

这样应用、Function 和 Action 可以针对所有 Alert 工作，而不必为每一种具体告警重复实现逻辑。Interface 相当于在企业业务模型中建立了可复用的能力契约。

### 4.4 Function：计算、规则与模型

Function 是 Ontology 的逻辑部分，可以：

- 读取对象属性；
- 遍历对象关系；
- 搜索对象集合；
- 计算指标；
- 调用传统机器学习模型；
- 调用优化器和仿真；
- 访问外部系统；
- 为应用返回结果；
- 为复杂 Action 生成一组对象修改。

例如：

```text
calculateDelayImpact(flight)
findReplacementAircraft(flight)
estimatePassengerMisconnections(flight)
rankRecoveryOptions(flight)
```

Function 的重要特征是直接使用 Ontology 类型。开发者看到的不是含义模糊的表名和字段，而是 Flight、Aircraft、Crew 等业务对象及其关系。

Function 不一定改变数据。它可以只读并返回分析，也可以被 Action 调用来完成复杂修改。

### 4.5 Action：把业务决定变成受控状态改变

Action 是 Palantir Ontology 与普通知识图谱差异最大的部分。它表达一项有业务含义的动作，例如：

```text
DelayFlight
CancelFlight
AssignReplacementAircraft
ResolveAlert
ApproveRecoveryPlan
```

一个 Action 通常包含：

#### Parameters

动作需要哪些输入。例如 `DelayFlight` 可能需要：

- flight；
- newDepartureTime；
- delayReason；
- decisionComment；
- notifyPassengers。

这些参数同时成为业务表单、API 和 Agent 工具调用的接口。

#### Submission Criteria

什么情况下允许提交。例如：

- 当前用户必须具有运营主管身份；
- 航班尚未起飞；
- 新起飞时间必须晚于计划时间；
- 延误时长不能超过当前用户的授权范围。

这些条件属于 Action 本身，而不是某个前端页面的临时校验，因此从其他应用、API 或 Agent 调用时同样有效。

#### Rules

提交后改变什么，例如：

- 修改 Flight 状态；
- 创建 Disruption 对象；
- 建立新的关系；
- 更新相关告警；
- 生成通知任务。

#### Function-backed Action

当动作需要读取多个对象、调用模型、批量修改或执行复杂分支逻辑时，可以由 Function 支持。简单说：

- Function 负责“怎样计算和判断”；
- Action 负责“是否允许以一项业务决定提交这些改变”。

#### Side Effects

Action 还可以调用外部 API、发送通知、触发构建或写回 ERP 等系统。需要注意，Ontology 内部对象修改可以按一次原子变更提交，但外部系统调用不应被简单理解为全部参与同一个全球数据库事务；仍需要处理失败、重试、补偿和监控。

### 4.6 Security：不是外围防火墙，而是业务模型的一部分

Ontology 的安全可以分别控制：

1. 谁可以查看或修改对象类型、关系和 Action 的定义；
2. 谁可以看到哪些对象实例；
3. 谁可以看到对象的哪些属性；
4. 谁可以调用哪些 Function；
5. 谁可以执行哪些 Action；
6. 在什么对象状态、参数和业务上下文中可以执行；
7. Agent 以哪个用户或项目身份运行；
8. 日志和模型调用记录由谁查看。

例如，一个用户可以看到航班状态并运行延误模拟，但不能查看旅客个人信息，也不能取消航班。另一个高级主管可以取消航班，但超过特定成本的方案仍需二次审批。

这说明安全不仅回答“能否访问数据”，还回答：

> 谁可以对哪个对象，在什么状态和参数条件下，采取什么行动。

### 4.7 Applications：不是展示层，而是业务决策入口

Palantir 的 Ontology-aware 应用直接绑定对象、关系、Function 和 Action。常见工具包括：

- Object View：单个对象的业务档案与相关工作；
- Object Explorer：搜索、筛选和探索对象；
- Quiver：对象分析和可视化；
- Workshop：低代码运营应用；
- Slate、OSDK：更复杂的定制应用；
- Map：地理态势和空间工作流；
- 外部应用：通过 Ontology API 工作。

一个运营应用可以同时显示异常对象、关联对象、Function 计算的方案和可执行 Action。业务人员在应用中作出的决定写回 Ontology，新的对象状态又会被其他应用和人员看到。

Palantir 因此区分两类系统：

- Dashboard：展示信息；
- Operational Application：展示信息，同时捕捉并执行决定。

---

## 五、对象、Function、Action、安全和应用怎样构成业务闭环

以航班延误处置为例：

### 第一步：现实状态进入 Ontology

天气、机场、飞机、机组和旅客系统持续更新对象：

```text
Flight 123
状态：Scheduled
计划起飞：10:00
预计起飞：10:00
使用飞机：Aircraft B-1234
相关告警：Thunderstorm Alert 987
```

### 第二步：Function 计算问题与方案

Function 读取相关对象，调用天气模型、网络影响模型和资源优化器：

```text
预计最早起飞时间：11:10
受影响旅客：168
可能错过转机：34
继续使用原飞机的综合成本：80
更换飞机的综合成本：65
取消航班的综合成本：210
```

### 第三步：应用组织决策情境

运营主管在应用中看到：

- 异常航班；
- 相关飞机、机组和旅客；
- 不同恢复方案；
- 每个方案的成本、约束和后果。

### 第四步：人或 Agent 提出 Action

主管或 Agent 提出：

```text
AssignReplacementAircraft
```

输入目标航班、替换飞机和原因。

### 第五步：安全和提交条件检查

系统验证：

- 当前身份是否可以看到相关对象；
- 是否具有更换飞机的权限；
- 飞机是否可用；
- 型号和航线是否匹配；
- 是否存在检修限制；
- 机组资质是否匹配；
- 是否与其他调度决定冲突；
- 是否需要更高级别审批。

### 第六步：执行和写回

Action 修改 Flight 与 Aircraft 的关系，更新对象状态，解决告警，生成调度记录，并将结果同步到外部航班运行和通知系统。

### 第七步：结果成为新状态和反馈

```text
Flight 123
新飞机：B-5678
预计延误：25分钟
可能错过转机人数：由34降为5
```

系统同时记录：

- 谁作出决定；
- 当时的数据和对象状态；
- 使用了哪些 Function 和模型；
- 提出过哪些方案；
- 最终执行了哪个 Action；
- 实际结果与预测有何差异。

完整闭环是：

```text
事实 → 分析 → 方案 → 决定 → 执行 → 结果 → 新事实
```

Ontology 真正的统一性不一定意味着所有数据都搬入一个数据库，而是所有参与者使用同一套业务契约：

| 参与者 | 共同使用的 Ontology 能力 |
|---|---|
| 数据工程师 | Object、Property、Link 和数据映射 |
| 算法工程师 | Ontology Types、Functions、Models |
| 应用开发者 | Objects、Functions、Actions、OSDK |
| 业务人员 | 对象视图、方案和业务动作 |
| 安全管理员 | 对象、属性、逻辑和动作权限 |
| AI Agent | 可读取的对象、可调用的 Function 和 Action |

---

## 六、为什么 Ontology 特别适合企业 AI

### 6.1 为 AI 提供稳定的业务语言

企业底层数据经常以技术结构存在：表、字段、接口、日志和文件。Ontology 将它们转换成稳定的业务对象和关系，使 LLM 不必直接猜测每张表和字段的含义。

这并不表示 LLM 真正获得了人的业务理解，而是它获得了更明确、类型化、受权限控制的上下文和工具契约。

### 6.2 为 AI 提供当前业务状态

向量检索适合找到相关文本，但“语义相关”不等于“当前有效”。Ontology 可以把实时或近实时状态绑定到具体对象，使 AI 知道自己面对的是哪张订单、哪台设备、哪个版本的计划。

### 6.3 为 AI 提供确定性逻辑

严肃业务不能把所有判断都交给 LLM。Ontology 可以把业务规则、传统模型、预测、优化器和仿真作为 Function 提供给 Agent。

合理分工是：

```text
LLM：理解、综合、解释、规划
业务规则：确定性约束
传统模型：预测
优化器：求解
Function：封装逻辑
Action：控制执行
人：审批高风险决定
```

### 6.4 为 AI 提供受控行动空间

AI 不是直接获得数据库写权限或任意 API 权限，而是通过预定义 Action 工作。Action 的参数、提交条件、权限和审计共同约束 Agent 能做什么。

### 6.5 为 AI 建立反馈和学习闭环

一次决策的输入、建议、执行和结果可以继续成为 Ontology 中的数据。这样企业可以分析：

- 哪类建议经常被采纳；
- 哪类建议被拒绝；
- 哪种模型在什么情境下有效；
- 实际结果与预测差异；
- 哪些业务规则需要调整。

这比只保存对话历史更接近企业层面的学习。

---

## 七、企业 AI 的两种典型落地范式

### 7.1 对“两种方法论”假设的修正

一种常见说法是：

1. 普通开源 Agent：LLM → 向量库 → 返回结果；
2. Palantir：LLM/Agent → Ontology → 底层数据库。

这个二分法在战略层面有解释力，但技术上过度简化。

LangChain 不是只能连接向量库。它可以连接 SQL、CRM、API、搜索、文件和各种工具；LangGraph 也支持有状态工作流、持久化和人工审批。向量库只是 RAG 的一种常见组件。

Palantir Ontology 也不是单纯的数据库中间层。它可能连接：

- 数据库和数据湖；
- 文档和向量检索；
- 实时流和 CDC；
- 规则、机器学习模型和优化器；
- ERP、CRM、MES 和外部 API；
- 边缘设备和业务应用。

因此，更准确的对照是：

#### 上下文增强型 AI

```text
围绕一个AI应用
→ 按需连接知识、数据和工具
→ 生成答案或完成任务
```

#### 业务世界型 AI

```text
围绕企业关键决策
→ 建立对象、逻辑、动作和权限
→ 人、应用和Agent共同使用
→ 决策与执行结果持续写回
```

前者可以称为 RAG/Agent-first，后者可以称为 Ontology/Operational AI-first。

### 7.2 两种技术链路

#### 常见 RAG/Agent-first

```text
用户问题
  ↓
Agent
  ├── 文档/向量库
  ├── SQL数据库
  ├── 搜索引擎
  ├── CRM/ERP API
  ├── 计算工具
  └── 外部服务
  ↓
LLM综合结果
  ↓
答案 / 结构化输出 / 工具调用
```

#### Palantir Ontology/AIP-first

```text
用户或业务事件
  ↓
应用 / Agent
  ↓
Ontology
  ├── Objects：业务对象和状态
  ├── Links：业务关系
  ├── Functions：规则、模型、优化器
  ├── Actions：允许执行的业务动作
  └── Security：数据和动作权限
  ↓
数据、模型、ERP、API、实时流和边缘系统
  ↓
执行结果与反馈写回
```

### 7.3 比较对象必须保持公平

LangChain/LangGraph 是开发和编排框架；Palantir 是 Foundry、Ontology、AIP、Apollo 及 FDE 组织的组合。因此真正公平的比较不是“LangChain vs Palantir”，而是：

> 企业自建的 RAG/Agent 技术栈及落地团队  
> 对比  
> Foundry+AIP+Ontology+FDE 的一体化交付体系。

---

## 八、普通企业 AI 团队与 Palantir FDE 范式的详细比较

| 维度 | 常见企业 RAG/Agent 团队 | Palantir Ontology/FDE 范式 |
|---|---|---|
| 核心目标 | 让模型获得企业上下文，完成问答或任务 | 建立可执行的企业业务世界，让人和 AI 共同决策和行动 |
| 项目起点 | 想做什么AI助手 | 哪一个关键业务决定正在失效 |
| 建模来源 | 文档、用户问题、数据表、API、应用需求 | 现实对象、业务约束、决策过程、动作和责任边界 |
| 基本单位 | 文档块、Embedding、消息、工具、工作流节点 | 对象、关系、状态、Function、Action、安全策略 |
| AI与数据联系 | 检索、SQL、工具调用，连接方式通常属于具体应用 | 通过统一Ontology访问对象、逻辑和动作 |
| 语义统一 | 常在Prompt、工具描述和应用代码中局部实现 | 通过Ontology统一定义并供多应用、多Agent复用 |
| 非结构化数据 | 通常以RAG为主 | 同样可使用RAG，但检索结果可以绑定到业务对象和决策 |
| 数据实时性 | 向量索引依赖同步；数据库工具可实时查询 | 可结合批处理、流和CDC，但实际时效仍取决于数据接入设计 |
| 输出边界 | 文本、引用、JSON和工具调用 | 查询、解释、模拟、建议、审批、对象修改和系统写回 |
| 行动机制 | 开发者为每个Agent包装工具或API | Action是一等业务能力，具有参数、规则、权限和提交条件 |
| 确定性逻辑 | 容易把较多判断放入LLM或应用代码 | LLM与规则、模型、优化器、Function组合 |
| 安全 | 向量库、API、Agent和应用通常分别配置 | 对象、属性、逻辑、Action和Agent统一受治理 |
| 迭代对象 | Prompt、Embedding、分块、检索、工具和Agent图 | 数据映射、Ontology、Function、Action、应用和流程 |
| 反馈指标 | 回答准确率、召回率、工具成功率、用户反馈 | 还包括决定是否采纳、执行结果、对象状态变化和运营KPI |
| 审计重点 | AI程序如何运行 | 企业基于什么状态作了什么决定并改变了什么 |
| 复用方式 | 复用Agent组件、工具和模板 | 复用统一对象、关系、Function和Action |
| 组织方式 | 业务、AI、数据、IT、安全多团队交接 | FDE贴近业务现场，与客户和核心工程团队共同迭代 |
| 首个Demo | 通常非常快 | 真实闭环需要更多基础工作，但可通过薄切口加速 |
| 第N个场景 | 容易重复建设知识库、接口和权限 | Ontology成熟后可复用既有业务能力 |
| 初期成本 | 相对低 | 平台、交付和组织投入较高 |
| 长期成本 | 集成、治理、维护和Agent烟囱成本可能快速增加 | 目标是通过共同底座降低边际成本，但Ontology治理本身也昂贵 |
| 主要风险 | Demo停留在问答；工具权限过大；形成新烟囱 | 过度建模；实施依赖；平台锁定；语义治理成为瓶颈 |

### 8.1 核心重点和建模来源

#### 常见企业 AI 团队

通常从以下问题开始：

- 用户会问什么；
- 有哪些文档；
- 怎样分块和Embedding；
- 需要哪些工具；
- Prompt怎样设计；
- 怎样减少幻觉。

其建模来源主要是用户问题、可获得数据和AI应用需求，形成以AI应用为中心的上下文模型。

#### Palantir FDE

更典型的问题是：

- 谁在什么时间作出哪项决定；
- 决策时需要看哪些对象；
- 信息和流程为什么失效；
- 有哪些业务规则、模型和约束；
- 决定后要修改哪些系统；
- 结果怎样反馈回来。

其建模来源是实际业务矛盾和一线决策过程，形成以运营决策为中心的业务世界模型。

重要的是，FDE 方法不应被理解为先建设一个包罗万象的全企业 Ontology。合理方式是围绕一个高价值决策，建立最小必要闭环：

```text
关键问题
→ 最少必要对象
→ 最少必要关系
→ 关键Function
→ 可控Action
→ 一个可运行应用
→ 业务结果
```

验证价值后，再将对象和能力扩展、复用。这可以概括为：**纵向切入，横向沉淀。**

### 8.2 AI 与数据之间的联系

RAG 主要建立“问题与相关内容”的语义关系，擅长找到可能包含答案的文档片段。但语义相关不天然等于：

- 当前有效；
- 属于哪个现实对象；
- 满足业务约束；
- 当前用户可以执行；
- 可以安全写回。

Ontology 则建立 AI 与业务对象、当前状态、关系、逻辑和动作之间的连接。AI 读取的不只是订单描述，而是具有明确类型、状态、关系和允许动作的 Order 对象。

这并不排斥 RAG。更合理的组合是：

```text
合同或报告片段
→ 绑定到Contract对象
→ 关联Customer和Order
→ 影响某条规则或Function
→ 决定是否允许某个Action
```

可以概括为：

- RAG 解决“找到相关知识”；
- Ontology 解决“把知识放回业务现实”。

### 8.3 输出边界

基础 RAG 的输出主要是答案、摘要、引用、报告和结构化抽取。成熟 Agent 也可以执行数据库、邮件、工单和业务 API，因此不能把开源 Agent 限定为只回答问题。

差别在于，普通 Agent 的行动边界通常由开发者为这个应用配置了哪些工具决定；Ontology Agent 的行动边界由企业统一定义的 Action、对象状态、权限和提交条件决定。

Palantir 范式可以逐步形成五级输出：

1. 查询当前状态；
2. 解释原因；
3. 模拟不同方案；
4. 提出受约束建议；
5. 经过授权后执行并写回。

### 8.4 迭代模式

常见 AI 团队主要迭代：

- Prompt；
- 分块和Embedding；
- 检索和重排；
- Agent路由；
- 工具描述；
- 模型选择；
- 延迟和Token成本。

Palantir FDE 除了这些，还会根据现场反馈调整：

- Object 定义；
- Link 关系；
- 数据映射；
- Function 和模型；
- Action 约束；
- 应用工作流；
- 权限；
- 人机交接节点。

两者最简洁的区别是：

- RAG团队主要迭代“AI怎样回答和调用工具”；
- FDE主要迭代“业务世界怎样被建模、判断和改变”。

成熟的开源 Agent 团队同样可以使用 LangSmith 等系统进行Trace、离线评估、在线评估和回归测试。Palantir 的差异主要在于，将AI评估进一步放到Ontology、业务动作和运营结果的共同上下文中。

### 8.5 审计追溯性

普通 Agent 可观测平台擅长记录 Execution Trace：

```text
用户输入
→ Prompt
→ 模型调用
→ 检索结果
→ 工具调用
→ 工具返回
→ 最终输出
```

它回答“这个 AI 程序怎样运行”。

Palantir 更强调 Decision Lineage：

```text
谁
→ 在什么时间
→ 基于哪个版本的数据和对象状态
→ 通过哪个应用或Agent
→ 使用了什么Function和模型
→ 提出了什么方案
→ 经过什么权限和审批
→ 执行了什么Action
→ 修改了哪些对象和系统
→ 最终业务结果是什么
```

它回答“企业为什么作出这个决定，这个决定改变了什么”。

因此：

- Trace 是模型和程序行为追溯；
- Decision Lineage 是企业决策追溯。

自建体系也可以实现 Decision Lineage，但通常需要自行打通 Agent Trace、数据血缘、数据库审计、IAM、API 网关、工作流、审批和业务日志。Palantir 的优势是将这些能力集成在同一平台治理体系中。

### 8.6 安全

普通 Agent 的安全常包括：

- 检索权限过滤；
- API Token；
- 工具白名单；
- Guardrail；
- PII过滤；
- 高风险工具人工批准。

问题在于权限容易分散在向量库、数据库、API、Agent和应用中。

Ontology 安全进一步控制：

- Agent能看哪些对象和属性；
- 能运行哪些Function；
- 能执行哪些Action；
- 在什么参数和对象状态下执行；
- 是否必须人工审批；
- 日志由谁查看。

因此普通 AI 安全往往以“模型可以访问什么数据”为中心，Ontology 安全还以“模型可以形成和执行什么决定”为中心。

### 8.7 确定性逻辑与 LLM 的地位

一些 Agent 项目容易让 LLM 同时负责理解问题、判断事实、选择工具、计算方案和决定执行，导致 LLM 权力过大。

Ontology 范式更适合把 LLM 放在混合决策体系中：

- LLM 负责非确定性理解和规划；
- 规则负责确定性约束；
- 传统模型负责预测；
- 优化器负责求解；
- Action 负责受控执行；
- 人负责高风险审批。

LLM 不是系统记录，也不天然是最终授权者。

### 8.8 复用和规模化

普通企业容易逐渐出现财务助手、法务助手、客服助手、采购助手和研发助手，每个助手都有自己的知识库、Prompt、接口、工具和权限，从 SaaS 烟囱走向 Agent 烟囱。

成熟平台团队可以通过统一数据平台、工具注册、MCP、身份系统和工作流缓解这个问题，但需要自行设计。

Ontology 范式希望多个应用和 Agent 复用同一个 Customer、Order、Inventory 对象以及同一个风险计算 Function、审批 Action。第一个闭环更重，但第 N 个场景有机会复用已有资产。

反面是 Ontology 治理也可能成为新瓶颈：

- 部门争夺对象定义权；
- 过度追求全局统一；
- 错误语义被多个应用复用；
- Schema 变更影响大量依赖；
- 平台团队重新成为中央排队部门。

### 8.9 组织方式与 FDE

普通企业 AI 项目常见流程是：

```text
业务提出需求
→ AI团队做Demo
→ 数据团队提供数据
→ IT团队接系统
→ 安全部门审批
→ 业务部门试用
```

这种模式容易产生多次交接：AI团队不懂业务隐性规则，数据团队只负责数据，IT担心Agent影响生产系统，Demo完成后没有团队负责最后一公里。

FDE 的理想角色同时承担：

- 理解业务矛盾；
- 接入数据；
- 建设 Ontology；
- 编写 Function 和 Action；
- 构建应用；
- 跟踪上线反馈；
- 将共性能力反馈给核心平台团队。

FDE 的本质是缩短以下反馈链：

```text
一线业务问题
→ 现场工程师
→ 可运行软件
→ 真实业务反馈
→ 修改Ontology、应用和核心产品
```

它不只是技术岗位设计，而是一种研发和交付组织形式。Palantir 将其形容为“人的反向传播”：FDE 接近问题，现场反馈持续进入核心工程团队。

---

## 九、两种范式的适用场景

### 9.1 RAG/Agent-first 更适合

- 企业知识问答；
- 文档搜索；
- 合同、会议和报告总结；
- 内容生成；
- 客服辅助；
- 研究分析；
- 低风险办公自动化；
- 数据和流程尚不稳定的快速试验。

这些场景的核心价值是更快获取和处理信息，不一定需要先建设完整 Ontology。

### 9.2 Ontology/Operational AI-first 更适合

- 供应链调度；
- 生产计划；
- 设备维护；
- 航空运营；
- 医疗资源分配；
- 金融风险处置；
- 质量管理；
- 多部门协同决策；
- 需要写回多个业务系统的流程；
- 高权限、高风险和强监管场景。

这些场景的核心价值不是生成答案，而是基于不断变化的现实状态，在复杂约束下形成和执行一个可追溯决定。

### 9.3 最合理的现实架构通常是混合式

两种方法并非互斥。成熟企业架构很可能是：

```text
非结构化文档
→ RAG / 向量检索
→ 绑定到业务对象
→ 与结构化状态、规则和模型共同进入Ontology
→ Agent形成方案
→ 通过Action受控执行
```

RAG 是 Ontology 中获取非结构化知识的一种能力，Ontology 则为 RAG 结果提供现实对象、当前状态、行动边界和反馈闭环。

---

## 十、Palantir 方法真正的优势

### 10.1 不是某一种算法，而是垂直集成

Palantir 的竞争力更可能来自以下能力的组合：

- 数据连接和转换；
- 业务语义建模；
- 对象、关系和实时状态；
- 规则、传统模型、优化器和LLM；
- 业务 Action 与写回；
- 对象和动作级安全；
- 应用开发；
- 数据与决策血缘；
- 多环境持续部署；
- FDE 现场交付。

任何单项都可以被其他技术实现，但企业要把它们组成一个长期运行、权限一致、可升级的系统，难度远高于做一个AI Demo。

### 10.2 以决策为中心，而不是以数据为中心

传统数据项目容易止于：

```text
数据接入 → 数据仓库 → 报表
```

Palantir 试图推进到：

```text
数据 → 业务对象 → 逻辑 → 决策 → 行动 → 反馈
```

这解释了为什么 Palantir 不愿把 Ontology 仅称为语义层或知识图谱。

### 10.3 将人的隐性知识转化为可复用软件

FDE 贴近一线人员，观察其判断、例外处理和权衡，并将这些经验转化为：

- 对象和关系；
- Function；
- Action；
- 应用工作流；
- 审批和权限。

这使企业知识不只存在于文档或某位员工头脑中，而成为可被人、应用和 Agent 共同调用的能力。

### 10.4 决策反馈具有复利性

当更多数据、逻辑、动作和决策记录进入同一 Ontology 后，新应用可以复用旧能力，历史决策可以用于评估新模型，Agent 也能在更丰富的上下文中工作。这是 Palantir 所强调的“compounding”逻辑。

---

## 十一、边界、风险与不能神化的地方

### 11.1 Ontology 不是自动生成的业务真相

Ontology 的质量取决于：

- 数据是否可靠；
- 对象定义是否符合现实；
- 关系是否完整；
- Function 是否正确；
- Action 是否符合真实流程；
- 权限是否正确配置；
- 业务人员是否真正使用。

错误的 Ontology 不只是返回错误答案，还可能让多个应用和 Agent 共同使用错误语义。

### 11.2 不能先建完整企业本体再找价值

如果把 Ontology 项目做成多年主数据和概念统一工程，容易陷入：

- 部门争论；
- 范围无限扩大；
- 长期没有业务成果；
- 模型在完成前已经过时。

合理路径应是围绕具体决策构建最小闭环，在使用中逐渐形成共享 Ontology。

### 11.3 平台集成优势也意味着锁定

Palantir 将数据、Ontology、应用、AI和治理紧密结合，有利于一致性，但也可能带来：

- 供应商依赖；
- 迁移成本；
- 专有技能依赖；
- 采购和许可成本；
- 客户内部平台能力弱化。

研究 Palantir 时应同时判断开放接口、数据可移植性、业务逻辑可迁移性和组织自主能力。

### 11.4 FDE 高度依赖人才密度

FDE 范式要求工程师能够同时理解业务、数据、软件和组织问题。这类人才成本高，也难以无限复制。如果客户长期依赖外部 FDE 而不能形成内部建设者，平台可能难以真正内生化。

### 11.5 安全能力不等于天然安全

统一权限、审计和 Action 边界可以降低风险，但前提是对象、策略、身份和Action均正确配置。复杂系统仍可能出现权限错误、数据泄漏、错误自动化和模型失控，因此必须保留测试、审批、监控、回滚和责任机制。

### 11.6 厂商叙事与业务证据必须分开

Palantir 的文档可以证明其架构愿景和产品能力，但业务收益必须逐案例验证。尤其需要区分：

- 已投入生产还是 PoC；
- 预期收益还是已实现收益；
- 客户整体改善还是 Palantir 单独贡献；
- 客户自有能力还是 Palantir 能力；
- 厂商演示还是可核实运营数据。

---

## 十二、企业采用时的判断框架

企业不应先问“要不要买 Palantir”，而应先问以下问题。

### 12.1 是否存在值得建设闭环的关键决策

- 决策频率是否足够高；
- 错误决策的损失是否足够大；
- 是否涉及多个数据源和部门；
- 是否存在明确执行动作；
- 结果能否被衡量和反馈。

### 12.2 问题只是知识获取，还是运营决策

如果目标主要是搜索、摘要和问答，RAG 可能已经足够。如果目标涉及资源分配、排产、风险处置和多系统写回，则需要更强的业务对象、工作流和Action模型。

### 12.3 是否具备最小 Ontology 的建设条件

- 能否识别核心对象；
- 能否确定对象主键和状态来源；
- 能否定义关键关系；
- 能否明确业务动作和权限；
- 是否有业务负责人参与；
- 是否有团队持续维护。

### 12.4 是否可以形成薄切口

好的第一个场景应：

- 价值明确；
- 数据可获得；
- 决策链条较短；
- 有真实用户；
- 可以产生可验证结果；
- 能沉淀可复用对象和动作。

### 12.5 如何衡量成功

不能只衡量回答准确率，还应衡量：

- 决策时间；
- 采纳率；
- 执行成功率；
- 人工交接次数；
- 业务结果；
- 风险事件；
- 新场景复用率；
- 平台长期维护成本。

---

## 十三、生态、开发者社群、市场推广与战略方向

### 13.1 核心判断：Palantir 正在解决 FDE 模式的规模化问题

Palantir 早期的增长高度依赖 FDE：工程师进入客户现场，识别高价值决策，连接数据，建立 Ontology，并快速做出可投入使用的应用。这种方法有利于攻克复杂场景，却受到三个天然约束：

1. 高水平 FDE 人才稀缺；
2. 每个新客户都从现场发现和实施开始，边际交付成本较高；
3. 现场形成的业务知识如果不能产品化，就难以跨客户和行业复用。

因此，Palantir 当前建设生态的核心，不只是获得更多品牌曝光或聚集一个技术论坛，而是把原来主要掌握在 Palantir FDE 手中的建设能力，逐渐交给客户内部开发者、合作伙伴、独立开发者和 AI Agent。其目标可以概括为：

> **让更多主体能够在同一套 Ontology、权限和运行基础设施上构建、封装、部署和复用业务软件，从而提高复杂企业软件的复制速度。**

这意味着 Palantir 的演进又增加了一条主线：

```text
第一阶段：Palantir FDE 为客户建设解决方案
       ↓
第二阶段：客户自己的开发者在 Foundry/AIP 上持续建设
       ↓
第三阶段：合作伙伴将行业经验封装为可复用产品和交付方法
       ↓
第四阶段：AI FDE 与编程 Agent 进一步降低建设和迁移成本
       ↓
长期目标：Ontology 成为企业应用与 Agent 的共同业务后端
```

### 13.2 Palantir 生态的五个层次

Palantir 所说的“开发者生态”不能只理解为公开注册的个人开发者。其生态至少包含五个相互连接、成熟度不同的层次。

| 层次 | 主要参与者 | 建设内容 | 对 Palantir 的战略价值 | 当前成熟度判断 |
|---|---|---|---|---|
| 客户内部 Builder 生态 | 数据工程师、软件工程师、业务分析师、运营人员、AI 团队 | 数据管道、对象模型、Function、Action、应用、Agent | 让平台从一个项目扩展到多个部门和场景 | 相对成熟，是当前生态的主体 |
| 公开开发者生态 | 个人开发者、学生、技术社群、开源贡献者 | OSDK 应用、连接器、示例、教程、社区组件 | 扩大人才池、降低学习门槛、形成外部认知 | 早中期，规模和开放度仍有限 |
| 产品与分发生态 | 客户团队、Palantir、合作伙伴、潜在 ISV | Marketplace 包、Foundry Products、可部署应用和模板 | 把一次性交付转为可重复的软件分发 | 基础设施已形成，独立开发者经济仍早期 |
| 服务与交付伙伴生态 | 咨询公司、系统集成商、云与技术伙伴 | 行业方案、实施服务、变革管理、联合销售 | 扩大交付覆盖，减少只依赖 Palantir FDE 的瓶颈 | 正在加速，战略地位明显提升 |
| 跨组织与行业网络 | 客户、供应商、承运商、医院、政府机构、行业联盟 | 共享工作流、协同对象、跨组织 Action 和行业操作系统 | 从单一客户扩展到供应链和行业网络 | 选择性落地，长期潜力大但治理难度高 |

这五层的关系并不是简单的渠道层级。客户内部 Builder 产生真实场景；Palantir 和合作伙伴从中抽象产品；Marketplace 和开发工具负责分发；培训和社群扩大能够使用这些产品的人群；跨企业网络则把同一 Ontology 模式延伸到客户边界之外。

### 13.3 开发者工具链：从“在平台里开发”走向“把 Ontology 当作后端”

Palantir 的开发者战略不是要求所有人都在 Foundry 的低代码界面中工作，而是在低代码、原生平台开发和外部专业开发之间建立连续工具链。

#### OSDK：把 Ontology 变成类型化开发接口

Ontology SDK（OSDK）允许开发者使用 TypeScript、Python 和 Java 等语言访问对象、关系、查询和 Action。其重要意义不是又提供了一套普通 API，而是让开发者以业务对象而不是底层表结构进行开发。

例如，应用面对的可以是：

```text
Aircraft
Flight
MaintenanceOrder
Supplier
Shipment
```

而不是：

```text
ODS_FLT_03
ERP_MO_HEADER
CRM_ACCOUNT_V7
```

这样，外部应用、移动端、Web 应用和 Agent 可以共享平台中已经定义的对象语义、权限和动作边界。Ontology 因而不只是 Foundry 内部的数据组织方式，也被推向一种统一的业务后端。

#### Developer Console：把开发、授权、运行和分发连接起来

Developer Console 面向专业开发者，负责应用注册、Ontology 资源选择、认证配置、网站托管、运行指标和 Marketplace 分发。它承担的角色类似一个围绕 Ontology 的应用控制面：开发者可以选择允许应用读取的对象和可以调用的 Action，而不是直接获得对整个数据平台的无限访问。

随着 MCP、OSDK 和面向编程 Agent 的能力加入，Ontology 中的查询、Function 和 Action 还可以被暴露为具备权限范围的 AI 工具。其战略含义是：未来第三方 AI 应用不一定要迁入 Foundry 界面，但可以将 Foundry/Ontology 作为经过治理的企业业务能力层。

#### 低代码、专业代码与 Agent 开发并行

Palantir 的开发者并非单一角色：

- 业务用户可以通过 Workshop、Pipeline Builder 等方式建立应用和流程；
- 平台开发者可以建立 Ontology、Function、Action 和数据管道；
- 专业软件工程师可以通过 OSDK、Developer Console 和代码仓库开发外部应用；
- AI Agent 可以在被授权的范围内生成代码、配置对象、调用 OSDK 或 MCP 工具；
- 合作伙伴可以将上述能力封装为行业产品和交付模板。

这个结构扩大了“开发者”的定义。Palantir 真正希望形成的，不只是程序员社群，而是围绕企业运营模型持续建设的 Builder 网络。

### 13.4 Developer Tier、学习体系与公开社群

Palantir 过去主要通过客户项目培养开发者。近几年，它开始补齐公开开发者入口，但这一体系仍处于形成阶段。

#### Developer Tier

Developer Tier 为符合条件、位于部分国家和地区的个人开发者提供有限的 Foundry/AIP 访问，使其可以在没有企业合同的情况下学习和试验平台。这是 Palantir 从纯企业销售模式向开发者自助体验迈出的关键一步。

但它与成熟公有云的免费层仍有明显差异：可用地区、资源、产品能力和使用场景受到限制，尚未形成完全开放、全球可自助进入的开发者漏斗。因此，Developer Tier 更适合作为人才培养和产品体验入口，而不能直接证明 Palantir 已经拥有大规模公开开发者经济。

#### Palantir Learning、Speedrun 与认证

Palantir 通过 Learning 平台、Speedrun、教程、训练营和认证，把原来由 FDE 在项目现场传递的知识拆分为标准化学习路径。官方资料显示，Palantir Learning 已形成覆盖 Foundry 与 AIP 的课程体系，并在 2025 年继续推出面向 Foundry/AIP 基础认知的免费认证。

这套学习体系承担三个功能：

1. 降低客户团队上手成本；
2. 形成可被雇主识别的人才标签；
3. 为合作伙伴扩大实施人员供给。

从商业角度看，认证和培训不只是教育产品，而是解决“谁来实施和维护 Ontology”这一规模化问题的基础设施。

#### 官方 Developer Community 与 GitHub

Palantir 在 2024 年 6 月推出官方 Developer Community，供开发者讨论 Foundry、AIP、Ontology、API 和开发问题。与此同时，`palantir/aip-community-registry` 等 GitHub 仓库为社区项目、示例和组件提供公开入口。

需要区分两种信号：

- **官方社群**说明 Palantir 正在主动经营开发者关系；
- **社区仓库**可以提高分享和发现效率，但其中项目可能是社区驱动、未经 Palantir 正式支持的组件，不能视为平台级 SLA 产品。

截至 2026 年，这个公开社群的战略意义大于其当前规模。它表明 Palantir 正在从封闭的客户项目知识网络，向可搜索、可分享、可持续参与的开发者知识网络转变；但其内容体量、第三方维护者数量和独立商业项目数量，仍不能与主流开源框架或大型云平台相比。

#### Fellowship、Buildcamp 与线下 Meetup

Palantir 还通过 Open Source Fellowship、Buildcamp、DevCon 竞赛和合作伙伴 Meetup，把线上学习者转化为能够公开展示作品、参与线下活动并相互认识的贡献者。2025 年 DevCon Open Source Fellowship 要求参与者使用 Enterprise 或 Developer Tier 建设项目，公开 GitHub 仓库或向 Community Registry 提交贡献，并通过视频演示成果；社区活动页也显示了 Buildcamp 和 PwC Palantir Developer Meetup 等活动。

这些项目的意义在于建立“学习—构建—公开展示—获得认可—进入客户或伙伴项目”的参与路径。不过，Fellowship 和竞赛更接近精选式社群运营，不能据此把 Palantir 定义为一家以大规模开源贡献为核心的公司。其核心平台仍是商业专有软件，开源活动主要用于扩大外围组件、人才网络和开发者参与。

### 13.5 Marketplace 与产品化：把一次性交付变成可重复分发

如果说 Developer Tier 和社群解决“谁能够学习和开发”，Marketplace 解决的就是“开发成果如何被封装和复用”。

Palantir Marketplace 可以分发数据连接器、应用、对象模型、代码、模板和其他平台资源。Foundry Products 则进一步尝试把一组跨项目资源封装成具有版本、依赖和安装边界的产品。它们共同指向一个关键转变：

```text
过去：FDE 在客户环境中完成一个解决方案

现在：把对象模型、Function、Action、应用和部署配置
      封装为可以在其他环境安装、升级和维护的产品
```

对 Palantir 而言，真正有价值的可复用单元，不只是一个界面或提示词，而可能是一整套纵向能力：

```text
行业数据映射
  + Ontology 对象和关系
  + 业务 Function
  + 可执行 Action
  + 权限策略
  + 应用与 Agent
  + 部署和运维配置
```

这种封装方式比普通应用商店更重，却更接近复杂企业软件的现实：安装一个“供应链控制塔”并不等于安装一个前端页面，而是要把客户数据映射到对象、校准规则、连接执行系统并配置权限。

目前仍需谨慎看待 Marketplace 的成熟度。它不是一个已经证明具有强大独立开发者收入分成和公开长尾供给的消费级 App Store，也尚未显示出 Salesforce AppExchange 或公有云 Marketplace 那样成熟的第三方商业结构。许多产品仍由 Palantir、客户内部团队或经过选择的合作伙伴在受控环境中分发，Foundry Products 也仍包含测试或逐步开放的能力。

### 13.6 Palantir 怎样做市场推广：从客户证明到现场转化

Palantir 的推广方式与传统软件“投放广告—在线试用—信用卡购买”不同。由于产品面向高复杂度、高安全要求和高组织摩擦的场景，它采用的是一条以真实业务问题为核心的转化路径。

| 阶段 | 主要机制 | 目的 | 典型产物 |
|---|---|---|---|
| 建立认知 | AIPCon、客户演示、行业案例、管理层内容 | 证明 AIP 不只是聊天机器人，可以进入运营流程 | 客户证言、现场演示、行业故事 |
| 形成技术兴趣 | DevCon、文档、Developer Community、GitHub、学习课程 | 让开发者理解 Ontology、OSDK 和 AIP 的建设方式 | 教程、示例、开发者关系 |
| 低门槛体验 | Developer Tier、Speedrun、Buildcamp | 缩短首次接触到首次构建的时间 | 个人原型、技能认证 |
| 验证业务价值 | AIP Bootcamp | 使用客户真实数据，在数天内构建第一个工作流或用例 | 可操作原型、初步 ROI 假设 |
| 进入生产 | FDE、平台团队、实施伙伴 | 处理数据治理、权限、工作流、变革管理和生产运维 | 生产应用、Ontology、Action |
| 横向扩展 | 客户内部 Builder、复用组件、培训、Marketplace | 从一个用例扩展到多个部门和业务线 | 共享对象、函数、动作和应用 |
| 跨组织扩展 | 行业伙伴、供应链网络、联合产品 | 将协同延伸到供应商、客户和行业伙伴 | 多方工作流、行业操作系统 |

#### AIPCon：用客户成果进行买方教育

AIPCon 的主要受众是企业决策者、潜在客户和合作伙伴。活动重点通常不是发布单一技术参数，而是由客户展示 AIP 如何进入供应链、制造、医疗、保险、国防和其他真实运营场景。其推广逻辑是：复杂企业软件很难通过功能清单销售，必须让相似组织看到“同类问题已经被解决”。

Palantir 在 2026 年第二季度业务更新中已将活动编号推进到 AIPCon 10。这种连续编号说明它正在把客户大会经营为高频、可重复的销售与市场机制，而不是偶发发布会。

#### DevCon：把产品叙事转化为 Builder 叙事

DevCon 更面向实际建设者，展示 OSDK、Developer Console、Ontology、AIP Agent、MCP、代码开发和平台新能力。AIPCon 回答“业务为什么要采用”，DevCon 回答“开发者怎样建设”。Palantir 在 2026 年第一季度业务更新中已披露 DevCon 5，说明开发者活动也在形成持续节奏。

#### AIP Bootcamp：推广、销售和交付的交叉点

Bootcamp 是 Palantir 当前最具辨识度的增长机制之一。客户不是使用一套完全虚构的演示数据，而是在较短时间内围绕真实数据和真实业务问题构建工作流。它同时承担四个功能：

1. 让客户亲自体验产品，而不是只观看销售演示；
2. 发现数据、权限和流程中的真实障碍；
3. 快速形成第一个可评估用例；
4. 识别客户内部可以继续建设的 Champion 和 Builder。

因此，Bootcamp 既是产品体验，也是售前 PoC、需求发现、客户培训和首个实施切口。它降低了复杂平台价值难以在采购前证明的问题，但由数天原型进入生产仍需补足安全、数据质量、集成、稳定性和组织变革，不能把 Bootcamp 原型等同于生产价值。

#### 客户案例驱动的“样板间”策略

Palantir 大量使用客户管理者和一线人员亲自讲述的方式推广。这种做法有三个效果：

- 将抽象的 Ontology/AIP 概念翻译为行业决策；
- 降低其他企业对新技术和供应商风险的感知；
- 为销售团队提供可以按行业复用的证明材料。

这里也存在证据边界：大会演示和客户证言可以证明应用存在、客户愿意公开背书，但不一定充分证明全面 ROI、长期维护成本和 Palantir 的独立贡献。研究时仍要把市场材料与可审计经营指标分开。

### 13.7 合作伙伴战略：扩展交付能力和行业覆盖

Palantir 的伙伴生态大致可分为四类。

#### 云、模型和技术伙伴

Palantir 需要在主流云、私有云、边缘和隔离环境中运行，也允许企业接入不同模型和现有数据基础设施。云与技术伙伴的作用，是降低客户把 Palantir 纳入现有架构的摩擦，并减少“AIP 只能绑定某一个模型或单一云环境”的担忧。

#### 咨询和系统集成伙伴

Accenture、PwC、Fujitsu、Proxet 等伙伴承担联合销售、行业设计、数据迁移、实施和组织变革。2025 年，Palantir 与 Accenture 扩大全球战略合作并成立 Accenture Palantir Business Group；同年又与 PwC UK 宣布多年期首选交付合作。这类合作透露出明确方向：Palantir 不希望所有项目都只能由自己的 FDE 亲自交付，而要让大型咨询与实施组织成为规模化渠道。

但伙伴交付也会带来新的挑战：Ontology 建设依赖对业务的深入理解，如果伙伴只复制传统系统集成的按需求实施方式，可能削弱 FDE 模式中“快速发现—共同构建—反馈产品”的优势。Palantir 必须在扩大交付供给和保持实施质量之间建立认证、产品边界和方法标准。

#### 行业伙伴和联合操作系统

Palantir 正在航空、太空、造船、保险、医疗、电信、汽车、安全风险和政府等领域与行业龙头形成联合方案。这类合作不是单纯经销，而是借助行业伙伴的流程知识、客户网络和数据标准，把 Palantir 的通用平台转化为行业操作系统。

其潜在飞轮是：

```text
头部客户真实场景
    ↓
提炼行业对象、动作和应用模式
    ↓
与行业伙伴形成可复制方案
    ↓
降低下一家客户的实施时间
    ↓
获得更多场景和反馈
```

#### FedStart 与受监管市场伙伴

FedStart 允许软件和国防科技企业利用 Palantir 已建设的政府合规基础设施，把自身 SaaS 或 AI 产品更快部署到受监管的政府环境。这里 Palantir 的角色不再只是应用平台供应商，也像是“合规进入市场”的基础设施伙伴。

这类模式很重要，因为它创造了一种不同于席位授权的生态价值：第三方产品的增长会增加对 Palantir 部署、合规和运行底座的需求。

### 13.8 六个值得持续观察的战略方向

#### 方向一：从项目交付转向产品化复用

Palantir 会继续保留 FDE，因为复杂问题仍需要现场发现；但它会努力把 FDE 成果沉淀为 Ontology 模板、Function、Action、连接器、应用和行业产品。未来竞争力取决于“每个新项目创造多少可复用资产”，而不只是签下多少工程服务。

#### 方向二：从 Palantir 开发者转向客户和伙伴开发者

平台必须让客户内部团队能够独立扩展，否则客户越多，Palantir 的交付压力越大。学习体系、认证、Developer Console、OSDK 和伙伴计划都服务于这项转移。

#### 方向三：Ontology 成为应用和 Agent 的共同业务后端

传统企业中，Web 应用、BI、自动化和 AI Agent 往往各自连接数据与权限。Palantir 希望这些消费者共同使用 Ontology 的对象、关系、Function、Action 和安全策略。OSDK 和 MCP 是把这一后端能力送出 Foundry 界面的关键接口。

#### 方向四：AI FDE 缓解人类 FDE 稀缺

AI FDE、Pilot、MINDKIT、Ontology Foundations 以及面向编程 Agent 的工具，指向一个共同目标：让 AI 协助完成数据理解、对象建模、应用搭建、迁移、测试和文档工作。它不能完全替代业务判断和责任承担，但可能显著提高每名 FDE、客户开发者和伙伴工程师的产出。

#### 方向五：从单一企业扩展到供应链和行业网络

当客户内部 Ontology 稳定后，下一步自然是把供应商、承运商、客户、政府或行业伙伴纳入共同工作流。这个方向可能产生更强黏性和网络效应，但也会放大数据主权、竞争边界、跨组织权限和责任分配问题。

#### 方向六：强调模型中立和互操作

Palantir 的长期利益不在于押注某一个基础模型，而在于让任何被客户选择的模型，都通过 Ontology 使用企业上下文和 Action。BYOM、OSDK、API 和 MCP 有助于强化这种“上层模型可替换、业务运行层稳定”的定位。

### 13.9 生态飞轮与网络效应的准确理解

Palantir 可能形成的生态飞轮如下：

```text
AIPCon/客户案例建立认知
          ↓
Developer Tier、课程和 DevCon 培养 Builder
          ↓
Bootcamp 与 FDE 找到真实高价值场景
          ↓
客户建立 Ontology、Action、应用和 Agent
          ↓
成功模式被封装为模板、产品和伙伴方法
          ↓
下一家客户更快获得首个价值
          ↓
更多客户、开发者和行业反馈进入平台
```

这里的网络效应主要不是把不同客户的数据集中到一起训练模型。对于敏感企业和政府客户，这样做通常不现实，也不应被默认假设。更准确地说，网络效应可能来自：

1. **产品复用**：连接器、对象模式、Function、Action 和应用能够重复部署；
2. **方法复用**：FDE 和伙伴积累某类业务问题的实施经验；
3. **人才复用**：受过训练和认证的 Builder 可以在更多组织中工作；
4. **生态互补**：模型、云、咨询公司和行业伙伴围绕同一平台形成互补供给；
5. **跨组织协同**：在获得授权的情况下，供应链和行业参与者共享受控工作流。

因此，Palantir 的潜在网络效应更接近“软件资产、业务模式和人才密度的累积”，而不是“客户数据池越大，模型自然越强”。

### 13.10 当前成熟度：基础设施已成形，公开开发者经济仍在早中期

综合来看，Palantir 已经具备生态平台的主要基础部件：

- Developer Tier；
- OSDK 和 Developer Console；
- Learning、Speedrun、认证和开发者活动；
- 官方 Developer Community 与 GitHub 入口；
- Marketplace 与 Foundry Products；
- 大型咨询、云、行业和合规伙伴；
- AIPCon、DevCon、Bootcamp 与 FDE 组成的推广和转化链路；
- AI FDE 和编程 Agent 所代表的自动化建设能力。

但仍不能把它直接等同于 AWS、Salesforce、ServiceNow、Databricks 或 iOS 的成熟生态。主要原因包括：

1. Developer Tier 尚非全球、完全开放的自助服务；
2. 官方公开社群建立时间较短；
3. Marketplace 中大量分发仍发生在客户或伙伴的受控关系内；
4. Foundry Products 等产品化机制仍在演进；
5. 独立 ISV 的定价、收入分成、获客和退出机制尚不够清晰；
6. 平台学习曲线、采购模式和企业数据接入门槛较高；
7. Ontology 和 Action 越深入核心运营，迁移成本和平台锁定越强。

最准确的阶段判断是：

> **Palantir 已从“公司自身交付复杂软件”进入“建设受控的平台生态”阶段，但尚未进入大规模、开放、长尾、自运转的开发者经济阶段。**

### 13.11 后续判断生态战略是否成功的指标

Palantir 官方经常披露客户、合同、收入和活动案例，但如果要专门判断生态是否形成，应持续跟踪更细的指标：

- Developer Tier 可用国家和注册门槛是否扩大；
- 月活跃开发者、社区提问和非 Palantir 维护者数量；
- OSDK 应用、MCP 工具和社区组件的新增与维护情况；
- Marketplace 中第三方产品数量、安装量、升级率和付费规模；
- 伙伴主导而非 Palantir FDE 主导的生产项目占比；
- 认证开发者和合作伙伴工程师数量；
- 客户从 Bootcamp 到生产的转化率和所需时间；
- 同一客户内部复用对象、Function、Action 和应用的比例；
- 行业产品相对定制项目的收入或交付占比；
- AI FDE 是否真正缩短 Ontology 建设和场景上线时间；
- 第三方开发者能否形成可持续收入，而不只是参与培训和演示。

这些指标比活动场次或 GitHub Star 更能回答一个关键问题：Palantir 是否真的把 FDE 的高密度能力转化为了一个能够由客户、伙伴和开发者共同扩张的平台。

---

## 十四、研究结论

### 结论一：Palantir 二十多年的核心问题没有改变

从 Gotham 到 Foundry、Ontology 和 AIP，Palantir 始终试图把分散数据转化为一个能够支持人在复杂现实中判断和行动的软件世界。

### 结论二：Ontology 是 Palantir 产品演进的逻辑中心

它将数据从技术结构转化为业务对象，又将规则、模型、动作和权限绑定到这些对象。它不是简单语义层，而是企业决策运行时。

### 结论三：AIP 不是对 Foundry 的替代，而是对 Ontology 的激活

AIP 让 LLM 和 Agent 可以使用此前为人、应用和传统模型建设的业务世界。AI 获得的不只是文档上下文，还有类型化对象、确定性逻辑、受控Action和权限边界。

### 结论四：普通 RAG/Agent 与 Palantir 的根本区别不是向量库

真正的区别是：

> 普通 AI 团队通常给模型建设上下文和工具；Palantir FDE 试图给企业建设一个人和 AI 共同使用的可执行运营模型。

### 结论五：FDE 是产品体系的一部分

Ontology 不可能只靠软件自动产生。FDE 通过贴近现场，把业务人员的对象、判断、约束和动作转化为软件，并将现场反馈传回平台研发。Palantir 的技术与组织方法必须结合研究。

### 结论六：Palantir 方法可借鉴，但不应被神化

开放技术栈也能组合数据平台、知识图谱、语义层、策略引擎、工作流、IAM、Agent框架和可观测系统，建设相近能力。Palantir 的优势主要是集成度、产品成熟度和FDE交付机制；代价是投入、复杂度、人才要求和平台锁定。

### 结论七：生态战略的本质是把 FDE 能力产品化和社会化

Developer Tier、Learning、OSDK、Developer Console、Marketplace、DevCon、Bootcamp、咨询伙伴和 AI FDE 并不是彼此独立的推广动作。它们共同服务于同一目标：把 Palantir 员工在客户现场建设 Ontology 和运营应用的能力，转化为客户、合作伙伴、开发者与 Agent 都能使用的工具、产品和方法。这个生态的基础设施已经形成，但公开开发者规模、第三方产品经济和独立 ISV 商业闭环仍需时间验证。

### 最终概括

```text
Foundry：构建和运行企业数字世界的平台
Ontology：这个世界的对象、关系、逻辑、动作和安全模型
AIP：让AI在这个世界中受控理解、推理和行动
Apollo：让整套软件在复杂环境中持续交付和运行
FDE：把现实业务问题不断翻译成上述软件系统的人与组织机制
生态：让客户、伙伴、开发者和AI共同复用并扩大上述建设能力
```

Palantir 最值得研究的，不是它是否拥有一个更强的大模型，而是它试图回答了企业 AI 落地中更困难的问题：

> **AI 获得答案之后，怎样进入真实业务；采取行动之前，怎样理解对象、约束和权限；行动完成之后，怎样留下可审计、可学习的反馈。**

---

## 十五、后续可继续研究的问题

1. Ontology 是怎样从底层数据源映射出来的，数据虚拟化、物化和写回分别如何实现？
2. Action 在跨多个外部系统时，失败、重试、补偿和一致性怎样处理？
3. Ontology 的版本、分支、发布和跨部门治理如何运作？
4. AIP Agent 怎样把 Ontology Function 和 Action 转化为工具，其规划与执行架构是什么？
5. Palantir 的决策血缘与传统数据血缘、Agent Trace 有哪些具体字段和技术差异？
6. FDE 在一个新客户中的实际交付流程、人员结构、时间投入和商业模式是什么？
7. 如何用开源组件搭建一套“轻量级 Ontology+Agent”参考架构，并与 Palantir 比较总拥有成本？
8. 在已经拥有成熟数据湖仓、主数据、语义层和工作流平台的企业中，Palantir 的增量价值在哪里？
9. Ontology 怎样避免成为中央化、僵化的企业数据模型？
10. 不同行业案例中，哪些价值来自 Foundry/AIP，哪些来自客户自身流程改造和模型能力？
11. Developer Tier 的地域、资源和商业限制将如何变化，Palantir 是否会形成真正全球自助式开发者入口？
12. Marketplace 和 Foundry Products 能否建立独立 ISV 的定价、分成、获客和持续升级机制？
13. 伙伴主导项目与 Palantir FDE 主导项目相比，在上线速度、质量和客户扩展率方面表现如何？
14. AI FDE 和编程 Agent 能否实质降低 Ontology 建模门槛，还是只把瓶颈转移到业务定义、验证和治理？

---

## 十六、主要参考资料

### 监管和年报

1. [Palantir 2020 年 S-1/A 上市文件（SEC）](https://www.sec.gov/Archives/edgar/data/1321655/000119312520239121/d904406ds1a.htm)
2. [Palantir 2025 财年 10-K](https://investors.palantir.com/files/2025%20FY%20PLTR%2010-K.pdf)

### Palantir 官方架构与产品文档

3. [Palantir Architecture Center Overview](https://www.palantir.com/docs/foundry/architecture-center/overview)
4. [The Ontology System](https://www.palantir.com/docs/foundry/architecture-center/ontology-system)
5. [Why Create an Ontology](https://www.palantir.com/docs/foundry/ontology/why-ontology)
6. [AIP Architecture Overview](https://www.palantir.com/docs/foundry/architecture-center/aip-architecture)
7. [AIP, Foundry and Apollo](https://www.palantir.com/docs/foundry/architecture-center/platforms)
8. [Ontology Types Reference](https://www.palantir.com/docs/foundry/object-link-types/type-reference)
9. [Link Types Overview](https://www.palantir.com/docs/foundry/object-link-types/link-types-overview)
10. [Functions Overview](https://www.palantir.com/docs/foundry/functions/overview)
11. [Function-backed Actions](https://www.palantir.com/docs/foundry/action-types/function-actions-overview)
12. [Action Side Effects](https://www.palantir.com/docs/foundry/action-types/side-effects-overview)
13. [Action Log](https://www.palantir.com/docs/foundry/action-types/action-log)
14. [Object Permissioning](https://www.palantir.com/docs/foundry/object-permissioning/managing-object-security)
15. [Ontology-aware Applications](https://www.palantir.com/docs/foundry/ontology/applications)
16. [AIP Evals](https://www.palantir.com/docs/foundry/aip-evals/overview)
17. [AI FDE Overview](https://www.palantir.com/docs/foundry/ai-fde/overview)

### LangChain/LangGraph 官方文档

18. [LangChain Retrieval](https://docs.langchain.com/oss/python/langchain/retrieval)
19. [LangChain Agents](https://docs.langchain.com/oss/python/langchain/agents)
20. [LangChain Tool Integrations](https://docs.langchain.com/oss/python/integrations/tools/index)
21. [LangGraph Overview](https://langchain-ai.github.io/langgraph/index.html)
22. [LangGraph Persistence](https://docs.langchain.com/oss/python/langgraph/persistence)
23. [LangChain Human-in-the-loop](https://docs.langchain.com/oss/python/langchain/human-in-the-loop)
24. [LangSmith Observability Concepts](https://docs.langchain.com/langsmith/observability-concepts)
25. [LangSmith Evaluation](https://docs.langchain.com/langsmith/evaluation)

### 生态、开发者与市场战略资料

26. [Palantir Developers Overview](https://www.palantir.com/docs/foundry/developers)
27. [Getting Started：Developer Tier、Bootcamp 与 Learning](https://www.palantir.com/docs/foundry/getting-started/overview)
28. [Palantir 2024 年 6 月产品公告：Developer Community 上线](https://www.palantir.com/docs/foundry/announcements/2024-06)
29. [Palantir Developer Community](https://community.palantir.com/)
30. [AIP Community Registry（GitHub）](https://github.com/palantir/aip-community-registry)
31. [Developer Console Overview](https://www.palantir.com/docs/foundry/developer-console/overview)
32. [Marketplace Overview](https://www.palantir.com/docs/foundry/marketplace/overview)
33. [Foundry Products](https://www.palantir.com/docs/foundry/marketplace/foundry-products)
34. [Developer Console：Marketplace Installation](https://www.palantir.com/docs/foundry/developer-console/marketplace-installation)
35. [Palantir 2025 年 11 月公告：Learning 与认证](https://www.palantir.com/docs/foundry/announcements/2025-11)
36. [Palantir 产品公告索引：开发工具、Agent 与 MCP 更新](https://www.palantir.com/docs/docs/foundry/announcements/index.html)
37. [Palantir 2026 年第一季度业务更新：DevCon 5](https://investors.palantir.com/files/Palantir%20-%20Q1%202026%20Business%20Update.pdf)
38. [Palantir 2026 年第二季度业务更新：AIPCon 10](https://investors.palantir.com/files/Palantir%20-%20Q2%202026%20Business%20Update.pdf)
39. [Accenture 与 Palantir 扩大全球战略合作](https://newsroom.accenture.com/news/2025/accenture-and-palantir-expand-global-strategic-partnership-to-drive-ai-reinvention)
40. [Palantir 与 PwC UK 宣布多年期首选交付合作](https://investors.palantir.com/news-details/2025/Palantir-and-PwC-UK-Sign-a-Multi-Year-Multi-Million-Pound-Deal-to-Accelerate-AI-Transformation-as-Preferred-Partners-in-the-UK/)
41. [Palantir FedStart 与政府合规基础设施](https://investors.palantir.com/news-details/2025/Palantir-Technologies-Achieves-Cybersecurity-Maturity-Model-Certification-CMMC-Level-2/)
42. [Ontology SDK Overview](https://www.palantir.com/docs/foundry/ontology-sdk/overview)
43. [DevCon Open Source Fellowship](https://community.palantir.com/t/the-open-source-fellowship-earn-your-place-at-devcon2/2756)
44. [Palantir Developer Community：Events、Buildcamp 与 Meetup](https://community.palantir.com/tag/events/70)

---

## 附录：一页式框架

### Palantir 的发展主线

```text
2003 反恐软件起点
  ↓
2008 Gotham：多源数据、实体关系、情报到行动
  ↓
商业行业实践：发现大型企业存在同构问题
  ↓
2016 Foundry：数据和运营能力平台化
  ↓
Ontology：对象、关系、逻辑、动作、安全成为共同核心
  ↓
Apollo：复杂环境持续部署
  ↓
2023 AIP：LLM和Agent进入Ontology业务世界
  ↓
2024—2026：Developer Tier、OSDK、Developer Console、Marketplace、
Developer Community、DevCon、伙伴计划与AI FDE逐步成形
  ↓
当前：人、应用、传统模型与Agent共同运行，
并由Palantir、客户和伙伴共同建设的企业操作系统愿景
```

### 两种企业 AI 方法

```text
RAG/Agent-first
AI应用 → 知识/数据/工具 → 答案或任务

Ontology/Operational AI-first
业务决策 → 对象/逻辑/动作/安全 → 人与AI协同 → 执行和反馈
```

### 最关键的辨别问题

```text
这个项目最终要交付的是：

A. 一个更会回答问题的AI助手？

还是

B. 一个能够基于企业当前状态、在权限约束下参与实际决策和行动的业务系统？
```

如果主要是 A，RAG/Agent-first 往往更经济；如果主要是 B，就需要逐步引入 Ontology、确定性逻辑、Action、权限和决策血缘等 Operational AI 能力。
