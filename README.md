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
Ticket ID	Unique ticket identifier
Created Timestamp / Resolved Timestamp	When the ticket was opened and closed (blank if still open)
Region	Northeast, Southeast, Midwest, West, Texas
Issue Category	Internet Outage, Slow Speeds, Billing, Equipment, New Install, TV/Video, Mobile
Channel	Phone, Chat, Email, Field Visit
Priority	High, Medium, Low (sets the SLA target)
Agent ID / Agent Tenure	Agent and experience group (0-6 months, 6-24 months, 2+ years)
SLA Target Hours / Resolution Hours / SLA Met	Target, actual time, and whether the target was met
First Contact Resolution	Resolved on first contact (Yes/No)
Repeat Contact 7 Days	Customer contacted again within 7 days about the same issue
Escalated	Escalated to a senior team
CSAT Score	Survey score 1-5 (blank if no survey returned)
Status	Open or Closed as of Sep 30, 2026
Process
Audit the raw data using Power Query's column profiling.
Clean in Power Query (every step is recorded in Applied Steps, so it's repeatable).
Add helper columns (month, year, SLA Flag, FCR Flag) to make rates easy to average.
Build pivot tables and KPI formulas in Excel.
Chart and assemble the dashboard, then write findings and a recommendation.
Document KPI definitions and data quality decisions in the workbook.
Data quality issues found and how I handled them
Issue	Rows	What I did	Why
Duplicate Ticket IDs	40	Removed, keeping one row per Ticket ID	Duplicates would inflate ticket counts and skew every rate
Inconsistent Region labels (e.g. "NE", "TX", stray spaces and capitalization)	~60	Standardized to the five region names	Variants split one region into several groups in a pivot
Resolution Hours of -1	~15	Replaced with null; SLA result left blank for those rows	A ticket can't take -1 hours
Open tickets with no resolution time	~20	Kept; skipped by time and SLA averages	They're real tickets that aren't resolved yet
Blank CSAT scores	~2,050 (about a third)	Left blank	The customer didn't return the survey; filling a score would invent data. Avg CSAT only reflects customers who answered
Resolved time earlier than created time	25	Identified and left in	Real tickets; SLA and resolution time use the Resolution Hours column, so those KPIs are unaffected. In a real setting I'd send these rows to the data owner to correct
Limitations
The data is synthetic, so the patterns illustrate analysis methods and are not real-world findings.
The dashboard shows patterns, not causes. For example, tenure measures time in the role, not prior experience elsewhere.
The 25 rows with contradictory timestamps remain in the data. Any calculation built by subtracting the two timestamps would be wrong for those rows.
Tools

Excel (pivot tables, pivot charts, AVERAGEIFS and COUNTA formulas), Power Query.

Files
├── README.md
├── images/
│   └── dashboard.png
└── service_ops_dashboard.xlsx
Next steps
Rebuild the same analysis in SQL (joins, aggregations and window functions).
Split the 7-day Southeast spike by hour and channel.
Test whether a realistic High priority target changes the SLA picture.
