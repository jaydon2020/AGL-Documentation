---
title: Creating a New Application
---

In the context of AGL, "applications" are usually considered to be standalone
user facing infotainment applications that can be started in addition to the
default "homescreen" or similar user interface that will always be present.
Such applications can be enumerated and executed with `applaunchd`, the AGL
[application launcher service](02_Application_Startup.md).  The following
sections walk through what is required to add an application to an AGL image
that uses applaunchd, as well as discussion of developer conveniences that
AGL provides to simplify doing so in the Yocto build environment.

# Basic Requirements

As described in the [applaunchd documentation](02_Application_Startup.md#application-discovery),
applications need to have a systemd system unit with a name matching the pattern `agl-app*@*.service`.
The portion of the unit name between `@` and `.service` is used as the application identifier
(see [Application Identifiers](02_Application_Startup.md#application-identifiers)).
The use of `@` in the unit filename indicates the use of a template unit, which `applaunchd`
assumes will be used for applications (see below). Template unit files are discussed in the
systemd unit file [man page](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Description).

The requirements on the unit file contents are:

- The `Description` field is used as the desired display name of the application.
  It is recommended that this be kept short, ideally to one or two words, in order to
  avoid issues when applications are listed in a UI.
- While not a hard requirement, ideally the `User` field is used to specify running
  the application as a non-privileged user. In the AGL demonstration images, the
  `agl-driver` user is used.

Blah blah icon...

# Graphical Application Requirements

In addition, graphical applications need to comply with a few more requirements:

1. Each application must set a Wayland application ID appropriately as soon as its main window
is created.
2. The `app-id` used must match the application identifier specified in the systemd unit filename.

Doing so will ensure other software can associate the actual `app-id` to the proper application.

*Note: application ID's are set using the [XDG toplevel](https://wayland-book.com/xdg-shell-basics/xdg-toplevel.html)
Wayland interface.*

# Application Templates

To provide a working example of application template units, AGL includes templates
for generic, Flutter, and web applications as part of the `applaunchd` recipe in the
`meta-app-framework` layer in [meta-agl](../../04_Developer_Guides/02_AGL_Layers/02_meta_agl.md).
If applaunchd is built into an image using the recipe, these templates will be available
for applications to use for their systemd units by creating a symlink that includes the
application identifier.  To simplify this, a `BitBake` class named `agl-app` is included in
the layer for use in application recipes.  The different templates and their configuration
will be discussed further in the following sections.

## Generic Graphical Application Template

The `agl-app@.service` template unit is intended for simple standalone graphical applications.
It makes the following assumptions by default:

- The application executable name is the same as the application identifier, and the
  executable is available via the default `PATH`.
- The application `Description` will be the application identifier.
- The application should run as the `agl-driver` user.

If using the `agl-app` class in a recipe (with `inherit agl-app`), these defaults can be
changed via the variables:

- `AGL_APP_NAME`: overrides the value of the `Description` field to set the desired
  application display name.
- `AGL_APP_EXEC`: overrides the application executable name.

If set, these variables will trigger the generation of one or more systemd `drop-in`
override files by the logic in the `agl-app` class.  The use of `drop-in` configuration
files is discussed in the systemd unit file [man page](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Description).

Additionally, the variable `AGL_APP_ID` may be used to override the assumption that the
application identifier is the same as the recipe name. If specified, its value should be
the desired application identifier.

The following is a partial example BitBake recipe using the template for an application named
`foo`:

```text

SUMMARY = "foo application"
HOMEPAGE = "https://git.automotivelinux.org/apps/foo"
LICENSE = "Apache-2.0"
LIC_FILES_CHKSUM = "file://LICENSE;md5=ae6497158920d9524cf208c09cc4c984"

PV = "1.0+git${SRCREV}"

SRC_URI = "git://git.automotivelinux.org/apps/foo;protocol=https;branch=master"
SRCREV = "abcdef012"

S = "${WORKDIR}/git"

inherit agl-app

AGL_APP_NAME = "Foo"

<Other application build configuration>
```

Note that this assumes that the recipe is named `foo_git.bb`. If the recipe was instead named
something like `foo-app_git.bb`, then `AGL_APP_ID = "foo"` would need to be added.

## Flutter Application Template

The `agl-app-flutter@.service` template unit is intended for Flutter graphical applications
that use the `flutter-auto` Flutter embedder.  It makes the following assumptions by default:

- The Flutter application name in `pubspec.yaml` is the same as the application identifier,
and the application bundle will be installed under the directory:
  ```text
  /usr/share/flutter/<application identifier>/<Flutter version>/<Flutter runtime type>
  ```
  For example:
  ```text
  /usr/share/flutter/flutter_hvac/3.3.7/release
  ```

- The application `Description` will be the application identifier.
- The application should run as the `agl-driver` user.

If using the `agl-app` class in a recipe (with `inherit agl-app`), the use of the Flutter
template can be triggered by setting the variable `AGL_APP_TEMPLATE` to `agl-app-flutter`,
and the above defaults can be changed via the variables:

- `AGL_APP_NAME`: overrides the value of the `Description` field to set the desired
  application display name.

If set, this will trigger the generation of a systemd `drop-in` override file by the logic
in the `agl-app` class.  The use of `drop-in` configuration files is discussed in the systemd
unit file [man page](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Description).

Additionally, the variable `AGL_APP_ID` may be used to override the assumption that the
application identifier is the same as the recipe name. If specified, its value should be
the desired application identifier and Flutter application / bundle name.

The following is an example BitBake recipe using the template for a Flutter application
named `foo`:

```text

SUMMARY = "Flutter foo application"
HOMEPAGE = "https://git.automotivelinux.org/apps/flutter-foo"
LICENSE = "Apache-2.0"
LIC_FILES_CHKSUM = "file://License.md;md5=f712ede8d4f845976061925d1416fc40"

PV = "1.0+git${SRCREV}"

SRC_URI = "git://git.automotivelinux.org/apps/flutter-foo;protocol=https;branch=master"
SRCREV = "abcdef012"

S = "${WORKDIR}/git"

inherit flutter-app agl-app

PUBSPEC_APPNAME = "foo"
FLUTTER_APPLICATION_INSTALL_PREFIX = "/flutter"

AGL_APP_TEMPLATE = "agl-app-flutter"
AGL_APP_NAME = "Foo"
```

Note that this assumes that the recipe is named `foo_git.bb`, and that the application name
in the Flutter `pubspec.yaml` is `foo`.  If the recipe was instead named something like
`flutter-foo_git.bb`, then `AGL_APP_ID = "foo"` would need to be added.

## Web Application Template

The `agl-app-web@.service` template unit is intended for web applications run using the
Web Application Manager (WAM) service.  It makes the following assumptions by default:

- The web application bundle directory name is the same as the application identifier, and the
  bundle will be installed in the directory hierarchy:
  ```
  /usr/lib/wam_apps/<application identifier>
  ```
  For example:
  ```
  /usr/lib/wam_apps/webapps-hvac
  ```
- The application `Description` will be the application identifier.
- The application should run as the `agl-driver` user.

If using the `agl-app` class in a recipe (with `inherit agl-app`), the use of the Flutter
template can be triggered by setting the variable `AGL_APP_TEMPLATE` to `agl-app-web`,
and the above defaults can be changed via the variables:

- `AGL_APP_NAME`: overrides the value of the `Description` field to set the desired
  application display name.
- `AGL_APP_WAM_DIR`: overrides the application bundle directory name. This can be used if
  the application bundle name does not match the application identifier.

If set, these variables will trigger the generation of one or more systemd `drop-in`
override files by the logic in the `agl-app` class.  The use of `drop-in` configuration
files is discussed in the systemd unit file [man page](https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Description).

Additionally, the variable `AGL_APP_ID` may be used to override the assumption that the
application identifier is the same as the recipe name.

The following is a partial example BitBake recipe using the template for a web application
named `foo`:

```text

SUMMARY = "Web foo application"
HOMEPAGE = "https://git.automotivelinux.org/apps/web-foo"
LICENSE = "Apache-2.0"
LIC_FILES_CHKSUM = "file://LICENSE;md5=3b83ef96387f14655fc854ddc3c6bd57"

PV = "1.0+git${SRCREV}"

SRC_URI = "git://git.automotivelinux.org/apps/web-foo;protocol=https;branch=master"
SRCREV = "abcdef012"

S = "${WORKDIR}/git"

inherit agl-app

AGL_APP_TEMPLATE = "agl-app-web"
AGL_APP_NAME = "Foo"

<Other web application build configuration>
```

Note that this assumes that the recipe is named `foo_git.bb`. If the recipe was instead named
something like `foo-web_git.bb`, then `AGL_APP_ID = "foo"` would need to be added.
