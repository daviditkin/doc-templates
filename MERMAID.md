```mermaid
block-beta

columns 3
  block:table1
    columns 1
    A1
    A2
    A3
  end
  block
    columns 1
  end
  block:table2
    columns 1
    B1
    B2
    B3
  end
  A1 --> B2
  A1 --> B3
%%  style B fill:#969,stroke:#333,stroke-width:4px
```

```mermaid
flowchart LR
    %% Set the main graph direction (Left to Right)
    
    subgraph Table_A [Table A]
        columns 1
        direction TB  %% Set subgraph direction to Top to Bottom
        A1[Row A1]
        A2[Row A2]
        A3[Row A3]
        
        %% Enforce the order with invisible links
        A1 --> A2
        A2 --> A3
        %% Hide these links
        linkStyle 0 stroke-width:0px
        linkStyle 1 stroke-width:0px
    end
    
    subgraph Table_B [Table B]
        direction TB
        B1[Row B1]
        B2[Row B2]
        B3[Row B3]
        
        %% Enforce the order with invisible links
        B1 --> B2
        B2 --> B3
        %% Hide these links
        linkStyle 2 stroke-width:0px
        linkStyle 3 stroke-width:0px
    end

    %% Define mappings between tables
    A1 -. Mapping 1 .-> B2
    A2 -. Mapping 2 .-> B1
    A3 -. Mapping 3 .-> B3


```

```mermaid
flowchart TD
    %% Define styles
    classDef functionAppStyle fill:#f9f,stroke:#333,stroke-width:1px;
    classDef storageStyle fill:#ff9,stroke:#333,stroke-width:1px;
    classDef apiStyle fill:#9ff,stroke:#333,stroke-width:1px;
    classDef logStyle fill:#9f9,stroke:#333,stroke-width:1px;
    classDef workbookStyle fill:#99f,stroke:#333,stroke-width:1px;

    A["Function App<br>(Connector)"]:::functionAppStyle
    B[Azure Blob Storage<br>last_ingest datetime]:::storageStyle
    C[BloodHound Enterprise APIs]:::apiStyle
    D[Transform Data into<br>BloodhoundEnterpriseData Struct]:::functionAppStyle
    E[Azure Log Ingestion Endpoint]:::logStyle
    F[BloodHoundLogs_CL<br>in Log Analytics]:::logStyle
    G[Azure Workbooks]:::workbookStyle

    %% Workflow
    A -->|Read last_ingest datetime| B
    B -->|Provide last_ingest datetime| A
    A -->|Invoke APIs with last_ingest| C
    C -->|Return Data| A
    A -->|Transform Data| D
    D -->|Upload Data| E
    E -->|Ingested Data| F
    F -->|Query Data| G
```

```mermaid
flowchart TD
    %% Define styles
    classDef functionAppStyle fill:#f9f,stroke:#333,stroke-width:1px;
    classDef storageStyle fill:#ff9,stroke:#333,stroke-width:1px;
    classDef apiStyle fill:#9ff,stroke:#333,stroke-width:1px;
    classDef logStyle fill:#9f9,stroke:#333,stroke-width:1px;
    classDef decisionStyle fill:#fff,stroke:#333,stroke-width:1px,shape:diamond;
    classDef processStyle fill:#f9f,stroke:#333,stroke-width:1px;
    classDef workbookStyle fill:#99f,stroke:#333,stroke-width:1px;

    %% Nodes
    A["Start Function App<br>(Connector)"]:::functionAppStyle
    B[Read last_ingest datetime<br>from Azure Blob Storage]:::storageStyle
    C[Get last analysis time<br>from BloodHound]:::apiStyle
    D{Is last_ingest < last analysis time?}:::decisionStyle
    E[Invoke BloodHound APIs<br>with last_ingest datetime]:::apiStyle
    F[Transform Data into<br>BloodhoundEnterpriseData Struct]:::processStyle
    G[Send Data to Azure Log Ingestion Endpoint]:::logStyle
    H[Update last_ingest datetime<br>in Azure Blob Storage]:::storageStyle
    I[Data Stored in BloodHoundLogs_CL]:::logStyle
    J[Azure Workbooks Query and Visualize Data]:::workbookStyle
    K[End]:::functionAppStyle

    %% Workflow
    A --> B
    B --> C
    C --> D
    D -->|Yes| E
    D -->|No| K
    E --> F
    F --> G
    G --> I
    I --> J
    G --> H
    H --> K
```

```mermaid
flowchart LR
    %% Define styles
    classDef sourceStyle fill:#455,stroke:#333,stroke-width:1px;
    classDef targetStyle fill:#655,stroke:#333,stroke-width:1px;

    %% Source data fields
    subgraph Source Data
        direction TB %%
        A1[PostureDataType<br>timestamp]:::sourceStyle
        A2[PostureDataType<br>domain_sid]:::sourceStyle
        A3[PostureDataType<br>exposure_index]:::sourceStyle
        A4[PostureDataType<br>domain_id]:::sourceStyle
        A5[Calculated<br>domain_name]:::sourceStyle
    end

    %% Target data fields
    subgraph Transformed Data
        direction TB %%
        B1[BloodhoundEnterpriseData<br>TimeGenerated]:::targetStyle
        B2[BloodhoundEnterpriseData<br>DomainSID]:::targetStyle
        B3[BloodhoundEnterpriseData<br>ExposureIndex]:::targetStyle
        B4[BloodhoundEnterpriseData<br>DomainID]:::targetStyle
        B5[BloodhoundEnterpriseData<br>DomainName]:::targetStyle
    end

    %% Internal ordering
    A1 --> A2 --> A3 --> A4 --> A5

    %% Field mappings
    A1 -->|timestamp| B5
    A2 -->|domain_sid| B2
    A3 -->|exposure_index| B3
    A4 -->|domain_id| B4
    A5 -->|domain_name| B5
```

```mermaid
flowchart LR
    classDef sourceStyle fill:#f9f,stroke:#333,stroke-width:1px;
    classDef targetStyle fill:#9f9,stroke:#333,stroke-width:1px;

    subgraph Source Data
        A1[AttackPathType<br>timestamp]:::sourceStyle
        A2[AttackPathType<br>id]:::sourceStyle
        A3[AttackPathType<br>type]:::sourceStyle
        A4[AttackPathType<br>exposure]:::sourceStyle
        A5[PathTitle from<br>pathMap]:::sourceStyle
    end

    subgraph Transformed Data
        B1[BloodhoundEnterpriseData<br>TimeGenerated]:::targetStyle
        B2[BloodhoundEnterpriseData<br>PathID]:::targetStyle
        B3[BloodhoundEnterpriseData<br>PathType]:::targetStyle
        B4[BloodhoundEnterpriseData<br>Exposure]:::targetStyle
        B5[BloodhoundEnterpriseData<br>PathTitle]:::targetStyle
        B6[BloodhoundEnterpriseData<br>Severity]:::targetStyle
        B7[BloodhoundEnterpriseData<br>DataType=attackPathData]:::targetStyle
    end

    A1 -->|timestamp| B1
    A2 -->|id| B2
    A3 -->|type| B3
    A4 -->|exposure| B4
    A5 -->|path title| B5
    A4 -->|"calculateSeverity(exposure)"| B6
```
