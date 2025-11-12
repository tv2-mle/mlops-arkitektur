# MLOps Arkitektur

Indeholder Mermaid diagrammer på en eksempel MLOps stack template.

## MLOps Stack Architecture Overview

This diagram shows the complete MLOps infrastructure stack with RHEL-based servers, AD authentication, Jenkins CI/CD pipeline, and deployment environments.

```mermaid
graph TB
    subgraph "Authentication Layer"
        AD[Active Directory<br/>Authentication]
    end
    
    subgraph "Developer Environment"
        DEV1[RHEL Developer<br/>Machine 1]
        DEV2[RHEL Developer<br/>Machine 2]
        DEV3[RHEL Developer<br/>Machine 3]
    end
    
    subgraph "Jenkins Server"
        JENKINS[Jenkins Master]
        subgraph "Jenkins Components"
            BUILD[Build Component]
            DEPLOY[Deploy Component]
        end
    end
    
    subgraph "Artifact Repository"
        JFROG[JFrog Artifactory]
    end
    
    subgraph "Test Environment"
        TEST[RHEL Test Server<br/>ML Applications]
    end
    
    subgraph "Production Environment"
        PROD[RHEL Production Server<br/>ML Applications]
    end
    
    subgraph "GPU-environment"
        GPU_TEST[GPU Test]
        GPU_PROD[GPU Prod]
    end
    
    subgraph "Rancher Kubernetes Environment"
        RANCHER_TEST[Test Cluster]
        RANCHER_PROD[Prod Cluster]
    end
    
    AD -.->|Authenticates| DEV1
    AD -.->|Authenticates| DEV2
    AD -.->|Authenticates| DEV3
    AD -.->|Authenticates| JENKINS
    AD -.->|Authenticates| TEST
    AD -.->|Authenticates| PROD
    
    DEV1 -->|Push Code| JENKINS
    DEV2 -->|Push Code| JENKINS
    DEV3 -->|Push Code| JENKINS
    
    JENKINS --> BUILD
    BUILD --> JFROG
    BUILD --> DEPLOY
    
    DEPLOY -->|Deploy| TEST
    DEPLOY -->|Deploy| PROD
    DEPLOY -->|Deploy| GPU_TEST
    DEPLOY -->|Deploy| GPU_PROD
    DEPLOY -->|Deploy| RANCHER_TEST
    DEPLOY -->|Deploy| RANCHER_PROD
    
    style AD fill:#e1f5ff,stroke:#0066cc,stroke-width:3px
    style JENKINS fill:#fff4e6,stroke:#ff9800,stroke-width:2px
    style BUILD fill:#ffe6e6,stroke:#f44336,stroke-width:2px
    style DEPLOY fill:#e8f5e9,stroke:#4caf50,stroke-width:2px
    style JFROG fill:#e0f2f1,stroke:#00897b,stroke-width:2px
    style TEST fill:#fff9c4,stroke:#fbc02d,stroke-width:2px
    style PROD fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px
    style GPU_TEST fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    style GPU_PROD fill:#f8bbd0,stroke:#880e4f,stroke-width:2px
    style RANCHER_TEST fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    style RANCHER_PROD fill:#bbdefb,stroke:#0d47a1,stroke-width:2px
```

## Detailed CI/CD Pipeline Flow

This diagram shows the detailed flow of the CI/CD pipeline from development to production.

