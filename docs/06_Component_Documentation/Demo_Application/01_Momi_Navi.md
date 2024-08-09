---
title: Momi Navi
---

# What is Momi Navi

Momi Navi is a example application for navigation.  It run on simple ivi demo (that name is momi IVI) for the instrument cluster container integration.

![](images/mominavi.jpg)

## How to use

When you build the instrument cluster integration without momi navi specific setting, it will not show map.  You need momi navi specific setting.

The momi navi uses mapbox gl plugin fo Qt location.  It requires own access token to download map.  You can create own mapbox access token following [this page](https://docs.mapbox.com/help/getting-started/access-tokens/).

After creation, please set environment variable to local.conf (or site.conf).

```
MOMIMAP_MAPBOX_ACCESS_TOKEN = "Your access token."
```

## Extra configuration

Momi navi supports configuration for customize.

When you set environment variable to local.conf (or site.conf), you can customize Momi navi.

| Environment variable | Customizing parametor |
|:---|:---|
| MOMIMAP_MAPBOX_STYLE | Map style |
| MOMIMAP_INITIAL_LATITUDE | Default location |
| MOMIMAP_INITIAL_LONGITUDE | Default location |

How to create map style, please check [this page](https://docs.mapbox.com/studio-manual/guides/map-styling/).
