# 网络安全架构

## 整体架构图

```mermaid
graph TB
    subgraph 安全开发与编码
        A1[编码规范<br/>输入校验/防注入/防爆破]
        A2[部署流程<br/>TKE镜像安全扫描]
        A3[开源组件管理<br/>漏洞检测+SBOM清单]
        A1 --> A2
        A2 --> A3
    end

    subgraph 数据全生命周期安全
        B1[传输层<br/>TLS 1.3 + SSL证书服务]
        B2[存储层<br/>AES-256 + CASB加密服务]
        B3[审计层<br/>CLS日志服务 + 操作审计]
        B1 --> B2
        B2 --> B3
    end

    subgraph 云环境安全
        C1[运维安全<br/>CAM最小权限]
        C2[网络安全<br/>VPC+安全组+WAF+API网关]
        C3[渗透测试<br/>自动化扫描+容器安全检测]
        C1 --> C2
        C2 --> C3
    end

    subgraph 权限管理_CAM
        D1[RBAC模型<br/>最小权限分配]
        D2[时效控制<br/>临时凭证最长72h]
        D3[审计记录<br/>180天可追溯]
        D1 --> D2
        D2 --> D3
    end

    subgraph 安全响应机制
        E1[SLA约定<br/>高危2h/中危4h/低危24h]
        E2[应急流程<br/>检测→隔离→修复→验证]
        E3[自测机制<br/>基线+扫描+复检]
        E1 --> E2
        E2 --> E3
    end

    A3 --> B1
    B3 --> C1
    C3 --> D1
    D3 --> E1

    style A1 fill:#e1f5ff
    style A2 fill:#e1f5ff
    style A3 fill:#e1f5ff
    style B1 fill:#fff3e0
    style B2 fill:#fff3e0
    style B3 fill:#fff3e0
    style C1 fill:#f3e5f5
    style C2 fill:#f3e5f5
    style C3 fill:#f3e5f5
    style D1 fill:#e8f5e9
    style D2 fill:#e8f5e9
    style D3 fill:#e8f5e9
    style E1 fill:#ffebee
    style E2 fill:#ffebee
    style E3 fill:#ffebee
```

## 安全防护流程图

```mermaid
flowchart LR
    Start([开发启动]) --> Code[安全编码]
    Code --> Scan[代码扫描]
    Scan --> Component[组件检测]
    Component --> Build[镜像构建]
    Build --> ImageScan[镜像扫描]
    ImageScan --> Deploy{安全检查通过?}
    Deploy -->|是| TKE[部署到TKE]
    Deploy -->|否| Fix[安全修复]
    Fix --> Scan
    
    TKE --> Runtime[运行时监控]
    Runtime --> WAF[WAF防护]
    WAF --> APIGateway[API网关]
    APIGateway --> App[应用服务]
    
    App --> Log[CLS日志审计]
    Log --> Alert{安全告警?}
    Alert -->|高危| Emergency2h[2h应急响应]
    Alert -->|中危| Emergency4h[4h应急响应]
    Alert -->|低危| Emergency24h[24h应急响应]
    
    Emergency2h --> Isolate[隔离处理]
    Emergency4h --> Isolate
    Emergency24h --> Isolate
    
    Isolate --> Recovery[修复验证]
    Recovery --> Complete([响应完成])

    style Start fill:#4caf50
    style Code fill:#2196f3
    style Scan fill:#2196f3
    style Component fill:#2196f3
    style ImageScan fill:#2196f3
    style Deploy fill:#ff9800
    style Fix fill:#f44336
    style TKE fill:#9c27b0
    style WAF fill:#9c27b0
    style Alert fill:#ff9800
    style Emergency2h fill:#f44336
    style Emergency4h fill:#ff9800
    style Emergency24h fill:#ffeb3b
    style Complete fill:#4caf50
```

## 数据安全流程

```mermaid
sequenceDiagram
    participant User as 用户
    participant Gateway as API网关/WAF
    participant App as 应用服务
    participant CASB as CASB加密服务
    participant DB as 数据库
    participant CLS as CLS日志服务

    User->>Gateway: HTTPS请求(TLS 1.3)
    Gateway->>Gateway: 输入校验/防注入检测
    Gateway->>App: 转发请求
    App->>App: 业务逻辑处理
    App->>CASB: 敏感数据加密请求
    CASB->>CASB: AES-256加密
    CASB->>App: 返回加密数据
    App->>DB: 存储加密数据
    App->>CLS: 记录操作日志
    DB->>App: 查询加密数据
    App->>CASB: 解密请求
    CASB->>App: 返回明文数据
    App->>Gateway: 返回响应(TLS 1.3)
    Gateway->>User: HTTPS响应
    App->>CLS: 记录访问日志
    
    Note over CLS: 180天可追溯审计
```

## 权限管理架构

```mermaid
graph TD
    subgraph CAM权限体系
        Admin[管理员]
        Dev[开发者]
        Ops[运维人员]
        Audit[审计员]
    end

    subgraph 资源访问
        TKE[TKE容器集群]
        VPC[VPC网络]
        CLS[CLS日志]
        CASB[CASB加密]
        DB[(数据库)]
    end

    subgraph 权限控制
        RBAC[RBAC模型]
        TempToken[临时凭证<br/>最长72h]
        Policy[最小权限策略]
    end

    Admin --> RBAC
    Dev --> RBAC
    Ops --> RBAC
    Audit --> RBAC
    
    RBAC --> TempToken
    TempToken --> Policy
    
    Policy --> TKE
    Policy --> VPC
    Policy --> CLS
    Policy --> CASB
    Policy --> DB
    
    TKE -.审计.-> AuditLog[CAM操作审计]
    VPC -.审计.-> AuditLog
    CLS -.审计.-> AuditLog
    CASB -.审计.-> AuditLog
    DB -.审计.-> AuditLog
    
    AuditLog --> Retention[180天保留期]

    style Admin fill:#f44336,color:#fff
    style Dev fill:#2196f3,color:#fff
    style Ops fill:#ff9800,color:#fff
    style Audit fill:#4caf50,color:#fff
    style RBAC fill:#9c27b0,color:#fff
    style TempToken fill:#ff5722,color:#fff
    style Policy fill:#00bcd4
```

