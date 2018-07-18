---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 02/14/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: d021dd8ef4744b7c50b296b231bf63481f92411a
ms.sourcegitcommit: 2a3bd491e087a1d0e7d269bed896c029357d62a6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988073"
---
El [SDK de Azure para Go](https://github.com/Azure/azure-sdk-for-go) es compatible con las versiones de Go 1.8 y posteriores. Para entornos con [perfiles de Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-version-profiles), la versión 1.9 de Go es el requisito mínimo.
Si no dispone de Go en el sistema, siga [las instrucciones de instalación Go](https://golang.org/doc/install).

Puede obtener el SDK de Azure para Go y sus dependencias a través de `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Asegúrese de escribir en mayúsculas `Azure` en la dirección URL. De lo contrario puede causar problemas de importación relacionados con las mayúsculas y minúsculas cuando se trabaja con el SDK. También debe escribir en mayúscula `Azure` en las instrucciones de importación.
