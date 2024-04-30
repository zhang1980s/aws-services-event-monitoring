# AWS main services event monitoring

## EC2/EBS

[Best Practices](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring_best_practices.html)

### Wrokflow
![EC2/EBS](./picture/aws-ec2-ebs-events.png)

[Scheduled Events](https://repost.aws/knowledge-center/eventbridge-notification-scheduled-events)


[Other EC2 Events](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/automating_with_eventbridge.html)

[EBS events](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-cloud-watch-events.html)

### Event related to availability
Health API via Eventbridge

| event type category | event ID |
| :--- | :--- |
|scheduledChange    |*         |

### Eventbridge rule pattern

```
{
  "source": [
    "aws.health"
  ],
  "detail-type": [
    "AWS Health Event"
  ],
  "detail": {
    "service": [
      "EC2"
    ],
    "eventTypeCategory": [
      "scheduledChange"
    ]
  }
}
```


## RDS

[RDS Monitoring](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Monitor_Logs_Events.html)

[RDS events](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-cloudwatch-events.sample.html)

### Workflow
RDS -> Eventbridge -> rule -> action

Or

RDS -> cluster -> event notification -> sns -> action

### Event related to availability

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

### Eventbridge rule pattern

```
{
  "source": ["aws.rds"],
  "detail-type": ["RDS DB Cluster Event", "RDS DB Instance Event", "RDS DB Parameter Group Event", "RDS DB Security Group Event", "RDS DB Proxy Event"]
}
```

## Elasticache Redis
[Elasticache events](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/self-designed-events.html)


### Workflow
Elasticache Redis -> Cluster -> Topic for SNS Notification -> sns -> action

*Self-designed cluster events are not published to Amazon EventBridge.*

### Event pattern
https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeEvents.html

```
<DescribeEventsResponse xmlns="http://elasticache.amazonaws.com/doc/2015-02-02/"> 
    <DescribeEventsResult> 
        <Events> 
            <Event> 
                <Message>Cache cluster created</Message> 
                <SourceType>cache-cluster</SourceType> 
                <Date>2015-02-02T18:22:18.202Z</Date> 
                <SourceIdentifier>my-redis-primary</SourceIdentifier> 
            </Event> 
               
 (...output omitted...)
          
        </Events> 
    </DescribeEventsResult> 
    <ResponseMetadata> 
        <RequestId>e21c81b4-b9cd-11e3-8a16-7978bb24ffdf</RequestId> 
    </ResponseMetadata> 
</DescribeEventsResponse>

```

## Amazon MemoryDB for Redis
[Monitoring event](https://docs.aws.amazon.com/memorydb/latest/devguide/monitoring-events.html)

### Workflow
MemorydB -> cluster -> event notification -> sns -> action

### Event pattern
https://docs.aws.amazon.com/memorydb/latest/devguide/mdbevents.viewing.html

```
{
    "Events": [        
        {
            "Date": "2021-03-29T22:17:37.781Z", 
            "Message": "Added node 0001 in Availability Zone us-east-1a", 
            "SourceName": "memorydb01", 
            "SourceType": "cluster"
        }, 
        {
            "Date": "2021-03-29T22:17:37.769Z", 
            "Message": "cluster created", 
            "SourceName": "memorydb01", 
            "SourceType": "cluster"
        }
    ]
}
```



## Amazon OpenSearch Service
[Monitoring event](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/monitoring-events.html)

### Workflow
AOS -> eventbridge -> rule -> action

### Eventbridge rule pattern

```
"source": ["aws.es"],
```

## Kafka

### Workflow
MSK -> Health API -> Eventbridge -> rule -> action

### Event related to availability
| detail-type   | source    | detail -> service |
| :--- | :--- | :--- |
| AWS Health Event | aws.health | KAFKA |

example: https://github.com/zhang1980s/aws-event-bot/blob/main/test/health-msk.json

### Eventbridge rule pattern

```
{
    "detail-type": "AWS Health Event",
    "source": "aws.health",
    "detail": {
        "service": "KAFKA",
    }
}
```