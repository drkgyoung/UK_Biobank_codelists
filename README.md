# UK Biobank codelists

## HES codelists
### Codelist contents
* hes_icd9_diabetes and hes_icd10_diabetes are codes for any type of diabetes mellitus
### Implementation
* Use codes as prefixes e.g. E10 is listed as a diabetes ICD10 code, so any code beginning with E10 is a diabetes ICD10 code
* ICD codes do not contain lowercase letters, so case sensitivity is not required

## GP (primary care) codelists
### Codelist contents
* "\_diabetes": any type of diabetes mellitus
* "\_diabetes_exclusion": types of diabetes mellitus which are not Type 1 or Type 2 diabetes
* "\_hba1c": HbA1c tests
* "\_pbcl_hba1c": HbA1c tests from the Pathology Bounded Code List (PBCL). TThe PBCL defines codes used in electronic reporting from pathology labs to GPs. Patient biomarker measurements with these codes may be more reliable than measurements with non-PBCL codes, as the latter are more likely to have been inputted manually by a GP.
* "\_diabetes_meds": glucose-lowering medications (including insulin). Note that 'actos ' includes a final space to prevent matching to medications containing lactose etc.
* "\_glucagon": glucagon
  * "\_test_strips": glucose testing strips. See below note on implementation.
### Implementation
* Clinical data from UK Biobank (diagnoses, test results etc.) are provided in the gp_clinical file, which contains both read_2 and read_3 fields. "\_read2_" files are lists of Read 2 codes to use with the read_2 field; "\_read3_" files are Read 3 codes to use with the read_3 field. Matches must be exact and case sensitive. Both fields should be used to identify individuals with a specific condition/type of test etc.
* Prescription data from UK Biobank are provided in the gp_scripts file. This contains read_2, bnf_code, dmd_code and drug_name fields; none are fully populated. I use a combined approach of searching both the read_2 and drug_name fields as combined this provides good coverage.
  * "\_read2drugs_" files are Read 2 codes to use with the read_2 field. Matches must be exact and case sensitive.
  * "\_drugname_" files are full or partial names of medications to use with the drug_name field. These should be used in non-case sensitive wildcard searches which allow any number of characrters before or after the name.
