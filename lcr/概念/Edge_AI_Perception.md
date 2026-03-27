---
aliases: [Edge AI Perception, 边缘 AI 感知, WiFi Sensing]
tags: [concept, ai, edge-computing, iot]
---

# 边缘 AI 感知 (Edge AI Perception)

## 定义
边缘 AI 感知是指在靠近数据源的物理设备（边缘端）而非云端数据中心运行人工智能算法，通过分析环境中的物理信号（如 WiFi 信道状态信息 CSI、音频、雷达波）来实现对环境和人的感知（如人体姿态估计、生命体征监测等）。

## 核心价值与应用
*   **隐私保护**: 无需依赖传统的光学摄像头（Camera-less），避免了图像泄露和隐私争议。
*   **低延迟与高可靠性**: 数据在本地处理，无需等待云端往返，在网络断开时依然可以工作。
*   **非视距感知 (NLOS)**: 通过射频（RF）信号（如 WiFi、毫米波）的穿透性，可实现隔墙检测。

## 典型技术 / 框架
*   WiFi DensePose (基于无线电信号重建人体 3D 姿态)
*   RuVector / RuView (利用消费级 WiFi 硬件如 ESP32 进行微小扰动分析)
