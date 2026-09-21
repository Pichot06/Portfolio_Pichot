---
title: "DIY NAS with Home Assistant"
translationKey: monitoring-hass-nas
date: 2025-04-01
url: "/en/projects/monitoring_hass_nas/"
category: projects
summary: A personal NAS and Home Assistant server
cover:
  image: cover.png
  alt: Home Assistant NAS
  relative: true
layout: single
showtoc: true
tocopen: true
tags: ["Home automation", "Home Assistant", "Monitoring", "Personal project"]
---

# Is Home Assistant essential for home automation?

Home Assistant has become one of the leading open-source home automation platforms. Its compatibility and active community make it a powerful foundation for a reliable smart home.

## Technology used

I installed Umbrel OS on a Raspberry Pi 5 with a PCIe-to-2-CH M.2 HAT and two SSDs. A Zigbee USB adapter connects my devices.

## Applications

I installed Home Assistant, Immich, Nextcloud and Tailscale. Home Assistant provides the automation dashboard, Immich stores photos locally, Nextcloud provides private file synchronization, and Tailscale gives secure remote access through WireGuard.

The next steps are to integrate my Linky electricity meter, automatic watering system, 3D printer and Google Home.
