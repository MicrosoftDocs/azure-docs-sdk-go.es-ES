---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 5df14f939efdd0550b49261c88c8dc6518ada459
ms.sourcegitcommit: 887b15afcdeaf926a5f3d21b64e4045167fd062c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2018
ms.locfileid: "52293542"
---
<span data-ttu-id="87085-101">[Azure SDK para Go](https://github.com/Azure/azure-sdk-for-go) es compatible con las versiones de Go 1.8 y posteriores.</span><span class="sxs-lookup"><span data-stu-id="87085-101">The [Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) is compatible with Go versions 1.8 and higher.</span></span> <span data-ttu-id="87085-102">Para entornos con [perfiles de Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-go), la versión 1.9 de Go es el requisito mínimo.</span><span class="sxs-lookup"><span data-stu-id="87085-102">For environments using [Azure Stack Profiles](/azure/azure-stack/user/azure-stack-version-profiles-go), Go version 1.9 is the minimum requirement.</span></span>
<span data-ttu-id="87085-103">Si tiene que instalar Go, siga [las instrucciones de instalación de Go](https://golang.org/doc/install).</span><span class="sxs-lookup"><span data-stu-id="87085-103">If you need to install Go, follow [the Go installation instructions](https://golang.org/doc/install).</span></span>

<span data-ttu-id="87085-104">Puede descargar Azure SDK para Go y sus dependencias en `go get`.</span><span class="sxs-lookup"><span data-stu-id="87085-104">You can download the Azure SDK for Go and its dependencies via `go get`.</span></span>

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> <span data-ttu-id="87085-105">Asegúrese de escribir en mayúsculas `Azure` en la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="87085-105">Make sure that you capitalize `Azure` in the URL.</span></span> <span data-ttu-id="87085-106">De lo contrario puede causar problemas de importación relacionados con las mayúsculas y minúsculas cuando se trabaja con el SDK.</span><span class="sxs-lookup"><span data-stu-id="87085-106">Doing otherwise can cause case-related import problems when working with the SDK.</span></span> <span data-ttu-id="87085-107">También debe escribir en mayúscula `Azure` en las instrucciones de importación.</span><span class="sxs-lookup"><span data-stu-id="87085-107">You also need to capitalize `Azure` in your import statements.</span></span>
