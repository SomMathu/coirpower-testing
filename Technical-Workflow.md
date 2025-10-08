# Technical Workflow Documentation

## System Components and Request Flow

### Frontend Components
- UI routes utilize Livewire single-action components for reactive interfaces
- Components are defined in `routes/web.php` using `ClassName::class` syntax
- Blade views and Livewire partials are organized under `resources/views/`

### API Layer
- RESTful JSON endpoints implemented in `app/Http/Controllers/Api/*`
- Handles authentication, data operations, and device telemetry
- Supports mobile app integration and third-party services

### System Workflow

The following diagram illustrates how user requests flow through the system, from web browsers and mobile apps to the backend services and databases.

```mermaid
flowchart LR
    Client[Web Browser/Mobile App]-->|Web Request| Web[Web Routes]
    Client-->|API Request| API[API Routes]
    
    Web-->|Dashboard| DC[Dashboard Controller]
    Web-->|Production| PC[Production Controllers]
    Web-->|Reports| RC[Report Views]
    
    API-->|Auth| Auth[Authentication]
    API-->|Data| Data[Data Controllers]
    
    DC-->|Query| DB[(Database)]
    PC-->|Query| DB
    Data-->|Query| DB
    
    DB-->|Events| Jobs[Background Jobs]
    Jobs-->|Process| Cache[Redis Cache]
    Jobs-->|Send| Notify[Notifications]
    
    style Client fill:#f9f,stroke:#333
    style Web fill:#bbf,stroke:#333
    style API fill:#bbf,stroke:#333
    style DB fill:#bfb,stroke:#333
```

### Module-Specific Workflows

#### Pith Module

```mermaid
flowchart TD
    subgraph PithModule["Pith Module: Core Functionalities & Components"]
        direction TB

        subgraph A["Pith Production Management"]
            LW_Prod["PithProductionList (Livewire)"]
            Ctrl_Prod["PithProductionController"]
            Model_Prod["PithProduction Model"]
            LW_Prod -- "Manages UI for Production Records" --> Ctrl_Prod
            Ctrl_Prod -- "Handles CRUD Operations" --> Model_Prod
        end

        subgraph B["Pith Inventory Management"]
            Trait_Inv["ManagePithInventory (Trait)"]
            Model_Inv["PithStockDepletion Model"]
            Trait_Inv -- "Provides Inventory Logic" --> Model_Inv
        end

        subgraph C["Pith Data Processing"]
            Job_Pith["ProcessDeviceDataForPithJob"]
            Model_Load["PithLoad Model"]
            Job_Pith -- "Processes Raw Device Data" --> Model_Prod
            Job_Pith -- "Creates Pith Loads" --> Model_Load
        end

        subgraph D["Pith Dashboard & Reporting"]
            Ctrl_Dash["DashboardPithLoadController"]
            Svc_Dash["PithProductionService"]
            Views_Dash["Pith Dashboard Views (Blade)"]
            Model_State["DeviceDataState Model"]
            Ctrl_Dash -- "Prepares Data for Display" --> Views_Dash
            Ctrl_Dash -- "Utilizes Business Logic" --> Svc_Dash
            Svc_Dash -- "Aggregates Data From" --> Model_Prod
            Svc_Dash -- "Aggregates Data From" --> Model_Load
            Svc_Dash -- "Integrates with" --> Model_State
        end

        subgraph E["Pith Master Data"]
            Model_Loc["PithLocation Model"]
            Model_Unit["PithUnit Model"]
        end

        A -- "Interacts with" --> C
        A -- "Interacts with" --> D
        B -- "Interacts with" --> A
        C -- "Feeds Data to" --> D
        E -- "Configures" --> A
        E -- "Configures" --> B
    end

    style A fill:#e0f7fa,stroke:#00796b
    style B fill:#fff3e0,stroke:#e65100
    style C fill:#e8f5e9,stroke:#33691e
    style D fill:#e3f2fd,stroke:#1565c0
    style E fill:#fce4ec,stroke:#ad1457
```

Notes: The Pith UI uses Livewire components (under `app/Livewire/Pith/*`) for interactive lists and modals. The components call models and use a `ManagePithInventory` trait for reusable inventory operations.

