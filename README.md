# UK Biobank codelists

&nbsp;

## Defining whether an individual has a pre-existing diagnosis of diabetes at enrolment
See file 'enrolment_diabetes_diagnosis.txt' which lists the UK Biobank variables (and codelists from this repository to be used with these variables) required to define diabetes status at UK Biobank enrolment.

&nbsp;

## HES codelists
### Codelist contents
* **hes_icd9_diabetes** and **hes_icd10_diabetes** are codes for any type of diabetes mellitus
* **hes_icd9_hyperglycaemia** and **hes_icd10_hyperglycaemia** are codes for hyperglycaemia (ICD9 code 7902 is for 'abnormal glucose' but commonly used for hyperglycaemia)
### Implementation
* Use codes as prefixes e.g. E10 is listed as a diabetes ICD10 code, so any code beginning with E10 is a diabetes ICD10 code
* ICD codes do not contain lowercase letters, so case sensitive searching is not required
* Episode start date used as date of diagnosis, or admission date where episode start date not available

&nbsp;

## GP (primary care) codelists
### Codelist contents
* **gp\_\*\_diabetes**: codes for Type 1 diabetes, Type 2 diabetes, or unspecified diabetes mellitus, including processes of care and diabetes-specific complications
* **gp\_\*\_diabetes_qof**: codes for Type 1 diabetes, Type 2 diabetes, or unspecified diabetes mellitus, from version 38 of the Quality and Outcomes Framework (QOF; available here: https://webarchive.nationalarchives.gov.uk/ukgwa/20220117164934mp_///nhs-prod.global.ssl.fastly.net/binaries/content/assets/legacy/excel/qof_v38_expanded_cluster_list_.xlsx)
* **gp\_\*\_diabetes_other_types**: types of diabetes mellitus which are not Type 1 or Type 2 diabetes
* **gp\_\*\_hba1c**: HbA1c tests. See below note on implementation.
* **gp\_\*\_pbcl_hba1c**: HbA1c tests from the Pathology Bounded Code List (PBCL). The PBCL defines codes used in electronic reporting from pathology labs to GPs. Patient biomarker measurements with these codes may be more reliable than measurements with non-PBCL codes, as the latter are more likely to have been inputted manually by a GP. General ‘glycosylated haemoglobin’ codes or HbA1 codes were excluded. See below note on implementation.
* **gp\_\*\_hyperglycaemia**: codes for hyperglycaemia (does not include glucose blood/urine tests)
* **gp\_\*\_glctests**: Glucose tests (random, fasting, and 2-hour postprandial/post-oral glucose tolerance test \[OGTT\]). See below note on implementation.
* **gp\_\*\_diabetes_meds**: glucose-lowering medications (including insulin) plus glucagon. Note that 'actos ' includes a final space to prevent matching to medications containing lactose etc.
* **gp\_\*\_blood_pressure_meds**: blood pressure medications (classes included: ACE-inhibitors, angiotensin receptor blockers, beta-blockers, calcium channel inhibitors, thiazide-like diuretics). Note that 'istin ', 'optil ' and 'univer ' include a final space to prevent matching to medications containing histine, universal etc.
* **gp\_\*\_glucagon**: glucagon
* **gp_\*\_test_strips**: glucose testing strips. See below note on implementation.

### Implementation
#### Clinical data
Clinical data from UK Biobank (diagnoses, test results etc.) are provided in the gp_clinical file, which contains both read_2 and read_3 fields.
  * **gp\_read2\_\*** files are lists of Read 2 codes to use with the read_2 field
  * **gp\_read3\_\*** files are Read 3 codes to use with the read_3 field. Matches must be exact and case sensitive. Both fields should be used to identify individuals with a specific condition/type of test etc.
  * HbA1c test searches only: the numeric result of the test will either be in the value1 field of gp_clinical, or value1 will contain "OPR003" and the result will be in value2.
    * When using the full HbA1c codelist (to find all HbA1c tests) we ignore any units suggested by the Read code description or in the value3 field and assume all values <=20 are in % units and all values >20 are in mmol/mol units. Values <3.9 and >195 are excluded. If a patient has multiple valid reading on the same day, results are averaged by day. HbA1c measurements in percentage can be converted to mmol/mol using the NGSP/IFCC equation (NGSP (%) = 0.0915 IFCC (mmol/mol) + 2.15).  
    * When using the PBCL HbA1c codelist to find a selective group of HbA1c tests which are more likely to be accurate: assume Read codes with 'DCCT aligned' in the description are in % units and exclude values <3.9 or >20; assume Read codes with 'IFCC standardised' in the description are in mmol/mol units and exclude values <20.01 or >195.
* Glucose test searches only: the numeric result of the test will either be in the value1 field of gp_clinical, or value1 will contain "OPR001", "OPR003" or "OPR004", and the result will be in value2. Exclude values <2.5 or >30. All values assumed to be mmol/L. Hyperglycaemia defined as value >=7.0 mmol/L (fasting) or >=11.1 mmol/L (random/2-hour postprandial).
* BMI searches only: the numeric result will either be in the value1 or value3 field of gp_clinical. Exclude values <15 or >100 (adults only). All values assumed to be in kg/m2.

#### Prescriptions
Prescription data from UK Biobank are provided in the gp_scripts file. This contains read_2, bnf_code, dmd_code and drug_name fields; none are fully populated. We search both the read_2 and drug_name fields as combined these provide good coverage.
  * **gp\_read2drugs\_\*** files are Read 2 codes to use with the read_2 field. These should be matched on the first 5 characters of the read_2 field in a case-sensitive manner.
  * **gp\_drugname\_\*** files are full or partial names of medications to use with the drug_name field. These should be used in non-case sensitive wildcard searches which allow any number of characters before or after the name.
  * Glucose test strip searches only: results containing 'lancet' (again, searched for as a non-case sensitive wildcard) should be excluded from the results
  * Blood pressure medication searches only: results containing 'eye', 'ophthalmic', 'drop', 'drp', '%', 'ointment', 'gel', 'cream' or 'crm' should be excluded to remove eye drops and topical creams. Results containing 'colistin ', 'fabahistin ' or 'antistin ' (picked by 'istin ' keyword) and 'guard' (picked up by 'uard' keyword) should also be removed.

&nbsp;

## Codelists for UK Biobank variables 20002 (verbal interview: non-cancer illnesses), 20003 (verbal interview: treatment/medications), and 20004 (verbal interview: operations)
### Codelist contents
* **20002_cvd**: codes for cardiovascular disease, taken from https://www.ahajournals.org/doi/pdf/10.1161/JAHA.117.007621.
* **20002_diabetes**: codes for any time of diabetes mellitus including gestational diabetes, plus diabetes-specific complications.
* **20002_hypertension**: codes for hypertension.
* **20002_pcos**: code for polycystic ovarian syndrome.
* **20003_diabetes_meds**: glucose-lowering medications (including insulin) plus glucagon.
* **20003_blood_pressure_meds**: blood pressure medications (classes included: ACE-inhibitors, angiotensin receptor blockers, beta-blockers, calcium channel inhibitors, thiazide-like diuretics).
* **20004_cvd**: codes for operations indicative of cardiovascular disease, taken from https://www.ahajournals.org/doi/pdf/10.1161/JAHA.117.007621


