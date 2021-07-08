AQL queries can either be added to this document or uploaded as text files to this directory

Hint: If your AQL query API does not support CSV export, tnen the raw standard openEHR JSON format can be converted to CSV e.g using https://konklone.io/json/ or libraries like https://csvkit.readthedocs.io/en/latest/

### 1. Demographic base data from Better EHRScape
The info about date_of_birth and sex may be different (or absent) in EHRbase and other implementations
```SQL
SELECT e/ehr_id/value AS ehr_id,
       e/ehr_status/Subject/external_ref/id/value AS patient_id,
       e/ehr_status/other_details/items[at0001]/value/value as sex,
       e/ehr_status/other_details/items[at0002]/value/value as date_of_birth
FROM EHR e
ORDER BY date_of_birth DESCENDING
```

### 2. Experiment extracting SNOMED CT coded data

Remove the -- (comment marker on AQL) to include the entire composition object in the response

```SQL
SELECT pul/data[at0002]/events[at0003]/data[at0001]/items[at0004]/value AS frekvens,
       pul/data[at0002]/events[at0003]/data[at0001]/items[at1023]/value AS Rytm,
       img_cl/items[at0001]/items[at0002]/value/magnitude AS Resultat,
       img_cl/items[at0001]/items[at0015]/value/value AS Snomed_CT_description,
       img_cl/items[at0001]/items[at0015]/value/defining_code/code_string AS Snomed_CT_code,
       img_cl/items[at0001]/items[at0015]/value/defining_code/terminology_id/value AS terminology_id,
       e/ehr_id/value AS ehr_id,
       c/uid/value AS composition_id
--   , c as the_whole_composition
      
FROM EHR e
CONTAINS COMPOSITION c 
CONTAINS (OBSERVATION pul[openEHR-EHR-OBSERVATION.pulse.v2] and 
          CLUSTER img_cl[openEHR-EHR-CLUSTER.imaging_result.v0] ) 
WHERE terminology_id = 'http://snomed.info/sct/'
ORDER BY ehr_id
OFFSET 0 LIMIT 10
```

### 3. all imaging_result clusters

```SQL
SELECT b/items[at0001]/items[at0002]/value AS Resultat,
       b/items[at0001]/items[at0002]/value/magnitude AS Resultat_magn,
       b/items[at0001]/items[at0002]/value AS Resultat,
       b/items[at0001]/items[at0002]/value AS Resultat_kod,
FROM EHR e
CONTAINS COMPOSITION c[openEHR-EHR-COMPOSITION.report.v1] 
CONTAINS CLUSTER b[openEHR-EHR-CLUSTER.imaging_result.v0] 
OFFSET 0 LIMIT 100
```
### 4. Experiment extracting SNOMED CT coded data from CLUSTER.imaging_result clusters inside OBSERVATION.imaging_exam_result

```SQL
SELECT img_cl/items[at0001]/items[at0002]/value AS Resultat,
       img_cl/items[at0001]/items[at0002]/value/magnitude AS magn,
       img_cl/items[at0001]/items[at0015]/value/value AS Snomed_CT_description,
       img_cl/items[at0001]/items[at0015]/value/defining_code/code_string AS Snomed_CT_code,
       img_cl/items[at0001]/items[at0015]/value/defining_code/terminology_id/value AS terminology_id,
       o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value AS Undersökning,
       o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/defining_code/code_string AS  Undersökning_kod,
       o/data[at0001]/events[at0002]/data[at0003]/items[at0055]/value AS Anatomisk_lokalisation,
       o/data[at0001]/events[at0002]/data[at0003]/items[at0005] AS Modalitet,
       c/uid/value AS composition_id,
      e/ehr_id/value AS ehr_id
FROM EHR e
CONTAINS COMPOSITION c 
CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.imaging_exam_result.v0] 
CONTAINS CLUSTER img_cl[openEHR-EHR-CLUSTER.imaging_result.v0] 
WHERE terminology_id = 'http://snomed.info/sct/'
ORDER BY ehr_id
OFFSET 0 LIMIT 50
```

### 5. vänster kammares ejektionsfraktion (SCT ID = 250908004)

