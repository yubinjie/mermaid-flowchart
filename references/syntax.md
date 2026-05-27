# Mermaid 语法速查

## 流程图 (Flowchart)

### 方向
```
flowchart TD   / TB   上→下 (推荐)
flowchart LR           左→右
flowchart BT           下→上
flowchart RL           右→左
```

### 节点形状
```
A[矩形 - 流程步骤]
B(圆角矩形 - 开始/结束)
C{菱形 - 条件判断}
D[[子程序]]
E[(数据库)]
F((圆形))
G>旗帜 - 输入/输出]
H{{六边形}}
I[/平行四边形/]
```

### 连接线
```
A --> B          实线箭头
A --- B          实线无箭头
A -.-> B         虚线箭头
A -.- B          虚线无箭头
A ==> B          粗线箭头
A --o B          圆头
A --x B          X头
A -->|文字| B    带标签箭头
A -->|文字| B
```

### 条件分支（模板）
```
flowchart TD
    A[开始] --> B{条件?}
    B -->|是| C[处理A]
    B -->|否| D[处理B]
    C --> E[结束]
    D --> E
```

### 子图 (Subgraph)
```
flowchart TD
    subgraph 数据预处理
        A[读取] --> B[清洗]
    end
    subgraph 模型训练
        B --> C[训练]
        C --> D[验证]
    end
```

### 样式
```
style A fill:#f9f,stroke:#333,stroke-width:2px
style B fill:#bbf,stroke:#f66,stroke-width:2px,color:#fff

classDef highlight fill:#ffe1cc,stroke:#ff9800
class A,B highlight
```

## 时序图 (Sequence Diagram)

```
sequenceDiagram
    participant U as 用户
    participant S as 服务器
    participant DB as 数据库

    U->>S: 发送请求
    S->>DB: 查询数据
    DB-->>S: 返回结果
    S-->>U: 响应
```

箭头类型：
- `->>` 实线箭头
- `-->>` 虚线箭头  
- `-x` 带X箭头
- `-)` 异步箭头

激活/销毁：
```
S->>+S: 激活
S-->>-S: 销毁
```

Note 注释：
```
Note right of U: 用户点击登录
Note over S,DB: 事务范围
```

Loop/Alt/Opt：
```
loop 每秒
    U->>S: 心跳
end

alt 成功
    S-->>U: OK
else 失败
    S-->>U: Error
end

opt 可选
    S->>DB: 记录日志
end
```

## 类图 (Class Diagram)

```
classDiagram
    class Animal {
        +String name
        +int age
        +eat() void
        +sleep() void
    }
    class Dog {
        +bark() void
    }
    class Cat {
        +meow() void
    }
    Animal <|-- Dog : 继承
    Animal <|-- Cat : 继承
```

关系：
- `<|--` 继承
- `*--` 组合（强拥有）
- `o--` 聚合（弱拥有）
- `-->` 关联
- `..>` 依赖
- `..|>` 实现接口

可见性：`+`public `-`private `#`protected `~`package

## 状态图 (State Diagram)

```
stateDiagram-v2
    [*] --> 空闲
    空闲 --> 运行 : 启动
    运行 --> 暂停 : 暂停信号
    暂停 --> 运行 : 恢复
    运行 --> 结束 : 完成
    结束 --> [*]
    
    state 运行 {
        [*] --> 加载
        加载 --> 计算
        计算 --> 加载 : 迭代
        计算 --> [*]
    }
```

## 甘特图 (Gantt Chart)

```
gantt
    title 项目计划
    dateFormat  YYYY-MM-DD
    section 阶段一
    需求分析    :a1, 2026-01-01, 14d
    方案设计    :a2, after a1, 7d
    section 阶段二
    开发        :b1, 2026-01-22, 21d
    测试        :b2, 2026-02-12, 14d
```

状态：`done` `active` `crit` 或 `after <id>`

## 饼图 (Pie Chart)

```
pie title 数据分布
    "类别A" : 45
    "类别B" : 30
    "类别C" : 15
    "其他" : 10
```

## Git 图

```
gitGraph
    commit
    commit
    branch develop
    checkout develop
    commit
    commit
    checkout main
    merge develop
    commit
```

## 常见问题

| 问题 | 解决 |
|-----|------|
| 节点文字含特殊字符 | 用双引号包裹：`A["包含:特殊(字符)"]` |
| 子图之间连线 | 子图外的节点连接到子图内节点 ID |
| 文字太长 | 用 `<br/>` 换行：`A["第一行<br/>第二行"]` |
| 中文标点问题 | 节点标签中文标点无需转义，直接用 |

## 渲染方式

1. **在线**：https://mermaid.live（免费，所见即所得）
2. **GitHub/GitLab**：Markdown 中直接写 ```` ```mermaid ```` 代码块
3. **VS Code**：安装 "Mermaid Preview" 插件（bierner.markdown-mermaid）
4. **Notion**：/mermaid 命令
5. **飞书文档**：```mermaid 代码块
6. **命令行导出**：`mmdc -i input.mmd -o output.png`
7. **Python 渲染**：`pip install mermaid`（实验性）
