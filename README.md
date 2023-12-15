
    PURPOSE
    The LA-Letters-Code repository provides the code for the manuscript, "Planning prompts reduce opioid prescribing: A randomized trial" published in Nature Communications      on ###

    DOI: 10.5281/zenodo.10263890

    DATA (collected from October 2017 to May 2021)
        1. Decedent data was obtained from the LA Medical Examiner-Coroner
        2. Prescriber demographics and prescriptions were obtained from the Controlled Substance Utilization Review and Evaluation System (CURES)
        3. MME Conversion factors, drug names and strengths, and drug NDCs obtained from the Centers for Disease Control and Prevention 
           a. Opioid National Drug Code and Oral MME Conversion File Update. https://www.cdc.gov/opioids/data-resources/index.html (2023). 
        4. DME conversion factors and info. from Borelli et al.
           b. Borrelli, E. P., Bratberg, J., Hallowell, B. D., Greaney, M. L. & Kogut, S. J. Application of a diazepam milligram equivalency algorithm to assess benzodiazepine dose intensity in Rhode Island in 2018. J Manag Care Spec Pharm 28, 58–68 (2022).

    ANALYSES 
         PRIMARY
         1.  Multi-level (mixed effects) left censored regression
         2.  Estimates used to derive adjusted MME and DME to assess whether difference in total mean pre-to-post intervention average daily MME or DME differed between study arms 

         SECONDARY
         1. Rxs => 50 MME
         2. Rxs > 90 MME
         3. New patients who received an opioid Rx

     
         POST-HOC
         1. Equality of coefficients testing whether study start coefficient (weeks 4-22) differed from study end (weeks 23-52)
         2. Three-way interaction tesing whether letter effect post-intervention was stronger for prescribers who had muliple decedents and received multiple comparator letters
     
    SOFTWARE
    SAS version 9.4, STATA software version 16, and R version 3.6.0

    LICENSE
    Schaeffer Center for Health Policy and Economics, University Southern California

    USAGE 
        FILE 1 (SAS)
            1. Imports CURES Rx data 
            2. Obtains drug strength, pill qty, and conversion factor per Rx
            3. Calculates MME per Rx
            4. Removes secondary decedents; post-intervention period is defined by first decedent's letter sent date 
            5. Calculates no. of Rxs, prescribers, and decedents at each study phase for constort diagram Figure 1
            6. Creates cleaned dataset called "letters_sample_mme"

        FILE 2 (STATA)
            1. Imports letters_sample_mme
            2. Creates flat file which has total, daily MME per-prescriber, called "analytic_daily_mme"
               a. Unused because of model convergence issues
            3. Creates flat file which has total, weekly MME per-prescriber, called "analytic_weekly_mme"
            4. Adds variables for post-intervention, log MME, no. of decedents, study start, study_end, no. of new patients, and high dose Rxs (50 and 90 MME)

        FILE 3 (SAS)
            1. Imports decedent data
            2. Uses proc sql, proc ttest, and proc freq to get counts (%) and mean (sd) between study arms for decedent Table 1

        FILE 4 (SAS)
            1. Imports presciber data
            2. Uses proc sql and proc freq to get counts (%) between study arms for prescriber Table 2

        FILE 5 (STATA)
            1. Imports analytic_weekly_mme
            2. Metobit testing interaction between intervention and letter effect (is_let*post)
            3. Adjusts post-intervention, per-prescriber, weekly MME for each letter group
            4. Calculates pre-intervention means for each letter group and trims by 9
            5. Bootstraps 95% confidence intervals for difference in pre- to post-intervention effects, and difference-in-difference between study arms
            6. Melogits testing interaction between intervention and letter effect (is_let*post) for high-dose Rxs, and new patient starts
            7. Lincom tests difference in study start and study end coefficients
            8. Converts results to matrices, and exports

        FILE 6 (STATA)
            1. Imports analytic_weekly_mme
            2. Metobit testing interaction between intervention, letter effect, and no. of decedents (is_let*post*decid_cat)
            3. Adjusts post-intervention, per-prescriber, weekly MME for each letter and decedent group
            4. Calculates pre-intervention means for each letter and decedent group, and trims by 9%
            5. Bootstraps 95% confidence intervals for difference in pre- to post-int effects between study arms and decedent groups
            6. Converts results to matrix and exports

        FILE 7 (R) 
            1. Creates dataframe with adjusted post-intervention mean MME by each letter and decedent group
            2. Uses ggplot to create Figure 2

    CONTACT
    For questions regarding code or data access email Emily Stewart, epstewar@usc.edu, and corresponding author Jason Doctor, jdoctor@usc.edu, respectively 
