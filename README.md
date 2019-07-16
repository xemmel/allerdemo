## Aller Teaching Azure Integration
### Morten la Cour

## Table of Content
1. [Event Grid Topic Demo](#event-grid-topic-demo)
2. [Create Event Grid Topic](#create-event-grid-topic)
3. [Create a Subscription](#create-a-subscription)
4. [Submit Events](#submit-events)
5. [Api Management](#api-management)
19. [List Resources](#list-resources)
20. [Clean up](#clean-up)

## Event Grid Topic Demo


### Create Resource Group
```powershell

Clear-Host
$rg = New-AzResourceGroup -Name aller_eventgrid_demo -Location westeurope

```

[Back to Top](#table-of-content)



### Create Event Grid Topic
```powershell

Clear-Host
$eventgrid_topic = New-AzEventGridTopic -ResourceGroupName $rg.ResourceGroupName -Name mytopic1314 -Location $rg.Location

$event_topic_url = $eventgrid_topic.Endpoint;
$event_topic_key = ($eventgrid_topic | Get-AzEventGridTopicKey).Key1

```

[Back to Top](#table-of-content)


### Create a Subscription

#### A simple Subscription (GET-All)

```powershell 

Clear-Host
$endpoint = "https://enuswcsnq5qr.x.pipedream.net/";
New-AzEventGridSubscription  -ResourceGroupName $rg.ResourceGroupName -TopicName $eventgrid_topic.TopicName `
 -EventSubscriptionName subscriptionsimple -Endpoint $endpoint

```

#### Only orderEvent Subscription

```powershell 

Clear-Host
$endpoint = "https://en68sphu3z3tq.x.pipedream.net/";
New-AzEventGridSubscription  -ResourceGroupName $rg.ResourceGroupName -TopicName $eventgrid_topic.TopicName `
 -EventSubscriptionName subscriptionordersOnly -Endpoint $endpoint -IncludedEventType @("orderEvent")

```

#### Only orderEvents only certain Customers

```powershell

Clear-Host
$endpoint = "https://ensla6u2xaiqp.x.pipedream.net/";
New-AzEventGridSubscription  -ResourceGroupName $rg.ResourceGroupName -TopicName $eventgrid_topic.TopicName `
 -EventSubscriptionName subscriptionordersandcustomersOnly -Endpoint $endpoint -IncludedEventType @("orderEvent") `
 -AdvancedFilter @(@{operator="StringIn"; key="data.customer"; Values=@("Aller","Egmont") })


```


[Back to Top](#table-of-content)


### Submit Events

```powershell

Clear-Host
"Aller Egmont DS Coop IBM MS".Split() |
foreach {
    $body = @"
    [
        {
            "subject" : "testSubject",
            "eventType" : "testEvent",
            "id" : "$((New-Guid).Guid)",
            "eventTime" : "$((Get-Date).ToString("yyyy-MM-ddTHH:mm:ss"))",
            "data" : {
                "customer" : "$_"
            }
        },
            {
            "subject" : "testSubject",
            "eventType" : "orderEvent",
            "id" : "$((New-Guid).Guid)",
            "eventTime" : "$((Get-Date).ToString("yyyy-MM-ddTHH:mm:ss"))",
            "data" : {
                "customer" : "$_"
            }
        }
    ]
"@
    curl -Uri $event_topic_url -Method Post -Body $body -Headers @{"aeg-sas-key" = $event_topic_key}
}


```

[Back to Top](#table-of-content)

### List Resources

```powershell

Clear-Host
Get-AzResource -ResourceGroupName $rg.ResourceGroupName | select Name, ResourceType, ResourceGroupName

```

[Back to Top](#table-of-content)

### Clean up

> Be sure that $rg points at the **Resource Group** to be deleted!
```powershell

Clear-Host
Remove-AzResourceGroup -Name $rg.ResourceGroupName -Force

```

[Back to Top](#table-of-content)



### Api Management

#### Call Old SOAP Service

Url: http://xemmel.com/webservices/postnumber.asmx
Method: Post
Headers:

```
SOAPAction: 	http://tempuri.org/GetCity
Content-Type: 	text/xml
```
Body: 

```xml

<Envelope xmlns="http://schemas.xmlsoap.org/soap/envelope/">
	<Body>
		<GetCity xmlns="http://tempuri.org/">
    		<pPostNumber>4000</pPostNumber>
    	</GetCity>
	</Body>
</Envelope>

```

#### 

[Back to Top](#table-of-content)