```SQL
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
### 6. Lista alla mätningar för alla patienter där vänster kammares ejektionsfraktion (SCT ID = 250908004) < 40%

```SQL
SELECT b/items[at0001]/items[at0002]/value/magnitude AS Resultat_magnitud,
       b/items[at0001]/items[at0015]/value AS Resultatets_namn,
       b/items[at0001]/items[at0015]/value/defining_code/code_string AS Resultat_kod,
       b/items[at0001]/items[at0015]/value/defining_code/terminology_id/value AS Resultat_termionologi,
       e/ehr_id/value,
       c/context/start_time/value AS besökstid
FROM EHR e
CONTAINS COMPOSITION c
CONTAINS CLUSTER b[openEHR-EHR-CLUSTER.imaging_result.v0] 
WHERE Resultat_kod = '250908004' AND Resultat_termionologi = 'http://snomed.info/sct/' AND Resultat_magnitud < 40
OFFSET 0 LIMIT 100

--Absolute paths + no comments + no line breaks
SELECT b/items[at0001]/items[at0002]/value/magnitude AS Resultat_magnitud, b/items[at0001]/items[at0015]/value AS Resultatets_namn, b/items[at0001]/items[at0015]/value/defining_code/code_string AS Resultat_kod, b/items[at0001]/items[at0015]/value/defining_code/terminology_id/value AS Resultat_termionologi, e/ehr_id/value FROM EHR e CONTAINS COMPOSITION c CONTAINS CLUSTER b[openEHR-EHR-CLUSTER.imaging_result.v0] WHERE b/items[at0001]/items[at0015]/value/defining_code/code_string = '250908004' AND b/items[at0001]/items[at0015]/value/defining_code/terminology_id/value = 'http://snomed.info/sct/' AND b/items[at0001]/items[at0002]/value/magnitude < 40

--Result + ehr_id
SELECT b/items[at0001]/items[at0002]/value/magnitude AS Resultat_magnitud, e/ehr_id/value FROM EHR e CONTAINS COMPOSITION c CONTAINS CLUSTER b[openEHR-EHR-CLUSTER.imaging_result.v0] WHERE b/items[at0001]/items[at0015]/value/defining_code/code_string = '250908004' AND b/items[at0001]/items[at0015]/value/defining_code/terminology_id/value = 'http://snomed.info/sct/' AND b/items[at0001]/items[at0002]/value/magnitude < 40
```

### 7. Räkna hur många journaler som har minst en mätning där vänster kammares ejektionsfraktion (SCT ID = 250908004) < 40%
```SQL
SELECT COUNT(DISTINCT e/ehr_id/value) AS number_of_matching_EHRs 
FROM EHR e
CONTAINS COMPOSITION c
CONTAINS CLUSTER b[openEHR-EHR-CLUSTER.imaging_result.v0] 
WHERE b/items[at0001]/items[at0015]/value/defining_code/code_string = '250908004' -- Resultat_kod
  AND b/items[at0001]/items[at0015]/value/defining_code/terminology_id/value  = 'http://snomed.info/sct/'--Resultat_termionologi
  AND b/items[at0001]/items[at0002]/value/magnitude < 40 -- Resultat_magnitud
```

## Exempel på relevanta frågor för klinfys

När det gäller ”prototyp-registret har jag 2 sorters frågor: en som gäller t.ex. 
* "Hur många i registret har en aortaflödeshastighet som överskrider 3.5 m/s?” 
 
Denna fråga kan identifiera alla dem som troligen har en aortastenos som blir behandlingskrävande inom det närmaste året. När frågan ställs på nytt efter 6-12 månader kan vi hitta dem som är nydiagnosticerade inom ett visst intervall och utifrån ett sådant svar kan vi beräkna det årliga tillskottet av patienter med behandlingskrävande aortastenos.

För den enskilde individen vill man kunna avgöra **progress**, t.ex. definierad som **ökning av aortaflödeshastigheten med >0.5 m/s på 2 år**.

På motsvarande sätt vill vi ställa frågan: 
* ”Hur många i registret har en beräknad ejektionsfraktion <40%?
     * Besvaras med AQL-fråga nr 7 ovan

En individuell fråga kan vara: 
* "har denna patient uppvisat försämring, definierat som sänkning av ejektionsfraktionen med >5% under en 1 års-period"
(här kan man för onkologpatienter som kontrolleras var 3:e månad använda 5% sänkning mellan varje konsultationstillfälle.

