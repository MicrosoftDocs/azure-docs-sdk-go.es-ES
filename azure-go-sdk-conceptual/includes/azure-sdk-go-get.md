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
<span data-ttu-id="ef2a9-101">El [SDK de Azure para Go](https://github.com/Azure/azure-sdk-for-go) es compatible con las versiones de Go 1.8 y posteriores.</span><span class="sxs-lookup"><span data-stu-id="ef2a9-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and later.</span></span> <span data-ttu-id="ef2a9-102">Para entornos con [perfiles de Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), la versión 1.9 de Go es el requisito mínimo.</span><span class="sxs-lookup"><span data-stu-id="ef2a9-102">For environments using [Azure Stack Profiles](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="ef2a9-103">Si no dispone de Go en el sistema, siga [las instrucciones de instalación Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="ef2a9-103">If you do not have Go available on your system, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="ef2a9-104">Puede obtener el SDK de Azure para Go y sus dependencias a través de `go get`.</span><span class="sxs-lookup"><span data-stu-id="ef2a9-104">You can obtain the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="ef2a9-105">Asegúrese de escribir en mayúsculas `Azure` en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="ef2a9-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="ef2a9-106">De lo contrario puede causar problemas de importación relacionados con las mayúsculas y minúsculas cuando se trabaja con el SDK.</span><span class="sxs-lookup"><span data-stu-id="ef2a9-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="ef2a9-107">También debe escribir en mayúscula `Azure` en las instrucciones de importación.</span><span class="sxs-lookup"><span data-stu-id="ef2a9-107">You also need to capitalize `Azure` in your import statements.</span></span>

