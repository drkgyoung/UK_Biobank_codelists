# UK Biobank codelists

&nbsp;

## HES codelists
### Codelist contents
* **hes_icd9_diabetes** and **hes_icd10_diabetes** are codes for any type of diabetes mellitus
### Implementation
* Use codes as prefixes e.g. E10 is listed as a diabetes ICD10 code, so any code beginning with E10 is a diabetes ICD10 code
* ICD codes do not contain lowercase letters, so case sensitivity is not required

&nbsp;

## GP (primary care) codelists
### Codelist contents
* **gp\_\*\_diabetes**: any type of diabetes mellitus
* **gp\_\*\_diabetes_exclusion**": types of diabetes mellitus which are not Type 1 or Type 2 diabetes
* **gp\_\*\_hba1c**: HbA1c tests. See below note on implemntation.
* **gp\_\*\_pbcl_hba1c**: HbA1c tests from the Pathology Bounded Code List (PBCL). The PBCL defines codes used in electronic reporting from pathology labs to GPs. Patient biomarker measurements with these codes may be more reliable than measurements with non-PBCL codes, as the latter are more likely to have been inputted manually by a GP. See below note on implemntation.
* **gp\_\*\_diabetes_meds**: glucose-lowering medications (including insulin). Note that 'actos ' includes a final space to prevent matching to medications containing lactose etc.
* **gp\_\*\_glucagon**: glucagon
* **gp_\*\_test_strips**: glucose testing strips. See below note on implementation.

&nbsp;

### Implementation
* Clinical data from UK Biobank (diagnoses, test results etc.) are provided in the gp_clinical file, which contains both read_2 and read_3 fields
  * **gp\_read2\_\*** files are lists of Read 2 codes to use with the read_2 field
  * **gp\_read3\_\*** files are Read 3 codes to use with the read_3 field. Matches must be exact and case sensitive. Both fields should be used to identify individuals with a specific condition/type of test etc.
  * HbA1c test searches only: the numeric result of the test will either be in the value1 field gp_clinical, or value1 will contain "OPR003" and the result will be in value2. We ignore any units suggested by the Read code description or in the value3 field and assume all values <=20 are in % and all values >20 are in mmol/mol. Values <3.9 and >195 are excluded. If a patient has mutiple valid reading on the same day, results are averaged by day.
* Prescription data from UK Biobank are provided in the gp_scripts file. This contains read_2, bnf_code, dmd_code and drug_name fields; none are fully populated. We search both the read_2 and drug_name fields as combined these provide good coverage.
  * **gp\_read2drugs\_\*** files are Read 2 codes to use with the read_2 field. Matches must be exact and case sensitive.
  * **gp\_drugname\_\*** files are full or partial names of medications to use with the drug_name field. These should be used in non-case sensitive wildcard searches which allow any number of characrters before or after the name.
  * Glucose test strip searches only: results containing 'lancet' (again, searched for as a non-case sensitive wildcard) should be excluded from the results
