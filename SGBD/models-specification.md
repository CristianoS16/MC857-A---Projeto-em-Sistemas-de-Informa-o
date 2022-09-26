# Models Specification

## Patients

> Version history note.

Patient demographic data.

### Schema in Relational Format

| Attribute                | Type     | Description           |
|:-------------------------|:---------|:----------------------|
| `Id (Usage identifier)`              | UUID | Primary Key. Unique Identifier of the patient. |
| `BirthDate (Birth date)`              | Date (`YYYY-MM-DD`) | The date the patient was born. |
| `DeathDate (Death date)`              | Date (`YYYY-MM-DD`) | The date the patient died. |
| `First (Given name)`              | String | First name of the patient. |
| `Last (Family name)`              | String | Last or surname of the patient. |
| `Marital (Marital status)`              | String | Marital Status. `M` is married, `S` is single. Currently no support for divorce (`D`) or widowing (`W`) |
| `Race`              | String | Description of the patient's primary race. |
| `Ethnicity (Ethnicity)`              | String | Description of the patient's primary ethnicity. |
| `Gender (Gender)`              | String | Gender. `M` is male, `F` is female. |
| `BirthPlace (Country of birth)`              | String | Name of the town where the patient was born. |
| `Lat`              | Numeric | Latitude of Patient's address. |
| `Lon`              | Numeric | Longitude of Patient's address. |

### Example in JSON Format:

```json
[
  {
  "patient": {
    "id": "1d604da9-9a81-4ba9-80c2-de3375d59b40",
    "birthDate": "1998-05-25",
    "deathDate": "",
    "first": "Mariana",
    "last": "Rutherford",
    "marital": "M",
    "race": "white",
    "ethnicity": "hispanic",
    "gender": "F",
    "birthPlace": "Yarmouth  Massachusetts  US",
    "lat": "42.63614335069588",
    "lon": "-71.3432549217789"
    }
  }
]
```
---

## Observations

> Version history note.

Patient observations including laboratory tests.

### Schema in Relational Format

| Attribute                | Type     | Description           |
|:-------------------------|:---------|:----------------------|
| `Date`              | Date	iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | The date and time the observation was performed. |
| `Patient (Usage identifier)`              | UUID | Foreign key to the Patient. |
| `Encounter`              | UUID | Foreign key to the Encounter where the observation was performed. |
| `Category`              | String | Observation category. |
| `Code`              | String | Observation or Lab code from LOINC |
| `Description`              | String | Description of the observation or lab. |
| `Value`              | String | The recorded value of the observation. |
| `Units`              | String | The units of measure for the value. |
| `Type`              | String | The datatype of `Value`: `text` or `numeric` |

### Example in JSON Format:

```json
[
  {
  "observation": {
    "date": "2012-01-23T17:45:28Z",
    "patient": "1d604da9-9a81-4ba9-80c2-de3375d59b40",
    "encounter": "d0c40d10-8d87-447e-836e-99d26ad52ea5",
    "category": "laboratory",
    "code": "718-7",
    "description": "Hemoglobin [Mass/volume] in Blood",
    "value": "15.1",
    "units": "g/dL",
    "type": "numeric"
    }
  }
]
```

---

## Encounters

> Version history note.

Patient encounter data.

### Schema in Relational Format

| Attribute                | Type     | Description           |
|:-------------------------|:---------|:----------------------|
| `Id`              | UUID | Primary Key. Unique Identifier of the encounter. |
| `Start`              | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | The date and time the encounter started. |
| `Stop`              | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | The date and time the encounter concluded. |
| `Patient`              | UUID | Foreign key to the Patient. |
| `EncounterClass`              | String | The class of the encounter, such as `ambulatory`, `emergency`, `inpatient`, `wellness`, or `urgentcare` |
| `Code`              | String | Encounter code from SNOMED-CT |
| `Description`              | String | Description of the type of encounter. |
| `ReasonCode	`              | String | Diagnosis code from SNOMED-CT, only if this encounter targeted a specific condition. |
| `ReasonDescription	`              | String | Description of the reason code. |

### Example in JSON Format:

```json
[
  {
  "encounter": {
    "id": "d0c40d10-8d87-447e-836e-99d26ad52ea5",
    "start": "2010-01-23T17:45:28Z",
    "stop": "2010-01-23T18:10:28Z",
    "patient": "1d604da9-9a81-4ba9-80c2-de3375d59b40",
    "encounterClass": "ambulatory",
    "code": "185345009",
    "description": "Encounter for symptom",
    "reasonCode": "10509002",
    "reasonDescription": "Acute bronchitis (disorder)"
    }
  }
]
```

---

## Conditions

> Version history note.

Patient conditions or diagnoses.

### Schema in Relational Format

| Attribute                | Type     | Description           |
|:-------------------------|:---------|:----------------------|
| `Start`              | Date (`YYYY-MM-DD`) | The date the condition was diagnosed. |
| `Stop`              | Date (`YYYY-MM-DD`) | The date the condition resolved, if applicable. |
| `Patient`              | UUID | Foreign key to the Patient. |
| `Encounter`              | UUID | Foreign key to the Encounter when the condition was diagnosed. |
| `Code`              | String | Diagnosis code from SNOMED-CT |
| `Description (Status)`              | String | Description of the condition. |

### Example in JSON Format:

```json
[
  {
  "condition": {
    "start": "2010-03-17",
    "stop": "2010-04-16",
    "patient": "1d604da9-9a81-4ba9-80c2-de3375d59b40",
    "encounter": "d0c40d10-8d87-447e-836e-99d26ad52ea5",
    "code": "36971009",
    "description": "Sinusitis (disorder)"
    }
  }
]
```

---

## Medications

> Version history note.

Patient medication data.

### Schema in Relational Format

| Attribute                | Type     | Description           |
|:-------------------------|:---------|:----------------------|
| `Start (Order start date/time)`              | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | The date and time the medication was prescribed. |
| `Stop (Order stop date/time)`              | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | The date and time the prescription ended, if applicable. |
| `Patient (Usage identifier)`              | UUID | Foreign key to the Patient. |
| `Encounter`              | UUID | Foreign key to the Encounter where the medication was prescribed. |
| `Code (Name - Medication detail)`              | String | Medication code from RxNorm. |
| `Description (Description - Medication detail)	`              | String | Description of the medication. |
| `Dispenses`              | Numeric | The number of times the prescription was filled. |
| `ReasonCode (Clinical indication)`              | String | Diagnosis code from SNOMED-CT specifying why this medication was prescribed. |
| `ReasonDescription (Clinical indication)`              | String | Description of the reason code. |

### Example in JSON Format:

```json
[
  {
  "medication": {
    "start": "2020-11-25T12:12:33Z",
    "stop": "2020-12-30T21:12:49Z",
    "patient": "1d604da9-9a81-4ba9-80c2-de3375d59b40",
    "encounter": "d0c40d10-8d87-447e-836e-99d26ad52ea5",
    "code": "562251",
    "description": "Amoxicillin 250 MG / Clavulanate 125 MG Oral Tablet",
    "dispenses": "1",
    "reasonCode": "36971009",
    "reasonDescription": "Sinusitis (disorder)"
    }
  }
]
```


---

## Allergies

> Version history note.

Patient allergy data.

### Schema in Relational Format

| Attribute                | Type     | Description           |
|:-------------------------|:---------|:----------------------|
| `Start`              | Date (`YYYY-MM-DD`) | The date the allergy was diagnosed. |
| `Stop`              | Date (`YYYY-MM-DD`) | The date the allergy ended, if applicable. |
| `Patient`              | UUID | Foreign key to the Patient. |
| `Encounter`              | UUID | Foreign key to the Encounter when the allergy was diagnosed. |
| `Code`              | String | Allergy code |
| `System`              | String | Terminology system of the Allergy code. `RxNorm` if this is a medication allergy, otherwise `SNOMED-CT`. |
| `Description`              | String | Description of the `Allergy` |
| `Type`              | String | Identify entry as an `allergy` or `intolerance`. |
| `Category`              | String | Identify the category as `drug`, `medication`, `food`, or `environment`. |
| `Reaction1`              | String | Optional SNOMED code of the patients reaction. |
| `Description1`              | String | Optional description of the `Reaction1` SNOMED code. |
| `Severity1`              | String | Severity of the reaction: `MILD`, `MODERATE`, or `SEVERE`. |
| `Reaction2`              | String | Optional SNOMED code of the patients second reaction. |
| `Description2`              | String | Optional description of the `Reaction2` SNOMED code. |
| `Severity2`              | String | Severity of the second reaction: `MILD`, `MODERATE`, or `SEVERE`. |

### Example in JSON Format:

```json
[
  {
  "allergy": {
    "start": "2010-03-17",
    "stop": "",
    "patient": "1d604da9-9a81-4ba9-80c2-de3375d59b40",
    "encounter": "d0c40d10-8d87-447e-836e-99d26ad52ea5",
    "code": "762952008",
    "System": "Unknown",
    "Description": "Peanut (substance)",
    "Type": "allergy",
    "Category": "food",
    "Reaction1": "247472004",
    "Description1": "Wheal (finding)",
    "Severity1": "MODERATE",
    "Reaction2": "271807003",
    "Description2	": "Eruption of skin (disorder)",
    "Severity2": "MILD",
    }
  }
]
```

---

## Procedures

> Version history note.

Patient procedure data including surgeries.

### Schema in Relational Format

| Attribute                | Type     | Description           |
|:-------------------------|:---------|:----------------------|
| `Start`              | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'`) | The date and time the procedure was performed. |
| `Stop`              | iso8601 UTC Date (`yyyy-MM-dd'T'HH:mm'Z'` | The date and time the procedure was completed, if applicable. |
| `Patient`              | UUID | Foreign key to the Patient. |
| `Encounter`              | UUID | Foreign key to the Encounter where the procedure was performed. |
| `Code (Report ID)`              | String | Procedure code from SNOMED-CT |
| `Description (Status)`              | String | Description of the procedure. |
| `ReasonCode`              | String | Diagnosis code from SNOMED-CT specifying why this procedure was performed. |
| `ReasonDescription`              | String | Description of the reason code. |

### Example in JSON Format:

```json
[
  {
  "procedure": {
    "start": "2020-11-25T21:12:49Z",
    "stop": "2020-11-25T21:23:25Z",
    "patient": "1d604da9-9a81-4ba9-80c2-de3375d59b40",
    "encounter": "d0c40d10-8d87-447e-836e-99d26ad52ea5",
    "code": "261352009",
    "description": "Face mask (physical object)",
    "reasonCode": "840544004",
    "reasonDescription": "Suspected COVID-19"
    }
  }  
]
```