## 网络安全架构

```mermaid
graph TB
    Internet([互联网])
    
    subgraph 边界防护层
        WAF[腾讯云WAF]
        DDoS[DDoS防护]
    end
    
    subgraph API网关层
        Kong[Konga API网关]
        RateLimit[限流控制]
        Auth[认证鉴权]
    end
    
    subgraph VPC隔离层
        VPC1[生产VPC]
        VPC2[测试VPC]
        VPC3[开发VPC]
    end
    
    subgraph 安全组策略
        SG1[Web层安全组]
        SG2[应用层安全组]
        SG3[数据层安全组]
    end
    
    subgraph TKE容器集群
        Pod1[业务Pod]
        Pod2[中间件Pod]
        Pod3[监控Pod]
    end
    
    subgraph 安全检测
        Scanner[漏洞扫描]
        Penetration[渗透测试]
        Baseline[基线检查]
    end

    Internet --> WAF
    Internet --> DDoS
    WAF --> Kong
    DDoS --> Kong
    Kong --> RateLimit
    RateLimit --> Auth
    
    Auth --> VPC1
    Auth --> VPC2
    Auth --> VPC3
    
    VPC1 --> SG1
    SG1 --> SG2
    SG2 --> SG3
    
    SG2 --> Pod1
    SG2 --> Pod2
    SG3 --> Pod3
    
    Pod1 -.定期检测.-> Scanner
    Pod1 -.定期检测.-> Penetration
    Pod1 -.定期检测.-> Baseline

    style WAF fill:#f44336,color:#fff
    style DDoS fill:#f44336,color:#fff
    style Kong fill:#ff9800,color:#fff
    style VPC1 fill:#2196f3,color:#fff
    style Scanner fill:#4caf50,color:#fff
    style Penetration fill:#4caf50,color:#fff
    style Baseline fill:#4caf50,color:#fff
```

## 安全响应时间线

```mermaid
gantt
    title 安全响应SLA时间线
    dateFormat X
    axisFormat %H:%M

    section 高危漏洞
    检测告警 :milestone, 0, 0m
    应急响应 :active, 0, 120m
    隔离处理 :120, 150m
    修复验证 :150, 180m

    section 中危漏洞
    检测告警 :milestone, 0, 0m
    应急响应 :0, 240m
    隔离处理 :240, 280m
    修复验证 :280, 320m

    section 低危漏洞
    检测告警 :milestone, 0, 0m
    应急响应 :0, 1440m
    修复验证 :1440, 1500m
```

## 安全检查清单

```mermaid
mindmap
  root((网络安全))
    安全开发
      编码规范<br/>输入校验/防注入/防爆破
      TKE镜像扫描
      开源组件漏洞检测<br/>SBOM清单
    数据安全
      传输: TLS 1.3 + SSL证书
      存储: AES-256 + CASB
      审计: CLS日志服务
    云环境
      运维: CAM最小权限
      网络: VPC + WAF + API网关
      容器: TKE安全检测
    权限管理
      RBAC模型
      临时凭证72h
      180天审计追溯
    应急响应
      SLA: 高危2h/中危4h/低危24h
      流程: 检测→隔离→修复→验证
      自测: 基线+扫描+复检
```

## 详细说明

### 1. 安全开发与编码

- **编码规范**：强制安全编码标准（输入校验、防注入、防爆破）
- **部署流程**：启用 TKE（腾讯云容器服务）镜像安全扫描
- **开源组件**：引入前通过腾讯云开源组件漏洞检测，生成 SBOM 清单，同步 TKE 镜像仓库做准入控制

### 2. 数据全生命周期安全

- **传输**：TLS 1.3 加密（搭配腾讯云 SSL 证书服务）
- **存储**：敏感数据 AES-256 加密（对接腾讯云 CASB 加密服务）
- **审计**：全链路操作日志同步腾讯云 CLS 日志服务，敏感操作增加相应审计日志

### 3. 云环境安全

- **运维安全**：CAM（腾讯云访问管理）最小权限操作授权
- **网络安全**：VPC 隔离 + 安全组策略 + 腾讯云 WAF + Konga API 网关接口防护
- **渗透测试**：定期自动化扫描（结合腾讯云渗透测试服务）+ TKE 容器安全检测

### 4. 权限管理（CAM）

- **权限分配**：CAM RBAC 模型，按角色分配最小权限，适配 TKE、VPC 等资源访问控制
- **时效控制**：CAM 临时访问凭证自动过期（最长 72h），杜绝长期权限泄露
- **审计记录**：所有权限操作同步 CLS 日志，结合 CAM 操作审计，可追溯 180 天

### 5. 安全响应机制

- **SLA 约定**：高危漏洞 2h 响应、中危 4h、低危 24h（对接腾讯云安全中心告警）
- **应急流程**：检测（安全中心告警）→ 隔离（VPC 子网隔离 / TKE 容器隔离）→ 修复 → 验证
- **自测机制**：腾讯云安全中心基线自测 + TKE 全量安全扫描 + 组件漏洞复检
