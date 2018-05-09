---
title: Instalación del SDK de Azure para Go
description: Cómo instalar y configurar el SDK de Azure para Go e incluir los archivos dependientes en la carpeta vendor.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/14/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: ad77bdff881770512a828b19dc7af4821f4a55ad
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="2a7a4-103">Instalación del SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="2a7a4-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="2a7a4-104">Le damos la bienvenida al SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="2a7a4-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="2a7a4-105">El SDK permite administrar los servicios de Azure desde las aplicaciones de Go, e interactuar con ellos.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="2a7a4-106">Obtención del SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="2a7a4-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="2a7a4-107">Para trabajar con instancias de Azure Storage Blob es necesario un SDK independiente.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-107">Working with Azure Storage Blobs requires a separate SDK.</span></span>

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="2a7a4-108">Inclusión del SDK de Azure para Go en la carpeta vendor</span><span class="sxs-lookup"><span data-stu-id="2a7a4-108">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="2a7a4-109">Los archivos dependientes se pueden incluir en la carpeta vendor del SDK de Azure para Go con [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="2a7a4-109">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="2a7a4-110">Por motivos de estabilidad, se recomienda incluir los archivos dependientes en la carpeta vendor.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-110">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="2a7a4-111">Para usar la compatibilidad con `dep`, agregue `github.com/Azure/azure-sdk-for-go` a una sección `[[constraint]]` de su archivo `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-111">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="2a7a4-112">Por ejemplo, para incluir en la carpeta vendor los archivos dependientes de la versión `14.0.0`, agregue la entrada siguiente:</span><span class="sxs-lookup"><span data-stu-id="2a7a4-112">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="2a7a4-113">Inclusión del SDK de Azure para Go en el proyecto</span><span class="sxs-lookup"><span data-stu-id="2a7a4-113">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="2a7a4-114">Para usar los servicios de Azure desde el código de Go, importe todos los servicios con lo que interactúa y los módulos `autorest` necesarios.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-114">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="2a7a4-115">Puede obtener una lista completa de los módulos disponibles en GoDoc para los [servicios disponibles](https://godoc.org/github.com/Azure/azure-sdk-for-go) y los [paquetes de AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="2a7a4-115">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="2a7a4-116">Los paquetes más comunes que necesita de `go-autorest` son:</span><span class="sxs-lookup"><span data-stu-id="2a7a4-116">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="2a7a4-117">Paquete</span><span class="sxs-lookup"><span data-stu-id="2a7a4-117">Package</span></span> | <span data-ttu-id="2a7a4-118">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="2a7a4-118">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="2a7a4-119">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="2a7a4-119">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="2a7a4-120">Objetos para el control de la autenticación del cliente del servicio</span><span class="sxs-lookup"><span data-stu-id="2a7a4-120">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="2a7a4-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="2a7a4-121">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="2a7a4-122">Constantes para las interacciones con los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="2a7a4-122">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="2a7a4-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="2a7a4-123">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="2a7a4-124">Mecanismos de autenticación para el acceso a los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="2a7a4-124">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="2a7a4-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="2a7a4-125">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="2a7a4-126">Aplicaciones auxiliares de aserción de tipos para trabajar con las estructuras de datos del SDK de Azure</span><span class="sxs-lookup"><span data-stu-id="2a7a4-126">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="2a7a4-127">Los módulos para los servicios de Azure tienen versiones independientes de las API del SDK que existen para ellos.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-127">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="2a7a4-128">Estas versiones son parte de la ruta de acceso de importación del módulo y pueden ser una _versión del servicio_ o un _perfil_.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-128">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="2a7a4-129">Actualmente, se recomienda que utilice una versión del servicio específica tanto para el desarrollo como para la versión final.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-129">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="2a7a4-130">Los servicios se encuentran en el módulo `services`.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-130">Services are located under the `services` module.</span></span> <span data-ttu-id="2a7a4-131">La ruta de acceso completa para la importación es el nombre del servicio, seguido de la versión en formato `YYYY-MM-DD` y seguido del nombre de servicio de nuevo.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-131">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="2a7a4-132">Por ejemplo, para incluir la versión `2017-03-30` del servicio de proceso:</span><span class="sxs-lookup"><span data-stu-id="2a7a4-132">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="2a7a4-133">Actualmente se recomienda que utilice la versión más reciente de un servicio, a menos que tenga una razón para no hacerlo.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-133">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="2a7a4-134">Si necesita una instantánea colectiva de los servicios, también puede seleccionar una versión de perfil único.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-134">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="2a7a4-135">En este momento, el único perfil bloqueado es la versión `2017-03-09`, que puede no tener las características más recientes de los servicios.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-135">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="2a7a4-136">Los perfiles se encuentran en el módulo `profiles`, con la versión en formato `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-136">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="2a7a4-137">Los servicios se agrupan bajo su versión de perfil.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-137">Services are grouped under their profile version.</span></span> <span data-ttu-id="2a7a4-138">Por ejemplo, para importar el módulo de administración de recursos de Azure desde el perfil `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="2a7a4-138">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="2a7a4-139">También están disponibles los perfiles `preview` y `latest`.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-139">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="2a7a4-140">No se recomienda su uso.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-140">Using them is not recommended.</span></span> <span data-ttu-id="2a7a4-141">Estos perfiles son las sucesivas versiones y el comportamiento del servicio puede cambiar en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-141">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a7a4-142">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="2a7a4-142">Next steps</span></span>

<span data-ttu-id="2a7a4-143">Para empezar a usar el SDK de Azure para Go, pruebe un inicio rápido.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-143">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="2a7a4-144">Implementación de una máquina virtual desde una plantilla</span><span class="sxs-lookup"><span data-stu-id="2a7a4-144">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="2a7a4-145">Transferencia de objetos a Azure Blob Storage con el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="2a7a4-145">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="2a7a4-146">Conexión a Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="2a7a4-146">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="2a7a4-147">Si desea empezar a trabajar con otros servicios del SDK para Go inmediatamente, eche un vistazo a algunos de los ejemplos de código disponibles.</span><span class="sxs-lookup"><span data-stu-id="2a7a4-147">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="2a7a4-148">Autenticación con los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="2a7a4-148">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="2a7a4-149">Implementación de nuevas máquinas virtuales con autenticación SSH</span><span class="sxs-lookup"><span data-stu-id="2a7a4-149">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="2a7a4-150">Implementación de una imagen de contenedor en Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="2a7a4-150">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="2a7a4-151">Creación de un clúster en el servicio de Kubernetes de Azure</span><span class="sxs-lookup"><span data-stu-id="2a7a4-151">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="2a7a4-152">Uso de los servicios de Azure Storage</span><span class="sxs-lookup"><span data-stu-id="2a7a4-152">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="2a7a4-153">Todos los ejemplos para el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="2a7a4-153">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
