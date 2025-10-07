# IoT Data Handling Documentation

## Overview

This document serves as a central hub for understanding how IoT data is collected, processed, and managed within the Smart Coir system. The system is divided into several modules, each with its own specific data handling processes. For detailed information, please refer to the module-specific documentation linked below.

## Module-Specific Documentation

-   [**Pith Module IoT Data Handling](./iot/pith.md)**
    -   Covers `device_data_pith_blocks` table schema.
    -   Details on duplicate handling via `ProcessDeviceDataForModBlocksJob`.
    -   Information on schedulers and summary table population for pith production.

-   [**Fibre Module IoT Data Handling](./iot/fibre.md)**
    -   Covers `device_data_fibre_bales` and `device_data_fibre_mega_bales_weight` table schemas.
    -   Details on duplicate handling via `ProcessDeviceDataForModBalesJob`.
    -   Information on schedulers and summary table population for fibre production.

-   [**General IoT Data Handling](./iot/general.md)**
    -   Covers `summary_power_states` and `device_data_water_meters` table schemas.
    -   Details on duplicate handling for water meters.
    -   Information on the `ProcessDeviceDataForStates` and `ProcessDeviceDataForModWaterJob` jobs.

---

Last updated: 2026-09-26