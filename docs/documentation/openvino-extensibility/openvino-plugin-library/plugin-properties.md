---
sidebar_label: 'Plugin Properties'
format: md
---

# Plugin Properties

> specific properties of an OpenVINO plugin.

Plugin can provide own device-specific properties.

## Property Class

OpenVINO API provides the interface ov::Property which allows to define the property and access rights. Based on that, a declaration of plugin specific properties can look as follows:

src/plugins/template/include/template/properties.hpp
