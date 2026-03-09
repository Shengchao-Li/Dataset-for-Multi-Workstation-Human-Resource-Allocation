# Benchmark Instances for Multi-Workstation Human Resource Allocation

## 1. Overview
This repository contains the benchmark dataset for the paper: 
**"A Pattern-Based Decomposition Algorithm for Optimal Multi-Workstation Human Resource Allocation under Spatial-Temporal Constraints"**.

These instances are designed to evaluate scheduling algorithms for the Human Resource Allocation Problem with Spatial-Temporal Constraints (HRAP-SC). The dataset features complex practical constraints commonly found in parallel assembly processes (e.g., satellite and aircraft manufacturing), including:
* Multi-workstation spatial separation
* Heterogeneous multi-worker collaboration
* Rigid precedence relations
* Technological tail times (e.g., curing or drying)
* Calendar constraints (e.g., 8-hour workdays)

## 2. File Naming Convention
The JSON files are named following the format:
`data_job[J]_worker[I]_site[S]_[C].json`
* `[J]`: Total number of jobs (`jobNum`)
* `[I]`: Total number of available workers (`workerNum`)
* `[S]`: Total number of parallel workstations/sites (`siteNum`)
* `[C]`: Instance case ID (`caseNo`)

*Example:* `data_job1_worker2_site1_1.json` represents an instance with 1 job, 2 workers, 1 workstation, and is the 1st test case of this scale.

## 3. Data Structure Dictionary
Each `.json` file contains a single object with the global parameters and a list of operations. Below is the detailed explanation of each JSON key:

### Global Parameters
* `jobNum` *(int)*: The total number of jobs to be scheduled.
* `workerNum` *(int)*: The total number of available multi-skilled workers in the shared pool.
* `siteNum` *(int)*: The number of available parallel workstations.
* `caseNo` *(int)*: The specific ID number of the instance within its scale category.
* `spanPerPeriod` *(float)*: The total length of a calendar cycle (e.g., `24.0` hours represents a full day).
* `workingStartTime` *(float)*: The specific time point when a working shift begins within a period (e.g., `9.0` means the shift starts at 09:00 AM).
* `workingHours` *(float)*: The duration of the effective working shift per period (e.g., `8.0` means an 8-hour workday).

### `operation` (Array of Objects)
This array lists all the tasks/operations involved in the assembly process. Each object contains:
* `operationIndex` *(int)*: The unique ID of the operation.
* `needWorkerNum` *(int)*: The exact number of workers required simultaneously to execute this operation (team collaboration constraint).
* `workerTimes` *(Array of floats)*: The baseline processing times if each worker (indexed 1 to `workerNum`) were to perform the task individually. *(Note: The actual duration is determined by `workerGroups`)*.
* `curingTime` *(float)*: The mandatory technological tail time that must elapse after the operation finishes. No workers are required during this time, but successor operations cannot start until it concludes.
* `immediateOprts` *(Array of ints)*: The precedence constraints. Lists the `operationIndex` of all immediate predecessors that must be completed (including their `curingTime`) before this operation can start.
* `workerGroups` *(Array of Objects)*: The set of all feasible worker combinations that are qualified to perform this operation.
    * `groupIndex` *(int)*: The ID of the worker group.
    * `workers` *(Array of ints)*: The specific worker IDs assigned to this group (the length of this array equals `needWorkerNum`).
    * `time` *(float)*: The actual processing duration of the operation if it is assigned to this specific group of workers (reflecting heterogeneous workforce efficiency).

> **Important Note on Data Types**: Although several parameters (such as `spanPerPeriod`) are formatted as floating-point numbers (e.g., `24.0`) in the JSON files, all numerical values in our specific problem formulation and experiments are strictly treated as integers. You may safely cast or interpret **all float values** as integer types when using this dataset.

## 4. Citation
> **Note**: A related journal article is currently in preparation. For the latest citation details, please refer to the Zenodo record.

## 5. License
This dataset is licensed under the **Creative Commons Attribution 4.0 International License (CC BY 4.0)**. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/. You are free to use, modify, and distribute the data for academic and research purposes, provided that proper attribution is given.
