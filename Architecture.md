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
%%{init: {'themeVariables': {'fontFamily': 'sans-serif'}}}%%
flowchart TD
    subgraph DataSources[Data Sources]
        IoT[IoT Devices]
        Manual[Manual Entry]
        API[External APIs (tomorrow.io)]
    end

    subgraph Backend[Backend Processing]
        Laravel(PHP Laravel):::php
        Controller[Controllers]
        Service[Service Layer]
        Job[Background Jobs (Laravel Scheduler)]
        Cache[Cache Layer]
    end

    subgraph Storage[Storage]
        MySQL(MySQL Database):::mysql
        Redis(Redis Cache):::redis
    end

    subgraph Frontend[Frontend & Output]
        Web[Web Interface (Blade/Livewire)]
        Mobile[Mobile App (Firebase Notifications)]
        Report[Reports/Exports]
        Notify[Notifications (Email/Firebase)]
    end

    subgraph Deployment[Deployment]
        AWS(AWS EC2):::aws
    end

    IoT --> Controller
    Manual --> Controller
    API --> Controller

    Controller --> Service
    Service --> Job
    Service --> Cache
    Job --> MySQL
    Service --> MySQL
    Cache --> MySQL

    MySQL --> Web
    MySQL --> Mobile
    MySQL --> Report
    MySQL --> Notify
    Redis --> Web
    Redis --> Mobile

    Web --> AWS
    Mobile --> AWS
    API --> AWS

    style DataSources fill:#e6f3ff,stroke:#333
    style Backend fill:#fff3e6,stroke:#333
    style Storage fill:#e6ffe6,stroke:#333
    style Frontend fill:#ffe6e6,stroke:#333
    style Deployment fill:#f0e6ff,stroke:#333

    classDef php fill:#777BB4,stroke:#333,color:#fff;
    classDef mysql fill:#00758F,stroke:#333,color:#fff;
    classDef redis fill:#DC382D,stroke:#333,color:#fff;
    classDef aws fill:#FF9900,stroke:#333,color:#fff;
    classDef firebase fill:#FFCA28,stroke:#333,color:#000;
    classDef livewire fill:#424F79,stroke:#333,color:#fff;

    linkStyle 0 stroke-width:0px;
    linkStyle 1 stroke-width:0px;
    linkStyle 2 stroke-width:0px;
    linkStyle 3 stroke-width:0px;
    linkStyle 4 stroke-width:0px;
    linkStyle 5 stroke-width:0px;
    linkStyle 6 stroke-width:0px;
    linkStyle 7 stroke-width:0px;
    linkStyle 8 stroke-width:0px;
    linkStyle 9 stroke-width:0px;
    linkStyle 10 stroke-width:0px;
    linkStyle 11 stroke-width:0px;
    linkStyle 12 stroke-width:0px;
    linkStyle 13 stroke-width:0px;
    linkStyle 14 stroke-width:0px;
    linkStyle 15 stroke-width:0px;
    linkStyle 16 stroke-width:0px;
    linkStyle 17 stroke-width:0px;
    linkStyle 18 stroke-width:0px;
    linkStyle 19 stroke-width:0px;
    linkStyle 20 stroke-width:0px;
    linkStyle 21 stroke-width:0px;
    linkStyle 22 stroke-width:0px;
    linkStyle 23 stroke-width:0px;
    linkStyle 24 stroke-width:0px;
    linkStyle 25 stroke-width:0px;
    linkStyle 26 stroke-width:0px;
    linkStyle 27 stroke-width:0px;
    linkStyle 28 stroke-width:0px;
    linkStyle 29 stroke-width:0px;
    linkStyle 30 stroke-width:0px;
    linkStyle 31 stroke-width:0px;
    linkStyle 32 stroke-width:0px;
    linkStyle 33 stroke-width:0px;
    linkStyle 34 stroke-width:0px;
    linkStyle 35 stroke-width:0px;
    linkStyle 36 stroke-width:0px;
    linkStyle 37 stroke-width:0px;
    linkStyle 38 stroke-width:0px;
    linkStyle 39 stroke-width:0px;
    linkStyle 40 stroke-width:0px;
    linkStyle 41 stroke-width:0px;
    linkStyle 42 stroke-width:0px;
    linkStyle 43 stroke-width:0px;
    linkStyle 44 stroke-width:0px;
    linkStyle 45 stroke-width:0px;
    linkStyle 46 stroke-width:0px;
    linkStyle 47 stroke-width:0px;
    linkStyle 48 stroke-width:0px;
    linkStyle 49 stroke-width:0px;
    linkStyle 50 stroke-width:0px;
    linkStyle 51 stroke-width:0px;
    linkStyle 52 stroke-width:0px;
    linkStyle 53 stroke-width:0px;
    linkStyle 54 stroke-width:0px;
    linkStyle 55 stroke-width:0px;
    linkStyle 56 stroke-width:0px;
    linkStyle 57 stroke-width:0px;
    linkStyle 58 stroke-width:0px;
    linkStyle 59 stroke-width:0px;
    linkStyle 60 stroke-width:0px;
    linkStyle 61 stroke-width:0px;
    linkStyle 62 stroke-width:0px;
    linkStyle 63 stroke-width:0px;
    linkStyle 64 stroke-width:0px;
    linkStyle 65 stroke-width:0px;
    linkStyle 66 stroke-width:0px;
    linkStyle 67 stroke-width:0px;
    linkStyle 68 stroke-width:0px;
    linkStyle 69 stroke-width:0px;
    linkStyle 70 stroke-width:0px;
    linkStyle 71 stroke-width:0px;
    linkStyle 72 stroke-width:0px;
    linkStyle 73 stroke-width:0px;
    linkStyle 74 stroke-width:0px;
    linkStyle 75 stroke-width:0px;
    linkStyle 76 stroke-width:0px;
    linkStyle 77 stroke-width:0px;
    linkStyle 78 stroke-width:0px;
    linkStyle 79 stroke-width:0px;
    linkStyle 80 stroke-width:0px;
    linkStyle 81 stroke-width:0px;
    linkStyle 82 stroke-width:0px;
    linkStyle 83 stroke-width:0px;
    linkStyle 84 stroke-width:0px;
    linkStyle 85 stroke-width:0px;
    linkStyle 86 stroke-width:0px;
    linkStyle 87 stroke-width:0px;
    linkStyle 88 stroke-width:0px;
    linkStyle 89 stroke-width:0px;
    linkStyle 90 stroke-width:0px;
    linkStyle 91 stroke-width:0px;
    linkStyle 92 stroke-width:0px;
    linkStyle 93 stroke-width:0px;
    linkStyle 94 stroke-width:0px;
    linkStyle 95 stroke-width:0px;
    linkStyle 96 stroke-width:0px;
    linkStyle 97 stroke-width:0px;
    linkStyle 98 stroke-width:0px;
    linkStyle 99 stroke-width:0px;
    linkStyle 100 stroke-width:0px;
    linkStyle 101 stroke-width:0px;
    linkStyle 102 stroke-width:0px;
    linkStyle 103 stroke-width:0px;
    linkStyle 104 stroke-width:0px;
    linkStyle 105 stroke-width:0px;
    linkStyle 106 stroke-width:0px;
    linkStyle 107 stroke-width:0px;
    linkStyle 108 stroke-width:0px;
    linkStyle 109 stroke-width:0px;
    linkStyle 110 stroke-width:0px;
    linkStyle 111 stroke-width:0px;
    linkStyle 112 stroke-width:0px;
    linkStyle 113 stroke-width:0px;
    linkStyle 114 stroke-width:0px;
    linkStyle 115 stroke-width:0px;
    linkStyle 116 stroke-width:0px;
    linkStyle 117 stroke-width:0px;
    linkStyle 118 stroke-width:0px;
    linkStyle 119 stroke-width:0px;
    linkStyle 120 stroke-width:0px;
    linkStyle 121 stroke-width:0px;
    linkStyle 122 stroke-width:0px;
    linkStyle 123 stroke-width:0px;
    linkStyle 124 stroke-width:0px;
    linkStyle 125 stroke-width:0px;
    linkStyle 126 stroke-width:0px;
    linkStyle 127 stroke-width:0px;
    linkStyle 128 stroke-width:0px;
    linkStyle 129 stroke-width:0px;
    linkStyle 130 stroke-width:0px;
    linkStyle 131 stroke-width:0px;
    linkStyle 132 stroke-width:0px;
    linkStyle 133 stroke-width:0px;
    linkStyle 134 stroke-width:0px;
    linkStyle 135 stroke-width:0px;
    linkStyle 136 stroke-width:0px;
    linkStyle 137 stroke-width:0px;
    linkStyle 138 stroke-width:0px;
    linkStyle 139 stroke-width:0px;
    linkStyle 140 stroke-width:0px;
    linkStyle 141 stroke-width:0px;
    linkStyle 142 stroke-width:0px;
    linkStyle 143 stroke-width:0px;
    linkStyle 144 stroke-width:0px;
    linkStyle 145 stroke-width:0px;
    linkStyle 146 stroke-width:0px;
    linkStyle 147 stroke-width:0px;
    linkStyle 148 stroke-width:0px;
    linkStyle 149 stroke-width:0px;
    linkStyle 150 stroke-width:0px;
    linkStyle 151 stroke-width:0px;
    linkStyle 152 stroke-width:0px;
    linkStyle 153 stroke-width:0px;
    linkStyle 154 stroke-width:0px;
    linkStyle 155 stroke-width:0px;
    linkStyle 156 stroke-width:0px;
    linkStyle 157 stroke-width:0px;
    linkStyle 158 stroke-width:0px;
    linkStyle 159 stroke-width:0px;
    linkStyle 160 stroke-width:0px;
    linkStyle 161 stroke-width:0px;
    linkStyle 162 stroke-width:0px;
    linkStyle 163 stroke-width:0px;
    linkStyle 164 stroke-width:0px;
    linkStyle 165 stroke-width:0px;
    linkStyle 166 stroke-width:0px;
    linkStyle 167 stroke-width:0px;
    linkStyle 168 stroke-width:0px;
    linkStyle 169 stroke-width:0px;
    linkStyle 170 stroke-width:0px;
    linkStyle 171 stroke-width:0px;
    linkStyle 172 stroke-width:0px;
    linkStyle 173 stroke-width:0px;
    linkStyle 174 stroke-width:0px;
    linkStyle 175 stroke-width:0px;
    linkStyle 176 stroke-width:0px;
    linkStyle 177 stroke-width:0px;
    linkStyle 178 stroke-width:0px;
    linkStyle 179 stroke-width:0px;
    linkStyle 180 stroke-width:0px;
    linkStyle 181 stroke-width:0px;
    linkStyle 182 stroke-width:0px;
    linkStyle 183 stroke-width:0px;
    linkStyle 184 stroke-width:0px;
    linkStyle 185 stroke-width:0px;
    linkStyle 186 stroke-width:0px;
    linkStyle 187 stroke-width:0px;
    linkStyle 188 stroke-width:0px;
    linkStyle 189 stroke-width:0px;
    linkStyle 190 stroke-width:0px;
    linkStyle 191 stroke-width:0px;
    linkStyle 192 stroke-width:0px;
    linkStyle 193 stroke-width:0px;
    linkStyle 194 stroke-width:0px;
    linkStyle 195 stroke-width:0px;
    linkStyle 196 stroke-width:0px;
    linkStyle 197 stroke-width:0px;
    linkStyle 198 stroke-width:0px;
    linkStyle 199 stroke-width:0px;
    linkStyle 200 stroke-width:0px;
    linkStyle 201 stroke-width:0px;
    linkStyle 202 stroke-width:0px;
    linkStyle 203 stroke-width:0px;
    linkStyle 204 stroke-width:0px;
    linkStyle 205 stroke-width:0px;
    linkStyle 206 stroke-width:0px;
    linkStyle 207 stroke-width:0px;
    linkStyle 208 stroke-width:0px;
    linkStyle 209 stroke-width:0px;
    linkStyle 210 stroke-width:0px;
    linkStyle 211 stroke-width:0px;
    linkStyle 212 stroke-width:0px;
    linkStyle 213 stroke-width:0px;
    linkStyle 214 stroke-width:0px;
    linkStyle 215 stroke-width:0px;
    linkStyle 216 stroke-width:0px;
    linkStyle 217 stroke-width:0px;
    linkStyle 218 stroke-width:0px;
    linkStyle 219 stroke-width:0px;
    linkStyle 220 stroke-width:0px;
    linkStyle 221 stroke-width:0px;
    linkStyle 222 stroke-width:0px;
    linkStyle 223 stroke-width:0px;
    linkStyle 224 stroke-width:0px;
    linkStyle 225 stroke-width:0px;
    linkStyle 226 stroke-width:0px;
    linkStyle 227 stroke-width:0px;
    linkStyle 228 stroke-width:0px;
    linkStyle 229 stroke-width:0px;
    linkStyle 230 stroke-width:0px;
    linkStyle 231 stroke-width:0px;
    linkStyle 232 stroke-width:0px;
    linkStyle 233 stroke-width:0px;
    linkStyle 234 stroke-width:0px;
    linkStyle 235 stroke-width:0px;
    linkStyle 236 stroke-width:0px;
    linkStyle 237 stroke-width:0px;
    linkStyle 238 stroke-width:0px;
    linkStyle 239 stroke-width:0px;
    linkStyle 240 stroke-width:0px;
    linkStyle 241 stroke-width:0px;
    linkStyle 242 stroke-width:0px;
    linkStyle 243 stroke-width:0px;
    linkStyle 244 stroke-width:0px;
    linkStyle 245 stroke-width:0px;
    linkStyle 246 stroke-width:0px;
    linkStyle 247 stroke-width:0px;
    linkStyle 248 stroke-width:0px;
    linkStyle 249 stroke-width:0px;
    linkStyle 250 stroke-width:0px;
    linkStyle 251 stroke-width:0px;
    linkStyle 252 stroke-width:0px;
    linkStyle 253 stroke-width:0px;
    linkStyle 254 stroke-width:0px;
    linkStyle 255 stroke-width:0px;
    linkStyle 256 stroke-width:0px;
    linkStyle 257 stroke-width:0px;
    linkStyle 258 stroke-width:0px;
    linkStyle 259 stroke-width:0px;
    linkStyle 260 stroke-width:0px;
    linkStyle 261 stroke-width:0px;
    linkStyle 262 stroke-width:0px;
    linkStyle 263 stroke-width:0px;
    linkStyle 264 stroke-width:0px;
    linkStyle 265 stroke-width:0px;
    linkStyle 266 stroke-width:0px;
    linkStyle 267 stroke-width:0px;
    linkStyle 268 stroke-width:0px;
    linkStyle 269 stroke-width:0px;
    linkStyle 270 stroke-width:0px;
    linkStyle 271 stroke-width:0px;
    linkStyle 272 stroke-width:0px;
    linkStyle 273 stroke-width:0px;
    linkStyle 274 stroke-width:0px;
    linkStyle 275 stroke-width:0px;
    linkStyle 276 stroke-width:0px;
    linkStyle 277 stroke-width:0px;
    linkStyle 278 stroke-width:0px;
    linkStyle 279 stroke-width:0px;
    linkStyle 280 stroke-width:0px;
    linkStyle 281 stroke-width:0px;
    linkStyle 282 stroke-width:0px;
    linkStyle 283 stroke-width:0px;
    linkStyle 284 stroke-width:0px;
    linkStyle 285 stroke-width:0px;
    linkStyle 286 stroke-width:0px;
    linkStyle 287 stroke-width:0px;
    linkStyle 288 stroke-width:0px;
    linkStyle 289 stroke-width:0px;
    linkStyle 290 stroke-width:0px;
    linkStyle 291 stroke-width:0px;
    linkStyle 292 stroke-width:0px;
    linkStyle 293 stroke-width:0px;
    linkStyle 294 stroke-width:0px;
    linkStyle 295 stroke-width:0px;
    linkStyle 296 stroke-width:0px;
    linkStyle 297 stroke-width:0px;
    linkStyle 298 stroke-width:0px;
    linkStyle 299 stroke-width:0px;
    linkStyle 300 stroke-width:0px;
    linkStyle 301 stroke-width:0px;
    linkStyle 302 stroke-width:0px;
    linkStyle 303 stroke-width:0px;
    linkStyle 304 stroke-width:0px;
    linkStyle 305 stroke-width:0px;
    linkStyle 306 stroke-width:0px;
    linkStyle 307 stroke-width:0px;
    linkStyle 308 stroke-width:0px;
    linkStyle 309 stroke-width:0px;
    linkStyle 310 stroke-width:0px;
    linkStyle 311 stroke-width:0px;
    linkStyle 312 stroke-width:0px;
    linkStyle 313 stroke-width:0px;
    linkStyle 314 stroke-width:0px;
    linkStyle 315 stroke-width:0px;
    linkStyle 316 stroke-width:0px;
    linkStyle 317 stroke-width:0px;
    linkStyle 318 stroke-width:0px;
    linkStyle 319 stroke-width:0px;
    linkStyle 320 stroke-width:0px;
    linkStyle 321 stroke-width:0px;
    linkStyle 322 stroke-width:0px;
    linkStyle 323 stroke-width:0px;
    linkStyle 324 stroke-width:0px;
    linkStyle 325 stroke-width:0px;
    linkStyle 326 stroke-width:0px;
    linkStyle 327 stroke-width:0px;
    linkStyle 328 stroke-width:0px;
    linkStyle 329 stroke-width:0px;
    linkStyle 330 stroke-width:0px;
    linkStyle 331 stroke-width:0px;
    linkStyle 332 stroke-width:0px;
    linkStyle 333 stroke-width:0px;
    linkStyle 334 stroke-width:0px;
    linkStyle 335 stroke-width:0px;
    linkStyle 336 stroke-width:0px;
    linkStyle 337 stroke-width:0px;
    linkStyle 338 stroke-width:0px;
    linkStyle 339 stroke-width:0px;
    linkStyle 340 stroke-width:0px;
    linkStyle 341 stroke-width:0px;
    linkStyle 342 stroke-width:0px;
    linkStyle 343 stroke-width:0px;
    linkStyle 344 stroke-width:0px;
    linkStyle 345 stroke-width:0px;
    linkStyle 346 stroke-width:0px;
    linkStyle 347 stroke-width:0px;
    linkStyle 348 stroke-width:0px;
    linkStyle 349 stroke-width:0px;
    linkStyle 350 stroke-width:0px;
    linkStyle 351 stroke-width:0px;
    linkStyle 352 stroke-width:0px;
    linkStyle 353 stroke-width:0px;
    linkStyle 354 stroke-width:0px;
    linkStyle 355 stroke-width:0px;
    linkStyle 356 stroke-width:0px;
    linkStyle 357 stroke-width:0px;
    linkStyle 358 stroke-width:0px;
    linkStyle 359 stroke-width:0px;
    linkStyle 360 stroke-width:0px;
    linkStyle 361 stroke-width:0px;
    linkStyle 362 stroke-width:0px;
    linkStyle 363 stroke-width:0px;
    linkStyle 364 stroke-width:0px;
    linkStyle 365 stroke-width:0px;
    linkStyle 366 stroke-width:0px;
    linkStyle 367 stroke-width:0px;
    linkStyle 368 stroke-width:0px;
    linkStyle 369 stroke-width:0px;
    linkStyle 370 stroke-width:0px;
    linkStyle 371 stroke-width:0px;
    linkStyle 372 stroke-width:0px;
    linkStyle 373 stroke-width:0px;
    linkStyle 374 stroke-width:0px;
    linkStyle 375 stroke-width:0px;
    linkStyle 376 stroke-width:0px;
    linkStyle 377 stroke-width:0px;
    linkStyle 378 stroke-width:0px;
    linkStyle 379 stroke-width:0px;
    linkStyle 380 stroke-width:0px;
    linkStyle 381 stroke-width:0px;
    linkStyle 382 stroke-width:0px;
    linkStyle 383 stroke-width:0px;
    linkStyle 384 stroke-width:0px;
    linkStyle 385 stroke-width:0px;
    linkStyle 386 stroke-width:0px;
    linkStyle 387 stroke-width:0px;
    linkStyle 388 stroke-width:0px;
    linkStyle 389 stroke-width:0px;
    linkStyle 390 stroke-width:0px;
    linkStyle 391 stroke-width:0px;
    linkStyle 392 stroke-width:0px;
    linkStyle 393 stroke-width:0px;
    linkStyle 394 stroke-width:0px;
    linkStyle 395 stroke-width:0px;
    linkStyle 396 stroke-width:0px;
    linkStyle 397 stroke-width:0px;
    linkStyle 398 stroke-width:0px;
    linkStyle 399 stroke-width:0px;
    linkStyle 400 stroke-width:0px;
    linkStyle 401 stroke-width:0px;
    linkStyle 402 stroke-width:0px;
    linkStyle 403 stroke-width:0px;
    linkStyle 404 stroke-width:0px;
    linkStyle 405 stroke-width:0px;
    linkStyle 406 stroke-width:0px;
    linkStyle 407 stroke-width:0px;
    linkStyle 408 stroke-width:0px;
    linkStyle 409 stroke-width:0px;
    linkStyle 410 stroke-width:0px;
    linkStyle 411 stroke-width:0px;
    linkStyle 412 stroke-width:0px;
    linkStyle 413 stroke-width:0px;
    linkStyle 414 stroke-width:0px;
    linkStyle 415 stroke-width:0px;
    linkStyle 416 stroke-width:0px;
    linkStyle 417 stroke-width:0px;
    linkStyle 418 stroke-width:0px;
    linkStyle 419 stroke-width:0px;
    linkStyle 420 stroke-width:0px;
    linkStyle 421 stroke-width:0px;
    linkStyle 422 stroke-width:0px;
    linkStyle 423 stroke-width:0px;
    linkStyle 424 stroke-width:0px;
    linkStyle 425 stroke-width:0px;
    linkStyle 426 stroke-width:0px;
    linkStyle 427 stroke-width:0px;
    linkStyle 428 stroke-width:0px;
    linkStyle 429 stroke-width:0px;
    linkStyle 430 stroke-width:0px;
    linkStyle 431 stroke-width:0px;
    linkStyle 432 stroke-width:0px;
    linkStyle 433 stroke-width:0px;
    linkStyle 434 stroke-width:0px;
    linkStyle 435 stroke-width:0px;
    linkStyle 436 stroke-width:0px;
    linkStyle 437 stroke-width:0px;
    linkStyle 438 stroke-width:0px;
    linkStyle 439 stroke-width:0px;
    linkStyle 440 stroke-width:0px;
    linkStyle 441 stroke-width:0px;
    linkStyle 442 stroke-width:0px;
    linkStyle 443 stroke-width:0px;
    linkStyle 444 stroke-width:0px;
    linkStyle 445 stroke-width:0px;
    linkStyle 446 stroke-width:0px;
    linkStyle 447 stroke-width:0px;
    linkStyle 448 stroke-width:0px;
    linkStyle 449 stroke-width:0px;
    linkStyle 450 stroke-width:0px;
    linkStyle 451 stroke-width:0px;
    linkStyle 452 stroke-width:0px;
    linkStyle 453 stroke-width:0px;
    linkStyle 454 stroke-width:0px;
    linkStyle 455 stroke-width:0px;
    linkStyle 456 stroke-width:0px;
    linkStyle 457 stroke-width:0px;
    linkStyle 458 stroke-width:0px;
    linkStyle 459 stroke-width:0px;
    linkStyle 460 stroke-width:0px;
    linkStyle 461 stroke-width:0px;
    linkStyle 462 stroke-width:0px;
    linkStyle 463 stroke-width:0px;
    linkStyle 464 stroke-width:0px;
    linkStyle 465 stroke-width:0px;
    linkStyle 466 stroke-width:0px;
    linkStyle 467 stroke-width:0px;
    linkStyle 468 stroke-width:0px;
    linkStyle 469 stroke-width:0

