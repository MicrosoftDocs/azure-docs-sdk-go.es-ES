---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: ddd58efdbc0c2d3ded068a9bebf2466db702566f
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
El [SDK de Azure para Go](https://github.com/Azure/azure-sdk-for-go) es compatible con las versiones de Go 1.8 y posteriores. Para entornos con [perfiles de Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), la versión 1.9 de Go es el requisito mínimo.
Si no dispone de Go en el sistema, siga [las instrucciones de instalación Go](https://golang.org/doc/install).

Puede obtener el SDK de Azure para Go y sus dependencias a través de `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Asegúrese de escribir en mayúsculas `Azure` en la dirección URL. De lo contrario puede causar problemas de importación relacionados con las mayúsculas y minúsculas cuando se trabaja con el SDK. También debe escribir en mayúscula `Azure` en las instrucciones de importación.

