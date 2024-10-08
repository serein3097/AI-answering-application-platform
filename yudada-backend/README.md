项目介绍：
基于 Spring Boot、MySQL、Redis、ChatGLM、RxJava 和 SSE 的 AI 答题应用平台。用户可以通过 AI 快速生成题目并创建应用，经过管理员审核后，用户可以在线答题，并根据多种评分算法或 AI 生成回答总结。管理员还可以集中管理全站内容，并进行统计分析。
1.评分模块：基于 策略模式 实现了多种用户回答评分算法（如统计得分、AI 评分等），全局执行器会扫描策略类上的 自定义注解 并选取策略，相较于 if else 提高了系统的可扩展性。
2.基于 RxJava 的操作符链式调用处理 AI 异步数据流，先通过 map 获取并处理字符串、filter 过滤空值、flatMap 映射串为单个字符，再通过括号平衡算法准确拼接出单道题目，使逻辑简单清晰。
3.为防止用户多次提交重复答案，基于雪花算法为每次答题分配唯一 id，并通过数据库主键实现 幂等设计，避免了重复的脏数据。
4.由于每个应用的相同答案的AI评分应该相同，使用Caffeine本地缓存答案hash对应的AI评分结果，提高评分性能（约10s到1s)的同时大幅节约成本；并通过Redisson分布式锁解决缓存击穿问题。
