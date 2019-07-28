# Patients

## Overview

- [Introduction](#introduction)
- [Fields](#fields)
- [Sync from Metaclinic](#sync-from-metaclinic)
- [API Reference](#api-reference)


## Introduction

The Metaclinic LIMS provides the main data store for patient data. However,
we need that patient data in an easily queryable environment for the landing
application to match, search, and use patient data with incoming cases.

Patients are synced with the Metaclinic LIMS via an automated
[#agenda job](../../agenda-jobs/README.md).

## Fields

The patient services exposes these fields through the API.

| Field               | Type          | Description                                               |
|---------------------|---------------|-----------------------------------------------------------|
| address_city        | String        | City address                                              |
| address_street1     | String        | Street address                                            |
| address_street2     | String        | Street address                                            |
| address_state       | String        | State, typically the 2-charcter abbreviation              |
| address_zip         | String        | Zip code for the address.                                 |
| clinical_mrn        | String        | Patient's clincial medical record number. Typically generated from the client's EHR.                                                                       |
| country_code        | String        | Country code (most commonly "US")                         |
| date_of_birth       | ISODate       | Patient's date of birth. Returned as an ISODate string    |
| first_name          | String        | Patient's first name.                                     |
| gender              | String(1)     | Patient's gender. Must be "M" for male or "F" for female  |
| last_name           | String        | Patient's last name.                                      |
| middle_name         | String        | Patient's middle name (or more commonly iniital)          |
| mrn                 | String        | Patient's LIS medical record number. This is a unique identifier the LIS uses to identify returning `lab` patients. Value is `null` for `case` patients.   |
| patient_lis_type    | String        | Patients maybe a "lab" patient or a "case" patient        |
| referring_mrn       | String        | Patient's MRN number in the Lean Lab instance of the LIS                                                                                                   |
| ssn                 | String        | Social Security number                                    |

## Sync from Metaclinic

Patient data flows in one direction--from Metaclinic to the Apollo app. An automated
[agenda job](../../agenda/README.md) is responsible for accessing the Metaclinc
API and retreiving the latest patient data.


## API Reference

### Find a patient
- [GET/ patients/](./get/GET-patients.md)
- [GET/ patients/like](./get/GET-patients-like.md)


