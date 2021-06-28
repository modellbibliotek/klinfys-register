AQL queries can either be added to this document or uploaded as text files to this directory

### Demographic base data from Better EHRScape
The info about date_of_birth and sex may be different (or absent) in EHRbase and other implementations
```
SELECT e/ehr_id/value AS ehr_id,
       e/ehr_status/Subject/external_ref/id/value AS patient_id,
       e/ehr_status/other_details/items[at0001]/value/value as sex,
       e/ehr_status/other_details/items[at0002]/value/value as date_of_birth
FROM EHR e
ORDER BY date_of_birth DESCENDING
```

### Experiment extracting SNOMED CT coded data

Remove the -- (comment marker on AQL) to include the entire composition object in the response

```
SELECT a/data[at0002]/events[at0003]/data[at0001]/items[at0004]/value AS frekvens,
       e/ehr_id/value AS ehr_id,
       c/uid/value AS composition_id,
       a/data[at0002]/events[at0003]/data[at0001]/items[at1023]/value AS Rytm,
       l/items[at0001]/items[at0002]/value/magnitude AS Resultat,
       l/items[at0001]/items[at0015]/value/value AS Snomed_CT_description,
       l/items[at0001]/items[at0015]/value/defining_code/code_string AS Snomed_CT_code,
       l/items[at0001]/items[at0015]/value/defining_code/terminology_id/value AS Snomed_CT_terminology_id
--   , c as the_whole_composition,
      
FROM EHR e
CONTAINS COMPOSITION c[openEHR-EHR-COMPOSITION.report.v1] 
CONTAINS (OBSERVATION a[openEHR-EHR-OBSERVATION.pulse.v2] and CLUSTER l[openEHR-EHR-CLUSTER.imaging_result.v0] ) 
WHERE l/items[at0001]/items[at0015]/value/defining_code/terminology_id/value = 'http://snomed.info/sct/'
OFFSET 0 LIMIT 10
```
