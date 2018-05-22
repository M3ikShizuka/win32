﻿---
Description: 'Provides access to the underlying XML node and &\#0034;name&\#0034; attribute information for a Print Schema element.'
ms.assetid: 'E6F6F00B-E116-4AEA-AF9A-55209DA20DC6'
title: IPrintSchemaElement interface
---

# IPrintSchemaElement interface

Provides access to the underlying XML node and "name" attribute information for a Print Schema element.

## Members

The **IPrintSchemaElement** interface inherits from the [**IDispatch**](ebbff4bc-36b2-4861-9efa-ffa45e013eb5) interface. **IPrintSchemaElement** also has these types of members:

-   [Properties](#properties)

### Properties

The **IPrintSchemaElement** interface has these properties.



| Property                                                            | Access type          | Description                                                                   |
|:--------------------------------------------------------------------|:---------------------|:------------------------------------------------------------------------------|
| [**Name**](iprintschemaelement-name.md)<br/>                 | Read-only<br/> | Gets the base value of the "name" attribute of this node.<br/>          |
| [**NamespaceUri**](iprintschemaelement-namespaceuri.md)<br/> | Read-only<br/> | Gets the namespace URI value of the "name" attribute of this node.<br/> |
| [**XmlNode**](iprintschemaelement-xmlnode.md)<br/>           | Read-only<br/> | Gets the IXMLDOMNode object associated with this item.<br/>             |



 

## Requirements



|                                     |                                                                                               |
|-------------------------------------|-----------------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows 8<br/>                                                                          |
| Minimum supported server<br/> | Windows Server 2012<br/>                                                                |
| Header<br/>                   | <dl> <dt>Printerextension.h</dt> </dl> |



 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bprint\print%5D:%20IPrintSchemaElement%20interface%20%20RELEASE:%20%285/12/2018%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/en-us/default.aspx. "Send comments about this topic to Microsoft")



