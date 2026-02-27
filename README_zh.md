<div align="center">

# 🐦 Boids: 鸟群行为模拟引擎

<p align="center">
  <b>简体中文</b> | <a href="README.md">English</a>
</p>

> **项目展示专用：** 为遵守 EPFL CS-214 课程严格的学术诚信与版权协议，本项目的源代码不可公开或私下分享。但我非常乐意在面试的屏幕共享环节，为您实时展示并深度讲解核心逻辑与技术细节。如有交流意向，请联系：**<steven.ji@epfl.com>**。

![Scala](https://img.shields.io/badge/Scala-3-red.svg?style=flat-square)
![Algorithms](https://img.shields.io/badge/Algorithm-Flocking-brightgreen.svg?style=flat-square)
![EPFL](https://img.shields.io/badge/School-EPFL-red.svg?style=flat-square)

*一个轻量级、基于函数式编程原理构建的群集行为模拟引擎，生动展示了人工生命系统中的涌现现象。*

</div>

## 📖 概览

**Boids** 是一款通过简单、局部的规则生成复杂且逼真群集行为的模拟引擎。该项目受 Craig Reynolds 1986 年经典的人工生命程序启发，可以在计算机中实时模拟鸟群、鱼群和昆虫生物群落的群体动态。

引擎没有为任何个体编写固定路径，而是基于每个“个体 (boid)”与其相邻个体的交互，实时动态计算受力情况，最终在宏观上涌现出有机的群体运动轨迹。

<div align="center">
  <!-- 占位符：GUI 截图 -->
  <!-- <img src="assets/boids-gui.png" alt="Boids GUI" width="800"/> -->
  <br/>
  <em>Boids 用户图形界面，展示了复杂的涌现群集行为。</em>
</div>

### 🎯 项目范围与实现细节 (Project Scope & Implementation Details)

本项目基于 EPFL CS-214 课程框架开发。为了准确说明工作内容，特此声明：

- **由课程框架提供**：基础的抽象结构特质 (Traits)、图形用户界面 (GUI) 、不依赖 Scala 标准库的不可变集合底层实现 (`BoidSequence` 等) 以及基础的高阶函数组件。
- **学生独立实现 (我们的工作)**：包含物理计算引擎、涌现动态算法、实体定义以及相关的测试代码。代码集中在 `Boid.scala`, `BoidLogic.scala` 与 `BoidTest.scala` 等文件中，共计约 700 行 Scala 3 代码。

---

## 🧠 核心技术原理解析

### 1. 基础集群规则 (Flocking Rules)

引擎通过计算代表不同本能的聚合力来实现栩栩如生的集群行为。在每一个时间帧 (tick) 中，所有的个体都会在精确的**感知半径 (Perception Radius)** 内评估其邻居，并应用三大基本原则：

- **凝聚力 (Cohesion)**：个体会被吸引向附近同伴组成的局部质心，促使它们聚集在一起。
- **对齐力 (Alignment)**：个体会调整自身的转向，使其速度和方向与邻居的平均状态相匹配，保持队伍的有序行进。
- **分离力 (Avoidance)**：一旦邻居突破了危险的内部阈值（*排斥半径*），就会产生一股相反的力将个体推开，以防止碰撞发生。

### 2. 高阶涌现动态与定制化机制

在基础规则之外，该模拟还引入了复杂的捕食者-猎物动态和特殊的环境受力模型：

- **捕食者与猎物机制**：特定类型的个体被标记为捕食者（如：红鸟捕食蓝鸟）。猎物在检测到捕食者时会主动逃窜 (`avoidPredatorForce`)，而捕食者则主动追击 (`chasePreyForce`)，从而形成动态的追逐猎杀场面。彻底的捕食还会触发列表层面的动态实体过滤（个体消亡）。
- **人群拥挤惩罚 (Crowd Penalties)**：一项独特的物理惩罚机制。当局部群体密度超过预设阈值 `CrowdPenaltyConfig.threshold` 时，引擎会施加额外的减速向量 (`crowdAccelPenalty`) 和最低速度限制，这使得庞大、密集的鸟群变得迟缓，并在面对捕食者时更显脆弱。

---

## 👨‍💻 个人贡献 (Personal Contributions)

我的核心工作集中在架构物理引擎和实体逻辑，具体实现在 `Boid.scala` 和 `BoidLogic.scala` 模块中：

1. **数学层面的群集算法实现**：利用函数式编程，编写了凝聚力 (Cohesion)、对齐力 (Alignment) 和分离力 (Avoidance) 算法，确保物理状态更新过程无副作用。
2. **复杂的多物种动态交互**：设计并实现了高级的捕食者-猎物机制。包括猎物发现敌人时主动逃避 (`avoidPredatorForce`)、捕食者主动追击 (`chasePreyForce`)，以及动态的实体消亡判定逻辑 (`deleteBoid`)。
3. **人群拥挤惩罚系统**：独创了一种空间密度算法 (`crowdPenalty`)，通过统计局部种群数量，动态施加物理减速和特定的最低速度限制，在结构上限制了过于密集的鸟群。

---

## ⚖️ 项目署名 (Attribution)

本项目作为 EPFL CS-214 课程的一部分开发。闭源模块中的实现逻辑与架构设计均为作者的原创成果。

---

> *本项目由 **Steven Ji**，**Elsa Sánchez Fernández** 和 **Nicolas Raymond Karmolinski**，于 2025 年春季学期历时 3 周完成，属于洛桑联邦理工学院 (EPFL) 的 [软件构建 (Software Construction, CS-214)](https://edu.epfl.ch/coursebook/en/software-construction-CS-214) 课程项目（8 学分）。*
