Customer Service Operations Dashboard (Excel + Power Query)

An end-to-end analysis of 6,000 customer support tickets: cleaning messy data in Power Query, building KPIs and pivot tables in Excel, and turning the results into a one-page dashboard with findings and a recommendation.

Note: The dataset is practice data, generated for this project. It does not come from any real company. Some data quality problems were included on purpose so I could practice finding and documenting them.

The question

Where is a customer service operation falling short on its service level agreements (SLAs), and what does the data suggest about why?

Key findings
Southeast outage spike. The Southeast had 252 internet outage tickets in March, mostly between March 9 and 15, about 10x the roughly 25 each in the other regions. Only 24.2% of those tickets met their SLA.
High priority tickets almost never meet their target. Only 9.4% of High priority tickets are resolved within the 4-hour target, compared with 70.4% for Medium (24-hour target) and 92.1% for Low (72-hour target).
Experience matters for first contact resolution. The share of tickets resolved on first contact rises from 57.3% for agents with 0-6 months of experience, to 68.9% for 6-24 months, to 79.6% for 2+ years.

Recommendation: Prioritize training and mentoring for newer agents, and review staffing and whether the 4-hour High priority target is realistic. These are patterns in the data, not proof of cause.

KPIs
KPI	Value	How it's calculated
Total Tickets	6,000	Count of Ticket ID
Avg SLA Rate	64.5%	Average of SLA Flag (1 = met target, 0 = missed)
First Contact Resolution	70.2%	Average of FCR Flag (1 = resolved on first contact)
Avg Resolution Time (hrs)	26.1	Average of Resolution Hours (open tickets skipped)
High Priority SLA Rate	9.4%	AVERAGEIFS of SLA Flag where Priority = High
Southeast Outage SLA (Mar)	24.2%	AVERAGEIFS of SLA Flag where Region = Southeast, Issue = Internet Outage, Month = 3

SLA targets by priority: High = 4 hours, Medium = 24 hours, Low = 72 hours.

Dataset

data/service_tickets.csv holds about 6,000 tickets from January to September 2026 (6,040 rows before duplicates were removed).

Column	Description
Dataset Columns
- **Ticket ID:** Unique ticket identifier
- **Timestamps:** Ticket creation and resolution times
- **Region:** Northeast, Southeast, Midwest, West, Texas
- **Issue Category:** Type of customer issue
- **Channel:** Phone, chat, email, or field visit
- **Priority:** High, Medium, Low
- **Agent ID / Tenure:** Agent identifier and experience group
- **SLA Fields:** Target hours, resolution hours, and SLA status
- **First Contact Resolution:** Whether resolved on first contact
- **Repeat Contact:** Whether the customer contacted again within 7 days
- **Escalated:** Whether escalated to a senior team
- **CSAT Score:** Customer satisfaction rating (1–5)
- **Status:** Open or closed as of September 30, 2026

Process
1. Audited data using Power Query column profiling.
2. Cleaned and standardized data using recorded Applied Steps.
3. Created month, year, SLA, and FCR helper columns.
4. Built pivot tables, KPI formulas, and charts in Excel.
5. Created a one-page dashboard with findings and recommendations.
6. Documented KPI definitions and data quality decisions.

Data Quality
| Issue | Action |
|---|---|
| 40 duplicate ticket IDs | Removed duplicates |
| ~60 inconsistent region labels | Standardized region names |
| ~15 invalid resolution times (-1 hours) | Replaced with null; excluded from relevant calculations |
| ~20 open tickets without resolution times | Retained; excluded from applicable averages |
| ~2,050 missing CSAT scores | Left blank to avoid inventing data |
| 25 contradictory timestamps | Documented and retained; used existing resolution hours for calculations |

Limitations
- Synthetic data; findings do not represent a real company.
- Results show associations, not proven causes.
- Agent tenure does not account for prior experience.
- Contradictory timestamps remain and should be corrected before any timestamp-based calculations.
Tools

Excel (pivot tables, pivot charts, AVERAGEIFS and COUNTA formulas), Power Query.

Files
readme, Customer Service Operations Dashboard picture, Customer Service Operations Dashboard.xlsx


Next steps
Rebuild the same analysis in SQL (joins, aggregations and window functions).
Split the 7-day Southeast spike by hour and channel.
Test whether a realistic High priority target changes the SLA picture.