```mermaid
flowchart LR
    subgraph "Development"
        DEV[Developers<br/>RHEL Machines]
    end
    
    subgraph "Authentication"
        AD[AD Authentication]
    end
    
    subgraph "Jenkins CI/CD"
        direction TB
        J[Jenkins Server]
        B[Build:<br/>- Compile Code<br/>- Run Tests<br/>- Create Artifacts]
        D[Deploy:<br/>- Deploy to Test<br/>- Deploy to Prod]
        J --> B
        B --> D
    end
    
    subgraph "Artifact Repository"
        JFROG[JFrog Artifactory]
    end
    
    subgraph "Environments"
        direction TB
        T[Test Environment<br/>RHEL Server<br/>ML Apps]
        P[Production Environment<br/>RHEL Server<br/>ML Apps]
    end
    
    subgraph "GPU-environment"
        direction TB
        GT[GPU Test]
        GP[GPU Prod]
    end
    
    subgraph "Rancher Kubernetes Environment"
        direction TB
        RT[Test Cluster]
        RP[Prod Cluster]
    end
    
    DEV -->|Code Commit| J
    AD -.->|Auth| DEV
    AD -.->|Auth| J
    AD -.->|Auth| T
    AD -.->|Auth| P
    
    B -->|Store Artifacts| JFROG
    D -->|Auto Deploy| T
    D -->|Manual Approval| P
    D -->|Deploy| GT
    D -->|Deploy| GP
    D -->|Deploy| RT
    D -->|Deploy| RP
    
    T -.->|Validation| D
    
    style AD fill:#e1f5ff,stroke:#0066cc,stroke-width:3px
    style B fill:#ffe6e6,stroke:#f44336,stroke-width:2px
    style D fill:#e8f5e9,stroke:#4caf50,stroke-width:2px
    style JFROG fill:#e0f2f1,stroke:#00897b,stroke-width:2px
    style T fill:#fff9c4,stroke:#fbc02d,stroke-width:2px
    style P fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px
    style GT fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    style GP fill:#f8bbd0,stroke:#880e4f,stroke-width:2px
    style RT fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    style RP fill:#bbdefb,stroke:#0d47a1,stroke-width:2px
```

## Component Architecture

This diagram shows the technical architecture of each component in the MLOps stack.

```mermaid
graph TD
    subgraph "Foundation Layer"
        AD[Active Directory<br/>Central Authentication]
    end
    
    subgraph "Development Layer"
        direction LR
        D1[Dev Machine 1<br/>RHEL Linux<br/>Python/ML Tools]
        D2[Dev Machine 2<br/>RHEL Linux<br/>Python/ML Tools]
        D3[Dev Machine 3<br/>RHEL Linux<br/>Python/ML Tools]
    end
    
    subgraph "CI/CD Layer"
        direction TB
        JS[Jenkins Server<br/>RHEL Linux]
        BC[Build Component<br/>- Code Compilation<br/>- Unit Tests<br/>- Model Training<br/>- Artifact Creation]
        DC[Deploy Component<br/>- Environment Prep<br/>- Deployment Scripts<br/>- Health Checks<br/>- Rollback]
        JFROG[JFrog Artifactory<br/>- Artifact Storage<br/>- Version Management]
        JS --> BC
        BC --> JFROG
        BC --> DC
    end
    
    subgraph "Runtime Layer"
        direction LR
        TE[Test Server<br/>RHEL Linux<br/>ML Applications<br/>Test Data]
        PE[Prod Server<br/>RHEL Linux<br/>ML Applications<br/>Production Data]
    end
    
    subgraph "GPU-environment"
        direction LR
        GPU_T[GPU Test<br/>ML Training/Inference]
        GPU_P[GPU Prod<br/>ML Training/Inference]
    end
    
    subgraph "Rancher Kubernetes Environment"
        direction LR
        RANCHER_T[Test Cluster<br/>Container Orchestration]
        RANCHER_P[Prod Cluster<br/>Container Orchestration]
    end
    
    AD ==>|Secures| D1
    AD ==>|Secures| D2
    AD ==>|Secures| D3
    AD ==>|Secures| JS
    AD ==>|Secures| TE
    AD ==>|Secures| PE
    
    D1 & D2 & D3 -->|Git Push| JS
    DC -->|Deploys| TE
    DC -->|Deploys| PE
    DC -->|Deploys| GPU_T
    DC -->|Deploys| GPU_P
    DC -->|Deploys| RANCHER_T
    DC -->|Deploys| RANCHER_P
    
    style AD fill:#e1f5ff,stroke:#0066cc,stroke-width:4px
    style JS fill:#fff4e6,stroke:#ff9800,stroke-width:2px
    style BC fill:#ffe6e6,stroke:#f44336,stroke-width:2px
    style DC fill:#e8f5e9,stroke:#4caf50,stroke-width:2px
    style JFROG fill:#e0f2f1,stroke:#00897b,stroke-width:2px
    style TE fill:#fff9c4,stroke:#fbc02d,stroke-width:2px
    style PE fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px
    style GPU_T fill:#fce4ec,stroke:#c2185b,stroke-width:2px
    style GPU_P fill:#f8bbd0,stroke:#880e4f,stroke-width:2px
    style RANCHER_T fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    style RANCHER_P fill:#bbdefb,stroke:#0d47a1,stroke-width:2px
```

