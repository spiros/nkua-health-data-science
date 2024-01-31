# Data dictionary

## List of files

| File | Description |
|------|-------------|
| [`conditions.csv.gz`](#conditions) | Patient conditions or diagnoses. |
| [`encounters.csv.gz`](#encounters) | Patient encounter data. |
| [`medications.csv.gz`](#medications) | Patient medication data. |
| [`observations.csv.gz`](#observations) | Patient observations including vital signs and lab reports. |
| `dictionary_loinc.csv` | LOINC data dictionary. |
| `dictionary_rxnorm.csv` | RxNorm data dictionary. |
| `dictionary_snomed.csv` | SNOMED-CT data dictionary. | 

## File schema

### Conditions
| Column Name | Data Type | Required? | Description |
|-------------|-----------|-----------|-------------|
| Start | Date (`YYYY-MM-DD`) | `true` | The date the condition was diagnosed. |
| Stop | Date (`YYYY-MM-DD`) | `false` | The date the condition resolved, if applicable. |
| Patient | UUID | `true` | Foreign key to the Patient. |
| Encounter | UUID | `true` | Foreign key to the Encounter when the condition was diagnosed. |
| Code | String | `true` | Diagnosis code from SNOMED-CT |

### Encounters
| Column Name | Data Type | Required? | Description |
|-------------|-----------|-----------|-------------|
| Id | UUID | `true` | Primary Key. Unique Identifier of the encounter. |
 Start | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | `true` | The date and time the encounter started. |
| Stop | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | `false` | The date and time the encounter concluded. |
| Patient | UUID | `true` | Foreign key to the Patient. |
| Organization | UUID | `true` | Foreign key to the Organization. |
| Provider | UUID | `true` | Foreign key to the Provider. |
| Payer | UUID | `true` | Foreign key to the Payer. |
| EncounterClass | String | `true` | The class of the encounter, such as `ambulatory`, `emergency`, `inpatient`, `wellness`, or `urgentcare` |
| Code | String | `true` | Encounter code from SNOMED-CT |
| Base_Encounter_Cost | Numeric | `true` | The base cost of the encounter, **not** including any line item costs related to medications, immunizations, procedures, or other services. |
| Total_Claim_Cost | Numeric | `true` | The total cost of the encounter, including all line items. |
| Payer_Coverage | Numeric | `true` | The amount of cost covered by the Payer. |
| ReasonCode | String | `false` | Diagnosis code from SNOMED-CT, **only if** this encounter targeted a specific condition. |

### Medications
| Column Name | Data Type | Required? | Description |
|-------------|-----------|-----------|-------------|
| Start | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | `true` | The date and time the medication was prescribed. |
| Stop | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | `false` | The date and time the prescription ended, if applicable. |
| Patient | UUID | `true` | Foreign key to the Patient. |
| Payer | UUID | `true` | Foreign key to the Payer. |
| Encounter | UUID | `true` | Foreign key to the Encounter where the medication was prescribed. 
| Code | String | `true` | Medication code from RxNorm. |
| Base_Cost | Numeric | `true` | The line item cost of the medication. |
| Payer_Coverage | Numeric | `true` | The amount covered or reimbursed by the Payer. |
| Dispenses | Numeric | `true` | The number of times the prescription was filled. |
| TotalCost | Numeric | `true` | The total cost of the prescription, including all dispenses. |
| ReasonCode | String | `false` | Diagnosis code from SNOMED-CT specifying why this medication was prescribed. |

### Observations
| Column Name | Data Type | Required? | Description |
|-------------|-----------|-----------|-------------|
| Date | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | `true` | The date and time the observation was performed. |
| Patient | UUID | `true` | Foreign key to the Patient. |
| Encounter | UUID | `true` | Foreign key to the Encounter where the observation was performed. |
| Category | String | `false` | Observation category. |
| Code | String | `true` | Observation or Lab code from LOINC |
| Value | String | `true` | The recorded value of the observation. |
| Units | String | `false` | The units of measure for the value. |
| Type | String | `true` | The datatype of `Value`: `text` or `numeric` |

### Patients
| Column Name | Data Type | Required? | Description |
|-------------|-----------|-----------|-------------|
| Id | UUID | `true` | Primary Key. Unique Identifier of the patient. |
| BirthDate | Date (`YYYY-MM-DD`) | `true` | The date the patient was born. |
| DeathDate | Date (`YYYY-MM-DD`) | `false` | The date the patient died. |
| SSN | String | `true` | Patient Social Security identifier. |
| Drivers | String | `false` | Patient Drivers License identifier. |
| Passport | String | `false` | Patient Passport identifier. |
| Prefix | String | `false` | Name prefix, such as `Mr.`, `Mrs.`, `Dr.`, etc. |
| First | String | `true` | First name of the patient. |
| Last | String | `true` | Last or surname of the patient. |
| Suffix | String | `false` | Name suffix, such as `PhD`, `MD`, `JD`, etc. |
| Maiden | String | `false` | Maiden name of the patient. |
| Marital | String | `false` | Marital Status. `M` is married, `S` is single. Currently no support for divorce (`D`) or widowing (`W`) |
| Race | String | `true` | Description of the patient's primary race. Valid values: `white`, `black`, `asian`, `hawaiian`, `other`, `native`.|
| Ethnicity | String | `true` | Description of the patient's primary ethnicity. Valid values: `nonhispanic` and `hispanic`. |
| Gender | String | `true` | Gender. `M` is male, `F` is female. |
| BirthPlace | String | `true` | Name of the town where the patient was born. |
| Address | String | `true` | Patient's street address without commas or newlines. |
| City | String | `true` | Patient's address city. |
| State | String | `true` | Patient's address state. |
| County | String | `false` | Patient's address county. |
| FIPS County Code | String | `false` | Patient's FIPS county code. |
| Zip | String | `false` | Patient's zip code. |
| Lat | Numeric | `false` | Latitude of Patient's address. |
| Lon | Numeric | `false` | Longitude of Patient's address. |
| Healthcare_Expenses | Numeric | `true` | The total lifetime cost of healthcare to the patient (i.e. what the patient paid). |
| Healthcare_Coverage | Numeric | `true` | The total lifetime cost of healthcare services that were covered by Payers (i.e. what the insurance company paid). |
| Income | Numeric | `true` | Annual income for the Patient |

