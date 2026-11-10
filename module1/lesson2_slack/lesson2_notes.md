# Lesson 2 Homework Notes — Seven Alerts, Seven Wins

## Homework 1: Hello Slack
I edited the Python script to include my name and successfully sent a custom INFO alert from my EC2 instance to Slack. The message appeared in the Slack channel as expected. I learned how the `send_alert()` function uses arguments to build a message and how an f-string inserts my name into that message.

## Homework 2: Severity Tour
I ran the Python script and successfully sent all four alert levels to Slack: INFO, WARNING, ERROR, and CRITICAL. All four arrived because the `.env` threshold was set to INFO. I learned how a `for` loop can send multiple alerts and how the `.env` configuration filters them without changing the script.

**Question: Which severities arrived, and why?** With the threshold set to INFO, all four alerts arrived. I changed the threshold to ERROR and received only ERROR and CRITICAL, as expected. The `.env` setting determines the minimum severity allowed, so lower-priority alerts are filtered before delivery.

## Homework 3: Server Time
I ran the script and successfully sent my EC2 server's date and time to Slack. The `datetime` module retrieved and formatted the UTC timestamp. I learned why servers commonly use UTC to keep records consistent across regions.

**Question: How many hours apart are server time and my wall clock?** Taiwan is eight hours ahead of UTC. The server reported 15:54 UTC, which was 23:54 in Taiwan.

## Homework 4: Disk Usage Monitoring
I ran the disk monitoring script and received an INFO alert in Slack. My EC2 instance showed 25.4% disk usage with 5.9 GB free. Since usage was below the 75% WARNING threshold, the script selected INFO. I practiced using system measurements and `if/elif/else` statements to choose an alert severity.

**Question: Why is a quiet INFO good design, not a failure?** Normal system conditions shouldn't interrupt engineers unnecessarily. Filtering routine INFO alerts helps reduce alert fatigue so important warnings and errors get attention. My INFO alert was delivered during this test because INFO alerts were enabled.

## Homework 5: CPU Load Monitoring
I ran the script to check the CPU load on my EC2 instance. The load averages were 0.00, 0.01, and 0.00 for the last 1, 5, and 15 minutes on a server with two CPUs. The script chose INFO because the 5-minute load was below the CPU count. I learned how to retrieve load averages and compare them against a limit.

**Question: What would make this number spike on the Balerica Bank?** Heavy transaction volume, many simultaneous customer requests, a large batch job, or a malfunctioning application could increase the load. Sustained higher load would be a reason to investigate server performance.

## Homework 6: Incident Drill
I ran the simulated incident script and it sent a WARNING alert to Slack. The selected scenario was a SWIFT traffic generator queue with 42 messages waiting, labeled DRILL-6891. I practiced working with a list of incidents, `random.choice()`, and a randomly generated incident ID. The messages were synthetic, not real bank incidents.

**Question: Which fake incident deserves to wake a human at 3 AM?** The CRITICAL incident where `account_lookup` cannot reach the ledger file would be my first choice because it could disrupt core banking functions. Payment failures might also warrant an immediate response if they are widespread or ongoing. I would use customer impact and urgency, not just the severity label, to decide.

## Homework 7: Morning Report Generator
I ran the final Python script and successfully delivered a combined monitoring report to Slack. It included the server's UTC timestamp, hostname, operating system, Python version, disk usage, and load average. The report showed Linux x86_64, Python 3.9.25, 25.4% disk usage, and a 1-minute load average of 0.00. I learned how to combine several Python modules into one script that creates a readable server status report. The report confirms the checks ran, but doesn't independently establish that every value is healthy.

## Experiment 1: Severity Threshold Prediction vs. Result
I tested the severity filter with three settings in `.env` and checked the messages that arrived in Slack:

| `SLACK_ALERT_SEVERITY` | Predicted alerts | Actual alerts received |
| --- | --- | --- |
| `INFO` | 4 (INFO, WARNING, ERROR, CRITICAL) | 4 (INFO, WARNING, ERROR, CRITICAL) |
| `ERROR` | 2 (ERROR, CRITICAL) | 2 (ERROR, CRITICAL) |
| `CRITICAL` | 1 (CRITICAL) | 1 (CRITICAL) |

**Required prediction vs. result:** I predicted that setting the threshold to CRITICAL would let only one of the four messages through. When I ran `hw2_severity_tour.py`, only the CRITICAL message appeared in Slack. My prediction matched the result. This showed me that `.env` configuration can change alert behavior without modifying the Python script.

## Experiment 2: Alert Kill Switch
I also tested `SLACK_ALERTS_ENABLED=false` and completed the alert-disable experiment. The purpose was to see how a kill switch can prevent alerts from being sent during maintenance or other planned work. I returned `SLACK_ALERTS_ENABLED` to `true` afterward. I did not record the terminal output or exact number of messages for this test in these notes.

## Experiment 3: Disk Usage Warning Threshold
I changed the WARNING threshold in `hw4_disk_watch.py` from 75% to 20%. My EC2 instance was using 25.4% of its disk space, so I expected the alert to change from INFO to WARNING. After rerunning the script, Slack received a WARNING with 25.4% used and 5.9 GB free. The result matched my prediction and showed how an `if/elif/else` rule changes a monitor's response. I restored the WARNING threshold to 75% afterward.

## Experiment 4: Restore Configuration
After testing, I restored the normal settings:

```dotenv
SLACK_ALERT_SEVERITY=INFO
SLACK_ALERTS_ENABLED=true
```

I also restored the disk WARNING threshold in `hw4_disk_watch.py` to 75% (with ERROR at 90%).

## Evidence Checklist
- [x] Slack screenshot showing at least five different homework alerts (alerts from all seven scripts were shown across my screenshots).
- [x] Terminal screenshot showing the `hw7_morning_report.py` report and `whoami && TZ='Asia/Taipei' date && ec2-metadata -i`.
- [x] Answers to each script's "In your notes" question where provided.
- [x] Experiment 1 prediction and actual CRITICAL-threshold result recorded.
- [x] Alert-disable experiment completed and configuration restored.
- [x] Disk WARNING threshold experiment verified in Slack and original threshold restored.

**Terminal evidence:** The identification command showed `ec2-user`, Wednesday November 11, 2026 at 00:05:04 (Taipei time), and instance ID `i-0d0bc81fc1a4d5b87`.

**Submission reminder:** Commit and push this notes file and the required screenshots to the homework repository.

