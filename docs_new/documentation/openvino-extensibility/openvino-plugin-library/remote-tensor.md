---
sidebar_label: 'Remote Tensor'
format: md
---

# Remote Tensor

ov::RemoteTensor class functionality:

  - Provides an interface to work with device-specific memory.

Note

If plugin provides a public API for own Remote Tensor, the API should be header only and does not depend on the plugin library.

## Device Specific Remote Tensor Public API

The public interface to work with device specific remote tensors should have header only implementation and doesn't depend on the plugin library.

src/plugins/template/include/template/remote\_tensor.hpp

The implementation below has several methods:

### type\_check()

Static method is used to understand that some abstract remote tensor can be casted to this particular remote tensor type.

### get\_data()

The set of methods (specific for the example, other implementation can have another API) which are helpers to get an access to remote data.

## Device-Specific Internal tensor implementation

The plugin should have the internal implementation of remote tensor which can communicate with public API.
The example contains the implementation of remote tensor which wraps memory from stl vector.

OpenVINO Plugin API provides the interface ov::IRemoteTensor which should be used as a base class for remote tensors.

The example implementation have two remote tensor classes:

  - Internal type dependent implementation which has as an template argument the vector type and create the type specific tensor.
  - The type independent implementation which works with type dependent tensor inside.

Based on that, an implementation of a type independent remote tensor class can look as follows:

src/plugins/template/src/remote\_tensor.hpp

The implementation provides a helper to get wrapped stl tensor and overrides all important methods of ov::IRemoteTensor class and recall the type dependent implementation.

The type dependent remote tensor has the next implementation:

src/plugins/template/src/remote\_context.cpp

### Class Fields

The class has several fields:

  - `m_element_type` - Tensor element type.
  - `m_shape` - Tensor shape.
  - `m_strides` - Tensor strides.
  - `m_data` - Wrapped vector.
  - `m_dev_name` - Device name.
  - `m_properties` - Remote tensor specific properties which can be used to detect the type of the remote tensor.

### VectorTensorImpl()

The constructor of remote tensor implementation. Creates a vector with data, initialize device name and properties, updates shape, element type and strides.

### get\_element\_type()

The method returns tensor element type.

### get\_shape()

The method returns tensor shape.

### get\_strides()

The method returns tensor strides.

### set\_shape()

The method allows to set new shapes for the remote tensor.

### get\_properties()

The method returns tensor specific properties.

### get\_device\_name()

The method returns tensor specific device name.
