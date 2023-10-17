---
title: Introduction
---

# Foreword

AGL has worked at providing the components of an application framework for many
years. However, the implementation used up until the `lamprey` release was retired
starting with the `marlin` release, and replaced with a redesigned one.
However, this new implementation does not aim to be a 1:1 replacement, and as such
it does not provide all of the features of the previous framework. Some of those
will be added back over time, while others have been discarded in favor of more
modern and/or widely-used alternatives.

With the `needlefish` release, further evolution of the replacement framework included:

- Using [gRPC IPC](https://grpc.io/about) for the application launcher API.
  The interim D-Bus based API was deprecated at this time, and removed in the
  `pike` release.
- Using only systemd unit metadata in the application launcher instead of using
  [Desktop Entry specification](https://www.freedesktop.org/wiki/Specifications/desktop-entry-spec/)
  to list applications, and relying entirely on systemd for the application
  lifecycle, rather than spawning them directly.

# Introduction

With a goal of being the provider of an integrated solution to build up on, it
is useful for AGL to define a reliable and well-specified method for managing
the deployment and integration of applications and services, as well as the way
they can interact with the rest of the system.

This is achieved by providing a common set of rules and components, known as
the application framework. By documenting those rules, application developers can
have a good understanding of the requirements for creating and packaging applications
targeting AGL-based systems that leverage the upstream application framework components.
Likewise, system developers and integrators have a clear path for including such
applications in AGL-based products.

The application framework's scope extends to the following areas:

- system services integration and lifecycle management
- user-level application lifecycle management
- inter-process communication

In order to be as simple as possible and avoid any unneeded custom
implementation, the application framework relies mainly on third-party
technologies and/or software components. They include:

- [systemd](https://www.freedesktop.org/wiki/Software/systemd/): system
  services and user session services management

- [gRPC](https://grpc.io/about): inter-process communication, new recommmended
  system-wide IPC, which should be used instead of D-Bus.

Note that while there are many open source software projects in the desktop Linux
space using [D-Bus](https://www.freedesktop.org/wiki/Software/dbus/) for inter-process
communication, AGL has decided to avoid using it for new development projects for the
following reasons:

- It has proven challenging in the past to manage dependencies between and the
  start-up of system and per-user D-Bus services in AGL.
- Managing the security of D-Bus APIs involves the use of PolicyKit, which is
  somewhat heavyweight and not widely understood. As well, providing D-Bus
  access to applications running inside containers or other sandbox schemes can
  be significantly more complicated than e.g. using gRPC with authorization token
  access control.
- D-Bus is not widely used in application development ecosystems outside of desktop
  Linux. This matters when engaging with application developers in newer ecosystems
  such as web applications or Flutter.

AGL also provides reference implementations whenever possible and relevant,
located in the [meta-agl](../../04_Developer_Guides/02_AGL_Layers/02_meta_agl.md)
layer under `meta-app-framework`. At the moment, the application framework
contains two such components:

- `agl-session`: `systemd` unit files for user sessions management
- `applaunchd`: application launcher service

# Service Management

Both system and user services are managed by `systemd`, which provides a number
of important features, such as dependency management or service monitoring:
when starting a service, `systemd` will ensure any other units this service
depends on are available, and otherwise start those dependencies. Similarly,
`systemd` can automatically restart a crashed service, ensuring minimal
downtime.

`systemd` also provides an efficient first layer of security through its
[sandboxing](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#Sandboxing)
and other security-related options.

It is also well integrated with D-Bus and can be used for a more fine-grained
control over D-Bus activated services.  By delegating the actual service start-up
to `systemd`, developers can take advantage of its advanced features, allowing
for improved reliability and security.

Each service should be represented by a `systemd` unit file installed to the
appropriate location. More details can be obtained from the [Creating a New
Service](../../04_Developer_Guides/03_Creating_a_New_Service.md) document.

# User Session Management

Similarly, user sessions and the services they rely on are also managed by
`systemd`.  Prior to the `pike` release, AGL used a user session for the
`agl-driver` user for the running of user facing applications, including the
compositor. This has been replaced with using system units that use the
`User` directive. The reason for this is two-fold:

- Several useful systemd sandboxing features are unavailable to user session
  units, or would require the use of unprivileged namespace mounting.  The
  latter is not necessarily available in vendor BSP kernels, and the security
  characteristics of it are still a matter of some debate.
- Encoding dependencies between user session and system units is sometimes
  not straightforward or impossible.

# Inter-process Communication

In order to provide a language-independent, "standard", IPC mechanism and avoid
the need for maintaining custom bindings for each programming language to be
used on top of AGL, the application framework previously promoted the use of
[D-Bus](https://www.freedesktop.org/wiki/Software/dbus/) as the preferred way
for applications to interact with services. Starting with `needlefish` release,
this has changed to recommending [gRPC](https://grpc.io) for our system-wide IPC,
with D-Bus being kept to provide functionality to services and applications
which haven't transitioned yet to using gRPC.

Most services already included in AGL provide one or several D-Bus interfaces,
and can therefore interact with D-Bus capable applications and services
without requiring any additional component. Those services include, among
others:

- [ConnMan](https://git.kernel.org/pub/scm/network/connman/connman.git/):
  network connectivity

- [BlueZ](http://www.bluez.org/): Bluetooth connectivity

- [oFono](https://git.kernel.org/pub/scm/network/ofono/ofono.git): telephony
  and modem management

- [GeoClue](https://gitlab.freedesktop.org/geoclue/geoclue/-/wikis/home):
  geolocation

Similarly, ongoing work involves expanding various services to expose a
gRPC interface.

# Application Launcher Service

The Application Framework used to follow the guidelines of the [Desktop Entry
specification](https://www.freedesktop.org/wiki/Specifications/desktop-entry-spec/)
for application enumeration and start-up, but with the `needlefish` release
instead it relies on systemd to provide that functionality, indirectly, by
using the `applaunchd` application.

As no simple reference implementation exists for this part of the
specification, AGL provides an application launcher service named `applaunchd`.
This service is part of the default user session, and as such is automatically
started on session start-up. It can therefore be considered always available.

`applaunchd` enumerates applications installed on the system and provides a
gRPC interface for services and applications to:

- query the list of available applications
- request the start-up of a specific application
- be notified when applications are started or terminated
