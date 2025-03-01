---
title: "Logging In Native Apps"
url: /refguide/mobile/distributing-mobile-apps/logging/
weight: 39
description: "Describes using logging in native mobile apps"
tags: ["native", "logging", "troubleshooting"]
---
## 1 Introduction

In Mendix Studio Pro v9.16 and above native mobile apps are able to send logs to the [Mendix Runtime](/refguide/runtime/). Read this guide for information on native app logging configuration.

{{% alert color="warning" %}}
Please note the following current limitations regarding native client logs:

* Only the following log levels are supported currently: **Information**, **Warning**, **Critical**, and **Error**
* Crash logs are not supported currently
* Native client logs will not appear directly in the cloud portal logs overview before they are sent to [Mendix Runtime](/refguide/runtime/), for more information see [Sending Log Messages To Runtime](#sending-client-log-nodes-to-runtime)
{{% /alert %}}

## 2 Enabling Native App Logging

Sending logs from native apps is disabled by default. However, sending logs can be enabled from your [native phone profile](/refguide/navigation/#native-phone) by selecting the **Enable sending logs to runtime** checkbox inside the **Logging** group box.

{{% alert color="warning" %}}
Please note that after enabling or disabling sending logs to runtime, you must create and distribute a new build of the native mobile app for this change to take effect. For more information on creating and distributing builds, see [Building, Testing, and Distributing Apps](/refguide/mobile/distributing-mobile-apps/).
{{% /alert %}}

{{< figure src="/attachments/refguide/mobile/native-mobile/logging/enable-logging.png" >}}

## 3 Log Levels

{{% alert color="warning" %}}

* Please note that at the moment only `information, warning, critical, and error` log levels are supported (support for other log levels will be added in the future)
* `Crash` logs are not supported currently
{{% /alert %}}

For more information on log levels, see the [Log Levels](/refguide/logging/#log-levels) section of *Logging*.

### 3.1 Critical

**Critical** is reserved for rare cases where the application may not be able to function reliably anymore. This should normally not occur. If it does, you should immediately take action. Mendix Cloud v3 treats these messages as alerts and will notify you on the cloud dashboard.

### 3.2 Error

**Error** is used to log all unhandled exceptions. These are unexpected events that should not occur, but are not critical. The application should be able to function normally afterwards.

### 3.3 Warning

**Warning** is often used for handled exceptions or other important log events. For example, if your application requires a configuration setting but has a default in case the setting is missing, then the **Warning** level should be used to log the missing configuration setting.

### 3.4 Information

The **Information** level is typically used to output information that is useful to the running and management of your system. **Information** is typically the level used to log entry and exit points in key areas of your application. 

However, you may choose to add more entry and exit points at the **Debug** level for more granularity during development and testing.

### 3.5 Debug

**Debug** should be used for debugging systems during development, but never in a production system. It can be used to easily assess problems as well as your app's general flow.

### 3.6 Trace

**Trace** is the most verbose logging level, and should be used if you want more detailed logging than **Debug**.

## 4 Native Client Default Log Nodes

This section provides some details on specific log nodes used by the Mendix native client. We recommend that if you write your own [log messages](/refguide/log-message/) you should also use your own log node names to avoid confusion with the Mendix log messages.

### 4.1 Default Mendix Native Client Log Nodes {#native-client-log-nodes}

The following log nodes are used by Mendix when writing log messages:

| Log Node | Description |
| --- | --- |
| Client_AppCenter| Logs messages related to the state and phases of over-the-air updates by AppCenter. |
| Client_Auth | Logs messages related to the different authentication states and user actions.|
| Client | The default log node when no log node is provided. |
| Client_Database | Logs messages related to different read/write operations on the local database. |
| Client_FileSystem | Logs messages related to different read/write operations on the local file system (for example downloading a file and storing it in the file system, moving a file, or removing a file.)|
| Client_Nanoflow | Logs messages related to nanoflows being executed.|  
| Client_NanoflowDebugger | Logs messages related to different steps and available variables while debugging a nanoflow. |
| Client_Navigation | Logs messages related to the navigation behavior in the app. |
| Client_Network | Logs messages related to the different interactions between the client and runtime over the network. |
| Client_OTA | Logs messages related to the state and phases of over-the-air updates by Mendix OTA. |
| Client_SelectiveSync | Logs messages related to selective synchronization, like the different synchronization phases, the count of objects which have been synchronized, sync duration, and more. |
| Client_Startup | Logs messages related to client startup phase. |
| Client_Synchronization | Logs messages related to the full synchronization action and its phases. |

## 5 Sending Log Messages To Runtime {#sending-client-log-nodes-to-runtime}

The native client stores log messages locally on the device. When the **Enable sending logs to runtime** checkbox is selected, the native client will attempt to send log messages in batches of **100** messages or after 1 hour from the time since these log messages occurred. 

Therefore these client log nodes will not appear directly in the cloud portal logs overview before they are sent. If there is network connectivity, once the log messages have been successfully sent to the runtime these log messages will be cleared from the device.
