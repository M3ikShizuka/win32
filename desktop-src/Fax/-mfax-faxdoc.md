---
Description: 'The FaxDoc object permits a fax client application to transmit fax documents and cover pages, and to retrieve and set information about fax transmissions.'
ms.assetid: '11462af9-20c2-4661-801e-dcc3e092283d'
title: FaxDoc
---

# FaxDoc

The FaxDoc object permits a fax client application to transmit fax documents and cover pages, and to retrieve and set information about fax transmissions.

A FaxDoc object is created by a [**FaxServer**](-mfax-faxserver.md) object.

A fax client application must create an instance of a [FaxServer](-mfax-faxserver-client.md) object before accessing most other objects that begin with Fax. (A fax server connection is not required to access a [FaxTiff](-mfax-faxtiff.md) object.)

## C/C++

-   Create by calling the [**IFaxServer::CreateDocument**](-mfax-ifaxserver-client-mfax-ifaxserver-createdocument-cpp.md) method.
-   Supports the [**IFaxDoc**](-mfax-ifaxdoc.md) interface.

For more information about creating an instance of a FaxDoc object, and for a list of the object's properties and methods, see [**IFaxDoc**](-mfax-ifaxdoc.md).

## Visual Basic

Create by calling the [**CreateDocument**](-mfax-faxserver-object-visual-basic-.md) method of the [**FaxServer**](-mfax-faxserver-object-visual-basic-.md) object.

For more information about creating a FaxDoc object, and for a list of the properties and methods of the object, see [**FaxDoc object (Visual Basic)**](-mfax-faxdoc-object-visual-basic-.md).

 

 