#### Fibre Module

```mermaid
flowchart TD
    subgraph FibreModule ["Fibre Module"]
        direction TB
        FL("FibreProductionList (Livewire)")
        FC("FibreProductionController (API/CRUD)")
        FM("FibreProduction / FibreBales")
        FService("FibreProductionService")
        ViewsF["/resources/views/livewire/fibre/*"]
    end

    FL -->|"selectProduction(id)"| FM
    FL -->|"deleteProduction"| FM
    FL -->|"updateBales"| FC
    FL -->|"renders"| ViewsF

    FC -->|"store/update/generateReport"| FM
    FC -->|"aggregateData"| FService
    FService -->|"reads"| Shift[(Shift model)]

    style FL fill:#ecfeff,stroke:#333
    style FC fill:#fff7ed,stroke:#333
    style FM fill:#fef2f2,stroke:#333
    style FService fill:#f0fdf4,stroke:#333
```

Notes: Fibre-related Livewire components paginate/filter fibre production records and call `summary_fibre_mini_bales` backed model classes. The Dashboard Fibre controller uses `FibreProductionService` to aggregate data for charts.

### IoT Module Architecture

The IoT module handles real-time device data processing and insights generation for both Pith and Fibre production units.

```mermaid
flowchart TD
    subgraph IOTModule ["IoT Data Processing & Insights"]
        direction TB
        IotP("IotPithController")
        IotF("IotFibreController")
        IotPB("IotPithBlocksController")
        DeviceModels[("Device Data Models")]
        SummaryTables[("Summary Tables")]
        ViewsI["/IoT Insights Views/"]
        Jobs("Device Data Processing Jobs")
        Device("Device Model")
    end

    IotP -->|"fetch events"| SummaryTables
    IotP -->|"fetch states"| DeviceModels
    IotF -->|"process data"| DeviceModels
    IotPB -->|"aggregate metrics"| DeviceModels
    
    Jobs -->|"update"| DeviceModels
    Jobs -->|"generate"| SummaryTables
    
    IotP & IotF & IotPB -->|"render"| ViewsI
    IotP & IotF & IotPB -->|"locate device"| Device

    style IotP fill:#ecfeff,stroke:#333
    style IotF fill:#ecfeff,stroke:#333
    style IotPB fill:#ecfeff,stroke:#333
    style Jobs fill:#fff7ed,stroke:#333
    style DeviceModels fill:#fef2f2,stroke:#333
    style SummaryTables fill:#fef2f2,stroke:#333
    style Device fill:#f0fdf4,stroke:#333
```

Notes: IoT controllers read from device-specific tables and transform raw events into calendar-friendly JSON (events + totals) returned to the front-end. They also consult `Device` to locate the active device_id for the current team.

## Livewire Integration

Livewire components are used extensively throughout the application for real-time, reactive user interfaces. Here's how Livewire fits into the system:

1. **Component Location**: All Livewire components are stored in `app/Livewire/` directory, organized by module (Pith, Fibre, etc.)
2. **Route Integration**: Components are registered in `routes/web.php` using the `ClassName::class` syntax
3. **View Rendering**: Each component has an associated Blade view in `resources/views/livewire/`
4. **State Management**: Components maintain their state between requests, handling user interactions without full page reloads
5. **Model Interaction**: Components directly interact with Eloquent models for data operations

## Typical Request Flow

Here's a sequence diagram showing a typical user action flow:

```mermaid
sequenceDiagram
    participant U as User
    participant L as Livewire Component
    participant C as Controller
    participant S as Service
    participant M as Model
    participant D as Database
    
    U->>L: Triggers action
    L->>C: Handles request
    C->>S: Process data
    S->>M: Validates input
    M->>D: Query/Update data
    D-->>M: Return results
    M-->>S: Format data
    S-->>C: Process response
    C-->>L: Return data
    L-->>U: Update UI
    
    Note over L,U: Real-time updates via Livewire events
    Note over S,D: Service layer handles business logic
```

This flow demonstrates how:
1. User interactions are captured by Livewire components
2. Data validation and processing occur in the backend
3. Real-time updates are pushed to the UI
4. The system maintains consistency across all layers