## Data Processing and Integration

### Data Processing Pipeline
```mermaid
%%{init: {'themeVariables': {'fontFamily': 'sans-serif'}}}%%
flowchart LR
    subgraph Input[Data Sources]
        IoT[IoT Devices]
        Manual[Manual Entry]
        API[External APIs (tomorrow.io)]
    end

    subgraph Processing[Data Processing]
        Laravel(PHP Laravel):::php
        Jobs[Background Jobs (Laravel Scheduler)]
        Services[Service Layer]
        Cache[Cache Layer]
    end

    subgraph Storage[Data Storage]
        MySQL(MySQL Database):::mysql
        Redis(Redis Cache):::redis
    end

    subgraph Output[Data Consumption]
        Web[Web Interface (Blade/Livewire)]
        Mobile[Mobile App (Firebase Notifications)]
        Reports[Reports/Exports]
    end

    Input --> Processing
    Processing --> Storage
    Storage --> Output

    style Input fill:#e6f3ff,stroke:#333
    style Processing fill:#fff3e6,stroke:#333
    style Storage fill:#e6ffe6,stroke:#333
    style Output fill:#ffe6e6,stroke:#333

    classDef php fill:#777BB4,stroke:#333,color:#fff;
    classDef mysql fill:#00758F,stroke:#333,color:#fff;
    classDef redis fill:#DC382D,stroke:#333,color:#fff;
    classDef aws fill:#FF9900,stroke:#333,color:#fff;
    classDef firebase fill:#FFCA28,stroke:#333,color:#000;
    classDef livewire fill:#424F79,stroke:#333,color:#fff;
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

