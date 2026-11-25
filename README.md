NHS A&E Performance Analysis (Excel and Power Query)

An analysis of NHS England's monthly A&E waiting time data, built in Excel with Power Query. It combines six monthly files into one clean table, validates the main performance measure against the published national figure, and ranks hospital A&E departments by how many patients were seen within four hours.


Status: Week 1 complete (data pipeline, measure, rankings). Pivot tables and a one-page summary are coming next. 


The question
Which hospital A&E departments perform best and worst on the four-hour standard, and does the answer change depending on how you measure it?
The data
Source: NHS England, A&E Attendances and Emergency Admissions monthly statistics (provider level). (https://www.england.nhs.uk/statistics/statistical-work-areas/ae-waiting-times-and-activity/ae-attendances-and-emergency-admissions-2026-27/).
Coverage: six monthly files, April 2026 to September 2026.
Size: 1,153 rows after combining, including 6 national total rows. 1,147 rows once totals were removed.
Licence: licensed under the [Open Government Licence v3.0 (https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).

The original files are in `data/raw/` and were never edited.

What I did
Combined six monthly CSV files into one table using Power Query (Get Data, From Folder), so a new month can be added by refreshing.
Built a proper date. The Period column held text like `MSitAE-JULY-2026`. I split it and rebuilt it as a real date so months sort and chart correctly.
Separated the total rows. Each file includes a national total row. Left in, they would double count every sum, so I moved them to their own sheet (`AE_Totals`) and used them for checking.
Shortened and typed the columns (counts as whole numbers, names as text).
Built the measure. Four-hour performance = 1 minus (attendances over 4 hours divided by total attendances). Percentages are calculated from summed counts, never by averaging hospital percentages.
Validated against the published figure. My July result was 75.37% against the published 75.4%. Getting that match showed that booked appointment attendances are reported separately from Type 1, Type 2 and Other, so the totals add all six attendance columns.
Ranked hospitals for September 2026, using a minimum of 5,000 attendances so very small departments do not distort the ranking.
Tested the ranking under different definitions (all attendance types versus Type 1 only) and investigated why trusts moved.

What I found
Some providers in the file are not hospital A&Es. 13 providers had no Type 1 (major A&E) department. They included urgent care centres, community health services, a GP practice, private surgical centres and an eye hospital. Because they see mostly minor cases, their four-hour figures are high. I excluded them from hospital comparisons.
On Type 1 attendances, Alder Hey, Calderdale and Huddersfield, and George Eliot Hospital ranked first, second and third. Alder Hey is a children's hospital, so it is not directly comparable with adult A&Es. Northumbria Healthcare ranked fifth.
The measure you choose changes the ranking. North Tees and Hartlepool ranked 24th on all attendance types but 115th on Type 1 only. Only 31% of its attendances are Type 1, so its faster urgent care services lifted its overall figure.
The lowest three on Type 1 in September were University Hospitals Plymouth, North Tees and Hartlepool, and Kingston and Richmond.
![Rankings table](images/rankings.png)


Files/Sheets and	what they are:
`AE_Project1.xlsx`	The workbook. Sheets: `AE_Monthly` (cleaned data and calculated columns), 
`AE_Totals` (national totals used for checking), 
`Rankings` (comparison table), 
`Data notes` (the 13 non-hospital providers), 
`ReadMe.`
`data/raw/`	The six original monthly CSV files, unedited
`images/`	Screenshots used in this README

Limitations
The ranking covers September 2026 only, so a trust's position could be a single bad month.
The 5,000-attendance cut-off is a judgement call.
A ranking is not a measure of quality of care. Trusts differ in size, patient mix and pressure.
Next steps
Rebuild the analysis with pivot tables and write a one-page summary
Compare each trust's performance across all six months to separate consistent results from one-off dips
Tools
Excel, Power Query, SUMIFS, and COUNTIFS
