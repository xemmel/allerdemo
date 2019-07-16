## Aller Teaching Azure Integration
### Morten la Cour

## Table of Content
1. [Event Grid Topic Demo](#event-grid-topic-demo)
2. [Create Event Grid Topic](#create-event-grid-topic)




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
