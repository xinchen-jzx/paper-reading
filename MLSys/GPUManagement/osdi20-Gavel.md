---
title: osdi20-Gavel
date: 2024-12-27
tags: GPU Cluster Management
---

> Heterogeneity-Aware Cluster Scheduling Policies for Deep Learning Workloads

## 1. 摘要

专门的加速器如GPU、TPUs、FPGA和定制加速器，已经被越来越多地用于训练深度学习模型。这些加速器展示了跨模型架构的异构性能行为。现有的用于在许多用户之间仲裁这些昂贵训练资源的加速器集群调度程序，已经展示了如何优化各种多任务、多用户目标，如公平性和完成时间。不幸的是，现有的调度器在很大程度上没有考虑性能的异构性。

在本文中提出了一个异构感知调度器Gavel，它系统地生成了的一系列调度策略。Gavel将这些策略表示为优化问题，然后使用我们称为有效吞吐量的抽象，系统地将这些问题转换为异构感知版本。然后，Gavel使用基于轮的调度机制来确保作业在给定的目标调度策略下得到理想的分配。异构感知策略允许异质集群维持较高的输入负载，并将最终目标 (如完工时间和平均工作完成时间)与异构不可知策略相比提高1.4和3.5。

## 2. 背景

### 2.1 深度神经网络 (DNN)训练

DNN训练以迭代的形式进行。在每一轮的迭代中，DNN处理一组输入 (minibatch)，并持续更新模型参数（采用来源于输入minibatch的梯度下降）。每个minibatch的大小是一样的，这意味着使用短的分析运行（以分钟为单位）来建模训练吞吐量。Gravel在其吞吐量评估器中利用了这一事实，作业典型地运行时间长（从小时到天），并且可以被分布式地分配到woker节点。

现代的DNN调度器利用了DNN训练是迭代的这一事实，在迭代边界处暂停和恢复训练，这确保作业可以在现有物理资源上进行时间复用。当作业被暂停时，需要将最新的模型参数检查为稳定存储，以确保训练进度不会丢失。在这项工作中，我们展示了如何分配时间，以优化各种单一和多任务的目标。