---

layout: post
title: 大模型可观测性
category: 架构
tags: MachineLearning
keywords: llm inference

---

<script>
  MathJax = {
    tex: {
      inlineMath: [['$', '$']], // 支持 $和$$ 作为行内公式分隔符
      displayMath: [['$$', '$$']], // 块级公式分隔符
    },
    svg: {
      fontCache: 'global'
    }
  };
</script>
<script async src="/public/js/mathjax/es5/tex-mml-chtml.js"></script>


* TOC
{:toc}

## 简介（未完成）

与传统微服务应用所关注的黄金三指标（请求数，错误，耗时）类比，我们认为 AI 应用的黄金三指标可能是 Token，Error，Duration。
1. 耗时主要关注的是模型推理延迟，也就是在推理过程中我们通常需要关注模型的首包延迟，即 TTFT(Time to first token)，这个指标反映了相应的速度，还有像 TPOT (Time Per Output Token) 反映生成的效率和流畅度。另外一个比较重要的指标就是吞吐率。吞吐率可以衡量我们这个模型本身，能够同时去支撑多少个推理请求。所以这几个指标是需要进行一些平衡的，三个指标不可能同时满足得特别好。
2. Token 可能是 AI 应用最重要的一个指标，所以每次请求会记录 Token 的消耗情况，甚至我们需要精确地区分 Input Token 和 Output Token 的消耗，因为大家知道模型的定价里面 Input Token 和 Output Token 是不一样的，我们在成本核算的时候，会将输入 Token 和输出 Token 分别进行统计。

## 耗时耗在哪？ ==> 全链路追踪

业务/应用层，还是推理引擎层？

## 问答质量

我们要解决模型回答得好不好，每次模型的升级和优化，都需要建立一个基线，并且确保模型的迭代满足这个基线，否则回答的质量会导致用户体验受损。为此，我们把模型的 input/output 全部都采集到日志平台中，接下来我们可以筛选出一批记录，通过数据加工，引用外部的裁判员模型，对当前这个模型回答的输入输出结果进行一个评估。