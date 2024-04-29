# AWS main services event monitoring

## EC2/EBS

[Best Practices](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring_best_practices.html)

### Wrokflow
![EC2/EBS](./picture/aws-ec2-ebs-events.png)

[Scheduled Events](https://repost.aws/knowledge-center/eventbridge-notification-scheduled-events)


[Other EC2 Events](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/automating_with_eventbridge.html)

[EBS events](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-cloud-watch-events.html)

### Event related to availability

| event type category | event ID |
| :--- | :--- |
|scheduledChange    |*         |

## RDS

[RDS Monitoring](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Monitor_Logs_Events.html)

[RDS events](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-cloudwatch-events.sample.html)

### Workflow
RDS -> Eventbridge -> rule -> action

Or

RDS -> cluster -> event notification -> sns -> action

### Event realted to availability

| detail-type               | EventCategory    | EventID   |
| :--- | :--- | :--- |
| RDS DB Cluster Event      | *                | *         |
| RDS DB Instance Event     | *                | *         | 
| RDS DB Parameter Group Event     | *         | *         | 
| RDS DB Security Group Event      | *         | *         | 
| RDS DB Snapshot Event     | *                | *         |
| RDS DB Cluster Snapshot Event     | *        | *         |
| RDS DB Proxy Event     | *          | *      | *
| RDS Blue Green Deployment Event     | *      | *         |



## Elasticache Redis
[Elasticache events](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/self-designed-events.html)


### Workflow
Elasticache Redis -> Cluster -> Topic for SNS Notification -> sns -> action

*Self-designed cluster events are not published to Amazon EventBridge.*

## Amazon OpenSearch Service
[Monitoring event](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/monitoring-events.html)

### Workflow
AOS -> eventbridge -> rule -> action

## Kafka

### Workflow
MSK -> Health API -> Eventbridge -> rule -> action

### Event
| detail-type   | source    | detail -> service |
| :--- | :--- | :--- |
| AWS Health Event | aws.health | KAFKA |

example: https://github.com/zhang1980s/aws-event-bot/blob/main/test/health-msk.json