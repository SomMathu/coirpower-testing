# Smart Coir Technical Architecture Documentation

## Executive Summary

Smart Coir is a modern web application designed to streamline coir production management. The system combines traditional web interfaces with real-time IoT device monitoring to provide comprehensive production tracking and insights.

### Key Features
- Real-time production monitoring
- IoT device integration
- Automated reporting and analytics
- Mobile app support
- Multi-user collaboration

## System Overview

Smart Coir is built with Laravel, utilizing:
- Modern web technologies for responsive user interfaces
- Real-time data processing for IoT device integration
- Secure API endpoints for mobile access
- Background processing for automated tasks

## Core Architecture Components

The system is organized into interconnected modules that handle different aspects of coir production management:

### Contract (inputs / outputs)
- Inputs: HTTP requests (web routes and API endpoints), authenticated user, team context
- Outputs: HTML (Blade + Livewire), JSON (API), emails and scheduled jobs
- Error modes: auth/authorization failures, validation errors, third-party API or cron failures

### Key components
- Routes: `routes/web.php`, `routes/api.php` — define public and authenticated endpoints
- Controllers: `app/Http/Controllers/...` — request handling and orchestration
- Livewire components: `app/Http/Livewire` and used directly from routes (single-action components)
- Models: `app/Models` — Eloquent models representing domain objects and device data
- Views: `resources/views/...` — Blade templates and Livewire partials

## Detailed Architectural Diagram
```mermaid
flowchart TD
    U1["User via Web Browser"]
    U2["User via Mobile App"]

    W["Web UI (Blade / Livewire)"]
    M["Mobile App (API Consumer)"]

    L["Laravel Backend"]
    DB["MySQL Database"]
    JOB["Background Jobs & Scheduler"]
    REDIS["Redis Cache"]

    IOT["IoT Devices"]
    WEATHER["Weather APIs"]
    NOTIFY["Firebase / Email Notifications"]

    U1 -->|"HTTP Request"| W
    W -->|"Route/Controller Call"| L
    L -->|"Query / Store Data"| DB
    L -->|"Trigger Jobs"| JOB
    L -->|"Read/Write Cache"| REDIS
    L -->|"Send HTML / JSON Response"| W

    U2 -->|"API Request (JSON)"| M
    M -->|"REST API Call"| L
    L -->|"DB Query"| DB
    L -->|"Send JSON Response"| M

    IOT -->|"Device Data"| L
    WEATHER -->|"Scheduled API Call"| L
    L -->|"Send Alerts / Updates"| NOTIFY
    JOB -->|"Process / Aggregate Data"| DB

    style W fill:#e8f3ff,stroke:#333,stroke-width:1px
    style M fill:#e8f3ff,stroke:#333,stroke-width:1px
    style L fill:#fff3e0,stroke:#333,stroke-width:1px
    style DB fill:#e8ffe8,stroke:#333,stroke-width:1px
    style JOB fill:#ffe6e6,stroke:#333,stroke-width:1px
    style REDIS fill:#ffe6e6,stroke:#333,stroke-width:1px
    style IOT fill:#f0f7ff,stroke:#333,stroke-width:1px
    style WEATHER fill:#f0f7ff,stroke:#333,stroke-width:1px
    style NOTIFY fill:#f0f7ff,stroke:#333,stroke-width:1px
```
## Data Processing and Integration

### Data Processing Pipeline
```mermaid
flowchart LR
    subgraph Input[Data Sources]
        IoT[IoT Devices]
        Manual[Manual Entry]
        API[External APIs]
    end

    subgraph Processing[Data Processing]
        Jobs[Background Jobs]
        Services[Service Layer]
        Cache[Cache Layer]
    end

    subgraph Storage[Data Storage]
        DB[(Database)]
        Redis[(Redis Cache)]
    end

    subgraph Output[Data Consumption]
        Web[Web Interface]
        Mobile[Mobile App]
        Reports[Reports]
    end

    Input --> Processing
    Processing --> Storage
    Storage --> Output

    style Input fill:#e6f3ff,stroke:#333
    style Processing fill:#fff3e6,stroke:#333
    style Storage fill:#e6ffe6,stroke:#333
    style Output fill:#ffe6e6,stroke:#333
```

### System Integration Points
- **IoT Device Integration**: Real-time data collection via MQTT/HTTP protocols
- **Mobile App Integration**: RESTful API endpoints with JWT authentication
- **External Services**: Weather data integration, Firebase notifications
- **Background Processing**: Laravel jobs and scheduled tasks
- **Caching Strategy**: Cloudflare

## Technical Reference

### Directory Structure
```
app/
├── Http/
│   ├── Controllers/    # Request handlers
│   └── Livewire/       # Real-time UI components
├── Models/             # Database models
├── Services/           # Business logic
└── Jobs/               # Background tasks

resources/
└── views/             # UI templates

routes/
├── web.php            # Web routes
└── api.php            # API endpoints
```

### Key Technologies
- **Backend**: PHP Laravel (Controllers, Livewire, Scheduler, Jobs)
- **Frontend**: Blade + Livewire
- **Database**: MySQL
- **Messaging**: Firebase Cloud Messaging, Email Notifications
- **Integrations**: Weather APIs (tomorrow.io), IoT devices
- **Deployment**: AWS (EC2), Laravel Scheduler

### Development Guidelines
1. Follow Laravel best practices
2. Use type hints and docblocks
3. Write unit tests for critical paths
4. Follow PSR-12 coding standards
5. Document API endpoints

---

Last updated: 2025-09-26





