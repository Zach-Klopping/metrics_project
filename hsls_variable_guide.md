# HSLS variables for family background, educational beliefs, and college attendance

Source: [HSLS codebook](HSLS%20Data/Codebook_HSLS_2009-16_PETS_Pear.txt). The main candidates below are also present in the student CSV, `HSLS Data/hsls09_16_student_pets_pear_v1_0.csv`.

## Study timing

| Prefix | Study period |
| --- | --- |
| `1` | 2009 base year, when students were ninth graders |
| `2` | 2012 first follow-up |
| `3` | 2013 update and high school transcripts |
| `4` | 2016 second follow-up |
| `6` | Administrative records through June 2021 |

The period in which a variable was collected can differ from its reference period. For example, baseline family income refers to 2008, and grade-specific transcript GPAs refer to the relevant school grade.

## 1. Parental income

These variables measure total family income, including sources beyond parents' wages.

| Variable | What it measures |
| --- | --- |
| `X1FAMINCOME` | Family income from all sources in 2008; recommended baseline income measure |
| `X2FAMINCOME` | Family income from all sources in 2011 |
| `X1POVERTY`, `X2POVERTY` | Family income relative to the Census poverty threshold |

The family income variables have 13 brackets, ranging from $15,000 or less to more than $235,000. Treat them as categories rather than dollar amounts. The raw continuous measures, `P1INCOME` and `P2INCOME`, are suppressed in this public-use file.

## 2. Parental education

| Variable | What it measures |
| --- | --- |
| `X1PAREDU` | Highest education attained by either parent living in the household |
| `X1MOMEDU` | Mother's education |
| `X1DADEDU` | Father's education |
| `X1PAR1EDU`, `X1PAR2EDU` | Education for each identified parent; parent numbers do not designate gender |
| `X2PAREDU`, `X2MOMEDU`, `X2DADEDU` | Corresponding first-follow-up measures |

**Recommended starting measure: `X1PAREDU`.** Its categories distinguish less than high school, high school/GED, associate's, bachelor's, master's, and advanced professional/doctoral degrees.

The categories change between 2009 and 2012: a bachelor's is coded 4 in `X1PAREDU` and 5 in `X2PAREDU`. Zero in the mother/father variables means that parent is absent from the household, rather than having no education.

## 3. Students' beliefs about education

These variables capture distinct dimensions of educational beliefs.

| Dimension | Variables | What they capture |
| --- | --- | --- |
| Expected attainment | `X1STUEDEXPCT`, `X2STUEDEXPCT` | How far students think they will get in school |
| College-going confidence | `S1SURECLG` | Certainty of pursuing a bachelor's |
| Perceived academic barrier | `S1GETINTOCLG` | Belief that even with studying, they cannot get into college |
| Perceived financial barrier | `S1AFFORD` | Belief that their family cannot afford college |
| Value of college versus work | `S1WORKING` | Agreement that working is more important than attending college |
| Value of school | `S1SCHWASTE` | Agreement that school is often a waste of time |
| Importance of grades | `S1GOODGRADES` | Agreement that getting good grades is important |
| Expected financial returns | `X2S2EARNHS`, `X2S2EARN2YPUB`, `X2S2EARN4Y` | Expected annual starting earnings with a high school diploma, two-year degree, or four-year degree |

A useful constructed measure for an economics project is:

```text
Perceived college earnings premium = X2S2EARN4Y - X2S2EARNHS
```

This measures the additional annual starting earnings a student expects from a four-year degree. Both input variables are already converted to annual wages. Remove special missing-value codes before calculating the difference.

The listed agreement questions use 1 = strongly agree through 4 = strongly disagree. Higher values therefore indicate less agreement with each statement. `S1SURECLG` uses 1 = very sure about going through 4 = very sure about not going, and has substantial legitimate skips.

The attainment categories in `X1STUEDEXPCT` and `X2STUEDEXPCT` also differ across waves and include a positive code for "Don't know." Harmonize their substantive categories before comparing them over time.

## 4. Eventual college attendance

| Variable | What it measures |
| --- | --- |
| `X4EVRATNDCLG` | Ever attended college or trade school after high school by February 2016; imputed version, coded 0/1 |
| `S4EVRATNDCLG` | Underlying survey response |
| `X4PS1LEVEL` | First institution's level: four-year, two-year, or less than two-year |
| `X6PSE` | Any known postsecondary enrollment through June 2021, combining survey and administrative sources |
| `X6ENRPSE2YR`, `X6ENRPSE4YR` | Ever enrolled at a two-year or four-year institution through June 2021 |
| `X6PHSSTART` | First known postsecondary attendance date after high school |

**Recommended initial outcome: `X4EVRATNDCLG`.** It explicitly excludes enrollment during high school. Its definition includes trade school attendance, so it is broader than attendance at two-year or four-year colleges alone.

For the longer horizon, `X6PSE` is useful, but 0 means "no known enrollment." To measure attendance specifically after high school, examine it alongside `X6DUALDTFLG`, which distinguishes dual-enrollment-only attendance. Do not recode missing values in the institution-level indicators as nonattendance.

The public-use version of `X6PHSSTART` recodes the month to July, so it supports year-level timing rather than precise month-level timing.

## 5. Student performance in high school

| Variable | What it measures |
| --- | --- |
| `X3TGPATOT` | Overall transcript-based GPA, on a 0–4 scale |
| `X3TGPAWGT` | Overall honors-weighted GPA |
| `X3TGPA9TH`, `X3TGPA10TH`, `X3TGPA11TH`, `X3TGPA12TH` | GPA by grade |
| `X3TGPAACAD` | GPA in academic courses |
| `X3TGPAMAT`, `X3TGPASCI` | Math and science GPA |
| `X1TXMTSCOR`, `X2TXMTSCOR` | Standardized math assessment scores in 2009 and 2012 |
| `X3TCREDAPIB` | AP/IB credits earned; a measure of advanced coursework |

**Recommended starting measures: `X3TGPATOT` for overall achievement and `X1TXMTSCOR` for baseline math performance.** The math score uses a reference mean of 50 and standard deviation of 10. SAT/ACT composite variables exist but are suppressed in this public-use file.

## Suggested initial variable set

| Role | Variables |
| --- | --- |
| Family background | `X1FAMINCOME`, `X1PAREDU` |
| Educational beliefs | `X1STUEDEXPCT`, `S1AFFORD`, `S1GETINTOCLG`, `S1WORKING` |
| Academic performance | `X1TXMTSCOR`, `X3TGPATOT` |
| Attendance outcome | `X4EVRATNDCLG` |
| Student identifier | `STU_ID` |

Keep relevant imputation flags, including `X1FAMINC_IM`, `X1PAREDU_IM`, `X1STUEDEX_IM`, and `X4EVRATNDCLG_IM`. Distinguish missing responses, unit nonresponse, suppression, and legitimate skips using each variable's codebook entry before estimating anything. Select survey weights and variance estimation methods appropriate to the waves and respondents included in the analysis.

Expectations and later GPA may be pathways through which family background affects attendance. Including them as controls changes the interpretation of the income and education coefficients. Baseline math performance and ninth-grade GPA offer earlier measures of achievement, although measurements within the same school year do not necessarily establish temporal ordering.
