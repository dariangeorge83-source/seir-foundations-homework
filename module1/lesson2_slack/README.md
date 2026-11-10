# Lesson 2: Slack Monitoring and Alerts

## Overview
This assignment focused on using Python scripts to monitor an AWS EC2 instance and send automated alerts to Slack. I completed seven scripts covering alert severity, server time, disk usage, CPU load, simulated incidents, and automated reporting.

## Completed Tasks
1. Hello Slack: Sent a custom INFO alert.
2. Severity Tour: Tested INFO, WARNING, ERROR, and CRITICAL alerts.
3. Server Time: Reported the EC2 server's UTC time.
4. Disk Watch: Monitored disk usage and assigned severity levels.
5. CPU Load Check: Monitored CPU load averages.
6. Incident Drill: Generated simulated banking incidents.
7. Morning Report: Combined multiple server metrics into one report.

## Experiments
- Tested alert filtering with INFO, ERROR, and CRITICAL thresholds.
- Tested disabling Slack alerts using the `.env` configuration.
- Modified the disk usage WARNING threshold and verified the change.
- Restored the original settings after testing.

## Evidence
The `screenshots/` folder contains:
- Slack screenshot showing multiple homework alerts.
- Terminal screenshot showing the morning report and EC2 identification.

## Reflections
See `lesson2_notes.md` for individual script reflections, homework question answers, and experiment results.
