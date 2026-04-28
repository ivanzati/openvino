---
sidebar_label: 'Remote Context'
format: md
---

# Remote Context

ov::RemoteContext class functionality:

  - Represents device-specific inference context.
  - Allows to create remote device specific tensor.

Note

If plugin provides a public API for own Remote Context, the API should be header only and does not depend on the plugin library.

## RemoteContext Class

OpenVINO Plugin API provides the interface ov::IRemoteContext which should be used as a base class for a plugin specific remote context. Based on that, a declaration of an compiled model class can look as follows:

src/plugins/template/src/remote\_context.hpp

### Class Fields

The example class has several fields:

  - `m_name` - Device name.
  - `m_property` - Device-specific context properties. It can be used to cast RemoteContext to device specific type.

### RemoteContext Constructor

This constructor should initialize the remote context device name and properties.

src/plugins/template/src/remote\_context.cpp

### get\_device\_name()

The function returns the device name from the remote context.

src/plugins/template/src/remote\_context.cpp

### get\_property()

The implementation returns the remote context properties.

src/plugins/template/src/remote\_context.cpp

### create\_tensor()

The method creates device specific remote tensor.

src/plugins/template/src/remote\_context.cpp

The next step to support device specific tensors is a creation of device specific \[Remote Tensor\](remote-tensor.md) class.
