
    PURPOSE
    The LA-Letters-Code repository provides code for manuscript, "Planning prompts reduce opioid prescribing: A randomized trial" published in Nature Communications in January 2023

    DOI: 10.5281/zenodo.10263890

    DATA (collected from October 2017 to May 2021)
        1. Decedent data was obtained from the LA Medical Examiner-Coroner
        2. Prescriber demographics and prescriptions were obtained from the Controlled Substance Utilization Review and Evaluation System (CURES)
        3. MME Conversion factors, drug names and strengths, and drug NDCs obtained from Centers for Disease Control and Prevention 
           a. Opioid National Drug Code and Oral MME Conversion File Update. https://www.cdc.gov/opioids/data-resources/index.html (2023). 
        4. DME conversion factors and info. from Borelli et al.
           b. Borrelli, E. P., Bratberg, J., Hallowell, B. D., Greaney, M. L. & Kogut, S. J. Application of a diazepam milligram equivalency algorithm to assess benzodiazepine dose intensity in Rhode Island in 2018. J Manag Care Spec Pharm 28, 58–68 (2022).

    ANALYSES 
         PRIMARY
         1.  Multi-level (mixed effects) left censored regression
         2.  Estimates used to derive adjusted MME/DME to assess whether difference in total mean pre-to-post intervention average daily MME/DME differed between study arms 

         SECONDARY
         1. Rxs => 50 MME
         2. Rxs > 90 MME
         3. New patients who received an opioid/benzo Rx
         4. >20% decrease in DME 

         POST-HOC
         1. Equality of coefficients testing whether study start coefficient (weeks 4-22) differed from study end (weeks 23-52)
         2. Three-way interaction tesing whether letter effect post-intervention was stronger for prescribers who had muliple decedents and received multiple comparator letters
     
    SOFTWARE
    SAS version 9.4, STATA software version 16, and R version 4.3.2

    LICENSE
    Schaeffer Center for Health Policy and Economics, University Southern California

    USAGE 
        FILE 1 (SAS)
            1. Imports CURES Rx data 
            2. Obtains drug strength, pill qty, and conversion factor per Rx
            3. Calculates MME and DME per Rx
            4. Removes secondary decedents; post-intervention period is defined by first decedent's letter sent date 
            5. Calculates no. of Rxs, prescribers, and decedents at each study phase for constort diagram Figures 1 and 2
            6. Creates cleaned datasets letters_sample_mme/letters_sample_dme

        FILE 2 (STATA)
            1. Imports letters_sample_mme/letters_sample_dme
            2. Creates flat file which has total, daily MME (analytic_daily_mme) or DME (analytic_daily_dme) per-prescriber
               a. Unused because of model convergence issues
            3. Creates flat file which has total, weekly MME (analytic_weekly_mme) or DME (analytic_weekly_dme) per-prescriber
            4. Adds variables for post-intervention, log outcome, no. of decedents, study start, study_end, no. of new patients, and high dose Rxs (50 and 90 MME)

        FILE 3 (SAS)
            1. Imports decedent data
            2. Uses proc sql, proc ttest, and proc freq to get counts (%) and mean (sd) between study arms for decedent Table 1
            3. Ods excel outputs Table 1 

        FILE 4 (SAS)
            1. Imports presciber data
            2. Uses proc sql, proc t-test, and proc freq to get counts (%) between study arms for prescriber Table 2
            3. Ods excel outputs Table 2 

        FILE 5 (STATA)
            1. Imports analytic_weekly_mme/analytic weekly_dme
            2. Metobit tests two-way interaction between intervention and letter (is_let*post)
            3. Adjusts post-intervention, per-prescriber, weekly MME/DME for each letter group
            4. Calculates, and trims pre-intervention means
            5. Bootstraps 95% confidence intervals for difference in pre- to post-intervention effects, and difference-in-difference between study arms
            6. Melogits tests two-way interaction between intervention and letter (is_let*post) for high-dose Rxs, and new patient starts
            7. Meobit tests three-way interaction beween intervention, letter, and number of decedents (is_let*post*decid_cat)
            8. Melogit tests two-way interaction between letter and study start, and study end
            9. Lincom tests difference in study start and study end coefficients
            10. Converts results to matrices and exports for Tables 3 and 4, and Supplemental Tables S1a, S1b, S3a, and S3b

        FILE 6 (STATA)
            1. Imports analyic_weekly_mme or analytic_weekly_vme
            2. Quantile-quantile (QQ) plots for raw MME/DME and log MME or DME 
            3. Graph exports Q-Q plots for Supplemental Figures S1a-S1d

        FILE 7 (SAS) 
            1. Imports analytic_weekly_mme/analytic_weekly_vme 
            2. Proc sql calculates total, per-clinician MME/DME by letter, no. decedents for pre- and post- periods
            3. Data step for log total MME/DME
            4. Proc means for mean and median total MME/DME by letter and no. of decedents for pre- and post- periods
            5. Proc export to export box_data_mme/box_data_vme for R box plots
            6. Proc sql calculates total MME/DME by letter or no. of decedents, takes log, and replaces missing logs to 0
            7. Proc means calculates and outputs q1 and q3
            8. Proc sql joins q1 and q3 with sample, and caclulates outliers using Tukey's fences 
            9. Proc freq chi-square for no. of outliers by letter and no. of decedents for Supplemental Tables S2a and S2b
            10. Proc means for t-test testing difference is mean log MME/DME and MME/DME by letter and no. of decedents 
            
        FILE 8 (R)
            1. Load libraries and specify user-defined function inputs 
            2. Imports box_data_mme/box_data_vme, and case_when to create no. of decedents by letter group 
            3. Function changes graph background, font style, tick marks, aspect ratio, and size 
            4. Function for graph legend-creates separate boxplot with annotations 
            5. Ggplot graphs boxplots and legend
            6. Plot_grid, dml, add_slide, ph_with, print (target) creates and exports editable .pptx plot for Figures 3 and 4

        FILE 9 (STATA)
            1. Imports analytic_daily_vme 
            2. Collapse creates file with mean per-clinician DME, and total no. new patients in the pre- and post- periods
            3. Gen calculates absolute and relative (percent) change 
            4. T-test tests change in absolute and percent change by letter 
            5. Melogit tests large percentage change (>20% decrease) by letter
            6. Merges with new patients and co-prescriptions 
            7. Generates variable for total number of new patients 
            8. Replaces clinicians with no co-prescriptions (missing) to zero
            9. Corr checks correlations between letter, no. new patients, and opioid co-prescriptions 
            10. By : summ calculates summary stats for no. new patiens and co-prescriptions by letter
            11. melogit tests >20% decrease by letter, controlling for no. new patient and co-prescriptions 
            12. Matrix extracts and puts results in matrices 
            13. Putexcel outputs to excel doc.     

    CONTACT
    For questions regarding code email Emily Stewart, epstewar@usc.edu. For questions regarding data access, email corresponding author Jason Doctor, jdoctor@usc.edu. 
