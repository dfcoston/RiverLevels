# RiverLevels
This project is a sample of a personal project I made to help me and my friends know when to float various rivers of Western Montana.  The end result is a tableau graph, charting various rivers by month year over year.

Underlying Technologies (keep in mind, all this was done with free resources)
DB: PostgreSQL
Language: C#
Simple REST call to collect HTTP delivered CSV data per river from: https://waterdata.usgs.gov

I first created a DIM table in the DB that houses the USGS site number of the desired rivers, and a site number (given by the USGS for each river) and a friendly name for each river (displayed in the Tableau Graph) - For example: SiteNumber: 12363000, River Name: North Fork Flathead River.  Then initated a dynamic REST call that loops through each river and dynamically generates the needed URL:https://waterdata.usgs.gov/nwis/uv?cb_00010=on&cb_00060=on&cb_00065=on&cb_63160=on&format=rdb&site_no=<site_number>&period=&begin_date=YYYY-MM-DD&end_date=YYYY-MM-DD

Running the C# script will query the DB for the most recent recorded date in a river_level fact table.  The script then dynamically generates a URL for each river, that will query the USGS website for all missing data between the current date and the last recorded datetime recorded in the database for that river.  A loop is used to iterate through querys that are too long for the USGS website (6 months is the max).

I then have built a view to aggrigate the 15 minute data intervals for each river into a daily average for each river.  Metrics include: cubic feet per second, gauge height, and temperature.  However, due to weather conditions (ie ICE), some metrics will not be available.

Long story short, the output is a graph that makes judging river flow in relation to previous years possible.

https://public.tableau.com/profile/derrick.coston#!/vizhome/MTRivers/dashboard
