# mjlab_MGDP：LgDog1 深度感知四足越障训练

> 类型：算法复现 / 四足机器人 / 深度感知 / mjlab

## 项目简介

这是基于 [mjlab](https://github.com/mujocolab/mjlab) 的四足机器人 LgDog1 训练工程，对应仓库 [mjlab_MGDP](https://github.com/heartpain-kong/mjlab_MGDP)。工程面向 2027 年足式机器人挑战赛的障碍赛道，覆盖沟壑台阶、高墙、限高笼、轮胎阵、梅花桩、单木桥、绕桩接砖木坑、平地、起伏和粗糙共 10 类课目，并附带逐课目录像回放、通过率诊断和 Web 训练工作台。

## 与官方 MGDP 的差异

仓库沿用官方 MGDP 的深度感知骨架，主要改动在感知训练方式：先离线预训练 CNN + GRU 感知网络，再冻结感知权重训练 PPO；官方做法是在运动训练中继续在线更新感知。深度输入当前为 32×24（历史版本 16×16），经 stride 卷积编码器接入策略；课程在地形难度之外叠加速度分组、指令带宽和相对进展，并针对墙前等待、越墙、摆腿清障、轮胎内孔、边缘落脚等场景单独设计奖励与惩罚。逐项对比见仓库内 `doc/MGDP_COMPARISON.md`。

## 训练与部署

`release/` 附带一份可直接部署的策略，对应 `policy-v1` 标签的代码版本；`main` 主干已改为 32×24 深度并调整地形与奖励，两者不兼容。安装用 `bash scripts/deploy.sh`，训练前需先跑一次 `python -m scripts.pretrain_mgdp`，工作台入口为 `python tools/lgdog.py`（:8765 工作台，:8770 总览，:8766 曲线终端）。2027 比赛场地由 27rc-dog-race 转换接入训练与 sim2sim，转换脚本为 `scripts/import_race_course.py`。

## 相关链接

- [项目仓库](https://github.com/heartpain-kong/mjlab_MGDP)
- [mjlab 框架](https://github.com/mujocolab/mjlab)
- [2027 四足赛道场地模型](https://github.com/uwvwko-zzz/27rc-dog-race)
