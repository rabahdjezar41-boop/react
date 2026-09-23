# React Architecture Overview

This repository is a monorepo containing the React runtime and the React Compiler. The main React packages live outside `compiler/`, while compiler-specific logic and tooling are isolated under that directory.

```mermaid
flowchart LR
    subgraph App["Application Layer"]
        U["User App\nReact Components"]
    end

    subgraph Core["Core React"]
        R["React\nHooks, Elements, State"]
        Rec["Reconciler\nUpdate Scheduling"]
        S["Scheduler"]
        SH["Shared Utilities\nEvents, Internals"]
    end

    subgraph Renderers["Renderers"]
        DOM["React DOM\nBrowser / SSR"]
        RN["React Native\nMobile UI"]
        Server["React Server\nStreaming / Rendering"]
    end

    subgraph Compiler["React Compiler"]
        C["compiler/\nTransform Pipeline"]
        O["Optimization Passes"]
        Rust["Rust Port\nExperimental Compiler Work"]
    end

    U --> R
    R --> Rec
    Rec --> S
    R --> SH
    Rec --> DOM
    Rec --> RN
    Rec --> Server

    C --> O
    O --> Rust
    O --> DOM

    classDef app fill:#E8F3FF,stroke:#3B82F6,color:#111827;
    classDef core fill:#E0F7EC,stroke:#16A34A,color:#111827;
    classDef renderer fill:#FDE68A,stroke:#D97706,color:#111827;
    classDef compiler fill:#F3E8FF,stroke:#9333EA,color:#111827;

    class U app;
    class R,Rec,S,SH core;
    class DOM,RN,Server renderer;
    class C,O,Rust compiler;
```

## Notes

- The `react` packages provide the core UI model and reconciliation engine.
- Renderer integrations such as DOM and Native adapt the reconciler to different targets.
- The compiler pipeline lives in `compiler/` and contains optimization and transformation steps.
- The Rust port work under `compiler/crates/` represents an experimental compiler implementation path.
