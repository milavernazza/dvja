# DevSecOps Pipeline for DVJA

This project implements a comprehensive DevSecOps pipeline for the Damn Vulnerable Java Application.

## Pipeline Workflow

```mermaid
graph TD
    A[Developer Push/PR] --> B[Checkout Code]
    B --> C[Set up Java 11]
    C --> D[Build with Maven]
    D --> E[CodeQL Analysis - Non-blocking]
    B --> F[Gitleaks Secret Scan - Non-blocking]
    
    %% Docker operations
    D --> G[Build Docker Image]
    G --> H[Tag Docker Image with Version]
    H --> I[Push to Docker Hub]
    
    %% Security scanning
    D --> J[Trivy SCA Scan - Non-blocking]
    I --> K[Trivy Container Scan - Non-blocking]
    
    %% Results processing
    E --> L[Parse Security Results]
    F --> L
    J --> L
    K --> L
    
    %% Issue creation
    L --> M{High/Critical?}
    M -->|Yes| N[Create GitHub Issues]
    M -->|No| O[Log in Summary]
    
    %% Issue categorization
    N --> P[Code Vulnerabilities]
    N --> Q[Secret Exposures]
    N --> R[Dependency Issues]
    N --> S[Container Vulnerabilities]
    
    %% Final reporting
    P --> T[Security Summary]
    Q --> T
    R --> T
    S --> T
    O --> T
    
    %% Deployment step
    T --> U[Deploy DVJA Application]
    
    classDef build fill:#b5e7ff,stroke:#0066cc,stroke-width:2px
    classDef scan fill:#ffe6e6,stroke:#990000,stroke-width:2px
    classDef results fill:#e6ffe6,stroke:#006600,stroke-width:2px
    classDef issues fill:#ffffcc,stroke:#cccc00,stroke-width:2px
    
    class B,C,D,G,H,I build
    class E,F,J,K scan
    class L,M,O,T results
    class N,P,Q,R,S issues
```

## Security Findings

The pipeline automatically scans for security issues and creates GitHub issues for any findings categorized as High or Critical.