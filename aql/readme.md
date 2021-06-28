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

### all imaging_result clusters

```
SELECT b/items[at0001]/items[at0002]/value AS Resultat,
       b/items[at0001]/items[at0002]/value/magnitude AS Resultat_magn,
       b/items[at0001]/items[at0002]/value AS Resultat,
       b/items[at0001]/items[at0002]/value AS Resultat_kod,
FROM EHR e
CONTAINS COMPOSITION c[openEHR-EHR-COMPOSITION.report.v1] 
CONTAINS CLUSTER b[openEHR-EHR-CLUSTER.imaging_result.v0] 
OFFSET 0 LIMIT 100
```

### vänster kammares ejektionsfraktion (SCT ID = 250908004)

```
SELECT b/items[at0001]/items[at0002]/value AS Resultat,
       b/items[at0001]/items[at0002]/value/magnitude AS Resultat_magn,
       b/items[at0001]/items[at0015]/value AS Resultatets_namn,
       b/items[at0001]/items[at0015]/value/defining_code/code_string AS Resultat_kod,
       b/items[at0001]/items[at0015]/value/defining_code/terminology_id/value AS Resultat_termionologi
FROM EHR e
CONTAINS COMPOSITION c[openEHR-EHR-COMPOSITION.report.v1] 
CONTAINS CLUSTER b[openEHR-EHR-CLUSTER.imaging_result.v0] 
WHERE Resultat_kod = '250908004' AND Resultat_termionologi = 'http://snomed.info/sct/'
OFFSET 0 LIMIT 100
```

## Exempel på relevanta frågor för klinfys

När det gäller ”prototyp-registret har jag 2 sorters frågor: en som gäller t.ex. 
* "Hur många i registret har en aortaflödeshastighet som överskrider 3.5 m/s?” 
 
Denna fråga kan identifiera alla dem som troligen har en aortastenos som blir behandlingskrävande inom det närmaste året. När frågan ställs på nytt efter 6-12 månader kan vi hitta dem som är nydiagnosticerade inom ett visst intervall och utifrån ett sådant svar kan vi beräkna det årliga tillskottet av patienter med behandlingskrävande aortastenos.

För den enskilde individen vill man kunna avgöra **progress**, t.ex. definierad som **ökning av aortaflödeshastigheten med >0.5 m/s på 2 år**.

På motsvarande sätt vill vi ställa frågan: 
* ”Hur många i registret har en beräknad ejektionsfraktion <40%?

En individuell fråga kan vara: 
* "har denna patient uppvisat försämring, definierat som sänkning av ejektionsfraktionen med >5% under en 1 års-period"
(här kan man för onkologpatienter som kontrolleras var 3:e månad använda 5% sänkning mellan varje konsultationstillfälle.

