```mermaid
flowchart TB
    %% Enterprise Architecture — Four Domain Layers

    subgraph B[Business Architecture]
        B1[Strategy & Goals]
        B2[Business Processes]
        B3[Org Structure & Capabilities]
        B4[KPIs & Performance]
    end

    subgraph D[Data Architecture]
        D1[Data Governance & Quality]
        D2[Data Models & Entities]
        D3[Master & Reference Data]
        D4[Analytics & Reporting]
    end

    subgraph A[Application Architecture]
        A1[Enterprise Apps (ERP/CRM)]
        A2[APIs & Microservices]
        A3[Integration Layer / ESB]
        A4[Front-end & Portals]
    end

    subgraph T[Technology Architecture]
        T1[Cloud Infrastructure]
        T2[Networking & Security]
        T3[DevOps & CI/CD Pipelines]
        T4[Platforms & Runtimes]
    end

    B --- D
    D --- A
    A --- T
```
