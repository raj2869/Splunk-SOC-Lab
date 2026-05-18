# Top 10 Failed Logon Attempts
1. source="brute_force_logss.csv" sourcetype="csv" | stats count by "Date and Time". "Event ID", Keywords | sort -count | head 10 | rename "Date and Time" AS "Login Time", "Event ID" AS "Event ID", count AS "Failed Attempts"

# Failed Attempts over time
2.source="brute_force_logss.csv" sourcetype="csv" | eval Hour=strftime(_time, "%H:00") | stats count AS "Attempts" by Hour | sort Hour

# Groups and counts events by logon details
3.source="brute_force_logss.csv" sourcetype="csv" | stats count by "EXTRA_FIELD_6" | rename EXTRA_FIELD_6 AS "Logon Details", count AS "Count" | sort -count

# Sorted Events by Time and Event Code
4.source="brute_force_logss.csv" sourcetype="csv" | table "Date and Time", "Event ID", Event | sort -"Date and Time" | rename "Date and Time" AS "Time", "Event ID" AS "Event Code", Event AS "Message"

# Total Brute Force Attempts
5.source="brute_force_logss.csv" sourcetype="csv" | stats count AS "Total Brute Force Attempts"
