---
title: Application Startup
---

# Introduction

At system runtime, it may be necessary for applications to start other
applications on demand. Such actions can be executed in reaction to a user
request, or they may be needed to perform a specific task.

In order to do so, running applications and services need an established way of
discovering installed applications and executing those.

In order to provide a language-independent interface for applications and
service to use, AGL includes `applaunchd`, a system service.

# Application launcher service

The purpose of `applaunchd` is to enumerate applications available on the
system and provide a way for other applications to query this list and start
those on demand.  It is also able to notify clients of the startup and
termination of applications it manages.

To that effect, `applaunchd` provides a gRPC interface which other applications
can use in order to execute those actions.

*Note: `applaunchd` will only send notifications for applications it started;
it isn't aware of applications started by other means (`systemd`, direct
executable call...), and therefore can't send notifications for those.*

## Application discovery

Applications are enumerated from systemd's list of available units based on the
pattern `agl-app*@*.service`, and are started and controled using their systemd
unit.  Please note `applaunchd` allows only one instance of a given
application.

## Application identifiers

Each application is identified by a unique Application ID. Although this ID can
be any valid string, it is highly recommended to use the "reverse DNS"
convention in order to avoid potential name collisions.

## gRPC interface

The interface provides methods for the following actions:

- retrieve the list of available applications
- request an application to be started
- subscribe to status events

Moreover, with the gRPC the client subscribes to a status signal to be notified
when an application has successfully started or its execution terminated.

The gRPC protobuf file provides a Request and Response arguments to RPC methods
even though in some cases these might be empty in order to allow forward
compatibility in case additional fields are required.
It is a good standard practice to follow up with these recommendation when
developing a new protobuf specification.

### Applications list

The `ListApplications` method allows clients to retrieve the list of available
applications. 

The `ListRequest` is an empty message, while `ListResponse` contains the following:

```
message AppInfo {
  string id = 1;
  string name = 2;
  string icon_path = 3;
}

message ListResponse {
  repeated AppInfo apps = 1;
}
```

### Application startup request

Applications can be started by using the `StartApplication` method, passing the
`StartRequest` message, defined as:

```
message StartRequest {
  string id = 1;
}
```

In reply, the following `StartResponse` will be returned:

```
message StartResponse {
  bool status = 1;
  string message = 2;
}
```

The "message" string  of `StartResponse` message will contain an error message
in case we couldn't start the application for whatever reason, or if the "id"
isn't a known application ID. The "status" string would be boolean set to
boolean `TRUE` otherwise.

If the application is already running, `applaunchd` won't start another
instance, but instead reply with a `AppStatus` message setting the `status`
string to "started".

### Status notifications

The gRPC interface provides clients with a subscription model to receive
status events. Client should subscribe to `GetStatusEvents` method to receive
them.

The `StatusRequest` is empty, while the `StatusResponse` is defined as
following:

```
message AppStatus {
  string id = 1;
  string status = 2;
}

message LauncherStatus {
}

message StatusResponse {
  oneof status {
    AppStatus app = 1;
    LauncherStatus launcher = 2;
  }
}
```

As mentioned above, the `status` string is set to "started" and is also emitted
if `applaunchd` receives a request to start an already running application.
This can be useful, for example, when switching between graphical applications:

- the application switcher doesn't need to track the state of each application;
  instead, it can simply send a `StartApplication` request to `applaunchd`
  every time the user requests to switch to another application
- the shell client then receives the `StatusResponse` with the message `status`
  string set to "started" indicating it that it should activate the window with
  the corresponding `id` string