## Deployment Sequence

This sequence diagram shows the typical deployment flow from code commit to production.

```mermaid
sequenceDiagram
    participant Dev as Developer<br/>(RHEL Machine)
    participant AD as Active Directory
    participant Jenkins as Jenkins Server
    participant Build as Build Component
    participant JFrog as JFrog Artifactory
    participant Deploy as Deploy Component
    participant Test as Test Server<br/>(RHEL)
    participant Prod as Prod Server<br/>(RHEL)
    participant GPU_T as GPU Test
    participant GPU_P as GPU Prod
    participant Rancher_T as Rancher Test<br/>Cluster
    participant Rancher_P as Rancher Prod<br/>Cluster
    
    Dev->>AD: Authenticate
    AD-->>Dev: Access Granted
    
    Dev->>Jenkins: Push Code
    Jenkins->>AD: Verify Credentials
    AD-->>Jenkins: Authorized
    
    Jenkins->>Build: Trigger Build
    Build->>Build: Compile & Test
    Build->>Build: Train ML Model
    Build->>Build: Create Artifacts
    Build->>JFrog: Store Artifacts
    JFrog-->>Build: Artifacts Stored
    Build-->>Jenkins: Build Success
    
    Jenkins->>Deploy: Start Deployment
    Deploy->>Test: Deploy to Test
    Test->>Test: Run ML Application
    Test-->>Deploy: Health Check OK
    
    Deploy->>GPU_T: Deploy to GPU Test
    GPU_T-->>Deploy: Health Check OK
    
    Deploy->>Rancher_T: Deploy to Rancher Test
    Rancher_T-->>Deploy: Health Check OK
    
    Deploy->>Deploy: Await Approval
    Deploy->>Prod: Deploy to Production
    Prod->>Prod: Run ML Application
    Prod-->>Deploy: Health Check OK
    
    Deploy->>GPU_P: Deploy to GPU Prod
    GPU_P-->>Deploy: Health Check OK
    
    Deploy->>Rancher_P: Deploy to Rancher Prod
    Rancher_P-->>Deploy: Health Check OK
    
    Deploy-->>Jenkins: Deployment Complete
    Jenkins-->>Dev: Notification: Success
```

## Infrastructure Components

### Developer Machines (RHEL)
- Red Hat Enterprise Linux workstations
- Development tools: Python, ML frameworks, Git
- Connected to AD for authentication

### Active Directory (AD)
- Central authentication and authorization
- Manages access to all infrastructure components
- Provides SSO capabilities across the MLOps stack

### Jenkins Server
- Orchestrates CI/CD pipeline
- **Build Component:**
  - Code compilation and validation
  - Unit and integration testing
  - ML model training and validation
  - Artifact packaging
  
- **Deploy Component:**
  - Environment provisioning
  - Application deployment
  - Configuration management
  - Health checks and monitoring

### Test Environment (RHEL)
- Pre-production testing server
- Runs ML applications with test data
- Validates deployments before production

### Production Environment (RHEL)
- Production ML application server
- Serves real-time ML predictions
- Monitored and maintained for high availability
