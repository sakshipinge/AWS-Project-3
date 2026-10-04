# AWS EC2 CloudWatch Monitoring with SNS

## 1. Project Title

AWS EC2 Web Server Monitoring Using Amazon CloudWatch and Amazon SNS

## 2. Objective

The objective of this project is to monitor an EC2 web server using Amazon CloudWatch and send email notifications using Amazon SNS when important monitoring conditions occur.

The project monitors:

* EC2 CPU utilization
* EC2 instance status checks
* CloudWatch alarms
* SNS email notifications

## 3. AWS Services Used

The following AWS services were used:

* Amazon EC2
* Amazon CloudWatch
* Amazon SNS
* AWS Security Groups

## 4. Architecture

The architecture of the project is:

EC2 Web Server
↓
Amazon CloudWatch
↓
CPU Utilization Alarm + Instance Status Alarm
↓
Amazon SNS
↓
Email Notification

## 5. EC2 Web Server Configuration

An Ubuntu EC2 instance was created and configured as the web server.

Apache HTTP Server was installed using:

```bash
sudo apt update
sudo apt install apache2 -y
```

Apache was started using:

```bash
sudo systemctl start apache2
```

Apache was enabled using:

```bash
sudo systemctl enable apache2
```

The Apache service was verified using:

```bash
sudo systemctl status apache2
```

The service showed:

```text
Active: active (running)
```

## 6. SNS Configuration

An SNS topic was created with the name:

```text
EC2-Monitoring-Alerts
```

An email subscription was added to the SNS topic.

The subscription was confirmed through the confirmation email received from Amazon SNS.

## 7. CloudWatch CPU Alarm

A CloudWatch alarm was created to monitor EC2 CPU utilization.

Configuration:

* Metric: CPUUtilization
* Statistic: Average
* Period: 5 minutes
* Condition: Greater than 70%
* Alarm Name: EC2-High-CPU-Alarm
* Notification: EC2-Monitoring-Alerts SNS topic

The purpose of this alarm is to detect high CPU utilization on the EC2 instance.

## 8. CloudWatch Instance Status Alarm

A second CloudWatch alarm was created to monitor the EC2 instance status.

Configuration:

* Metric: StatusCheckFailed
* Statistic: Maximum
* Period: 5 minutes
* Condition: Greater than 0
* Alarm Name: EC2-Instance-Status-Alarm
* Notification: EC2-Monitoring-Alerts SNS topic

This alarm is designed to detect EC2 instance status check failures.

## 9. Testing

The CPU alarm was tested by generating temporary CPU activity on the EC2 instance.

The following command was used:

```bash
stress-ng --cpu 2 --timeout 10m
```

The CPU utilization was monitored using Amazon CloudWatch.

When the configured threshold was reached, the CPU alarm changed to the ALARM state.

## 10. SNS Notification Testing

When the CPU alarm entered the ALARM state, Amazon SNS generated an email notification to the confirmed email subscription.

The notification was verified in the email inbox.

## 11. Alarm Recovery

After the CPU test completed, CPU utilization decreased.

The CloudWatch CPU alarm eventually returned to the OK state after CloudWatch evaluated the new metric data.

## 12. Testing Results

| Test             | Expected Result                  | Result     |
| ---------------- | -------------------------------- | ---------- |
| Apache service   | Running                          | Passed     |
| CPU monitoring   | CPU metric available             | Passed     |
| CPU alarm        | Alarm when CPU exceeds threshold | Passed     |
| Status alarm     | Detect status check failure      | Configured |
| SNS subscription | Email subscription confirmed     | Passed     |
| SNS notification | Email received during alarm      | Passed     |
| Alarm recovery   | Alarm returns to OK              | Passed     |

## 13. Screenshots

Project screenshots are available in the `screenshots` folder.

Important screenshots include:

1. EC2 instance running
2. Apache service running
3. SNS topic
4. Confirmed SNS subscription
5. CPU alarm configuration
6. CPU alarm in OK state
7. Instance status alarm configuration
8. Both CloudWatch alarms
9. CPU stress test
10. CPU alarm in ALARM state
11. SNS email notification
12. CPU alarm returning to OK state

## 14. Security Configuration

The EC2 Security Group was configured with the required inbound rules.

SSH access was restricted to the user's IP address.

HTTP access was allowed on port 80 for the web server.

No unnecessary ports were opened.

## 15. Result

The EC2 web server was successfully monitored using Amazon CloudWatch.

Two CloudWatch alarms were configured for CPU utilization and instance status checks. The alarms were connected to an Amazon SNS topic, which provided email notifications during alarm events.

The project successfully demonstrates AWS monitoring and alerting using EC2, CloudWatch, and SNS.

## 16. Conclusion

This project demonstrates how Amazon CloudWatch can be used to monitor an EC2 web server and how Amazon SNS can be used to send notifications when monitoring conditions are triggered.

The implementation provides a basic monitoring and alerting solution for an AWS-hosted web server.
