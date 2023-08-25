# EventGridFASample

Using this PowerShell Script and Ngrok we are able to reproduce and the trigger using JSON content

```PowerShell
$json = @"
{
  "id": "5ED8D56C-6121-454F-9FF6-9709E0F97B59",
  "topic": "",
  "subject": "",
  "eventType": "",
  "data": {
    "twin": {
      "deviceId": "",
      "etag": "",
      "deviceEtag": null,
      "status": "",
      "statusUpdateTime": "",
      "connectionState": "",
      "lastActivityTime": "",
      "cloudToDeviceMessageCount": 0,
      "authenticationType": "",
      "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
      },
      "modelId": "",
      "version": 1,
      "properties": {
        "desired": {
          "$metadata": {
            "$lastUpdated": ""
          },
          "$version": 1
        },
        "reported": {
          "$metadata": {
            "$lastUpdated": ""
          },
          "$version": 1
        }
      }
    },
    "hubName": "",
    "deviceId": ""
  },
  "dataVersion": "",
  "metadataVersion": "1",
  "eventTime": "2020-01-02T01:06:55.3670057Z"
}
"@



$headers = @{
    "Content-Type" = "application/json"
    "aeg-event-type" = "Notification"
}


$url = "https://0320-2600-1700-3650-dad0-c426-b6b7-7060-5acd.ngrok-free.app/runtime/webhooks/EventGrid?functionName=EventGrid1"
Invoke-RestMethod -Method 'Post' -Uri $url -Body $json -ContentType "application/json" -Headers $headers
```

### The Function App Code (In-Process) is using EventGridEvent

```Csharp
using System;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Extensions.Logging;
using Azure.Messaging.EventGrid;

namespace EventGridInProc
{
    public static class EventGrid1
    {
        [FunctionName("EventGrid1")]
        public static void Run([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}

```

### Results from the Test Event Grid Execution
![image](https://github.com/macavall/EventGridFASample/assets/43223084/00bd8147-e325-4641-8bdc-cf7e86755d4b)



