---
title: Create an application map
published: true
tags: #azure #logging #servicefabric
---

# Introduction

An Azure cloud offers a service - [Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview) - which is able to trace application logs, correlate them, show dependencies, etc. One of its many features is displaying an [Application Map](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-map). When platform consists of multiple microservices it is easier to analyze logs, when there is trace visualization. A Service Fabric platform implement RPC with its own framework. Here I want to show how to implement tracing requests through Service Fabric Remoting and display it on an Application Insights.

# Prerequisites

To fully understand this example, you need to be aware of:
* Service Fabric platform 
* Stateless Native service
* Remoting
* Application Insights

# Nugets

Most of the functionality can be achieved by installing nuget packages. Install following packages in your stateless service:
* Microsoft.ApplicationInsights
* Microsoft.ApplicationInsights.DependencyCollector
* Microsoft.ApplicationInsights.ServiceFabric.Native
* Microsoft.ServiceFabric.Services.Remoting

# Code changes

Add following lines to stateless service constructor.
```
TelemetryConfiguration.Active.TelemetryInitializers.Add(
    FabricTelemetryInitializerExtension.CreateFabricTelemetryInitializer(Context)
);
TelemetryConfiguration.Active.InstrumentationKey = instrumentationKey;
TelemetryConfiguration.Active.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
TelemetryConfiguration.Active.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

new DependencyTrackingTelemetryModule().Initialize(TelemetryConfiguration.Active);
new ServiceRemotingRequestTrackingTelemetryModule().Initialize(TelemetryConfiguration.Active);
new ServiceRemotingDependencyTrackingTelemetryModule().Initialize(TelemetryConfiguration.Active);
```

# Results

After that, you can run your app and view traces on an Application Insights. Each service will have its own representation. All calls can be tracked through all properly configured services.
![img](https://raw.githubusercontent.com/meanin/service-fabric-request-correlation/master/img/app-map.jpg)
Furthermore, a single call to public API can be traced as an end to end transaction through all services.
![img](https://raw.githubusercontent.com/meanin/service-fabric-request-correlation/master/img/e2e-transaction.jpg)
