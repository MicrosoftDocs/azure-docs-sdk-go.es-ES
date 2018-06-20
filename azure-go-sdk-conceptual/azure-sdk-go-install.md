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
ms.openlocfilehash: 3388359bba791c87025b6ffd0e6b476f95589f73
ms.sourcegitcommit: 81e97407e6139375bf7357045e818c87a17dcde1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36262987"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="304bb-103">Instalación del SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="304bb-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="304bb-104">Le damos la bienvenida al SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="304bb-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="304bb-105">El SDK permite administrar los servicios de Azure desde las aplicaciones de Go, e interactuar con ellos.</span><span class="sxs-lookup"><span data-stu-id="304bb-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="304bb-106">Obtención del SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="304bb-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="304bb-107">Algunos servicios de Azure tienen sus propios SDK de Go y no se incluyen en el paquete principal del SDK de Azure para Go.</span><span class="sxs-lookup"><span data-stu-id="304bb-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="304bb-108">En la tabla siguiente se enumeran los servicios que tienen sus propios SDK, y sus nombres de paquete.</span><span class="sxs-lookup"><span data-stu-id="304bb-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="304bb-109">Todos estos paquetes están en versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="304bb-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="304bb-110">Servicio</span><span class="sxs-lookup"><span data-stu-id="304bb-110">Service</span></span> | <span data-ttu-id="304bb-111">Paquete</span><span class="sxs-lookup"><span data-stu-id="304bb-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="304bb-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="304bb-112">Blob Storage</span></span> | [<span data-ttu-id="304bb-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="304bb-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="304bb-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="304bb-114">File Storage</span></span> | [<span data-ttu-id="304bb-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="304bb-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="304bb-116">Cola de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="304bb-116">Storage Queue</span></span> | [<span data-ttu-id="304bb-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="304bb-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="304bb-118">Centro de eventos</span><span class="sxs-lookup"><span data-stu-id="304bb-118">Event Hub</span></span> | [<span data-ttu-id="304bb-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="304bb-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="304bb-120">Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="304bb-120">Service Bus</span></span> | [<span data-ttu-id="304bb-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="304bb-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="304bb-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="304bb-122">Application Insights</span></span> | [<span data-ttu-id="304bb-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="304bb-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="304bb-124">Inclusión del SDK de Azure para Go en la carpeta vendor</span><span class="sxs-lookup"><span data-stu-id="304bb-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="304bb-125">Los archivos dependientes se pueden incluir en la carpeta vendor del SDK de Azure para Go con [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="304bb-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="304bb-126">Por motivos de estabilidad, se recomienda incluir los archivos dependientes en la carpeta vendor.</span><span class="sxs-lookup"><span data-stu-id="304bb-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="304bb-127">Para usar la compatibilidad con `dep`, agregue `github.com/Azure/azure-sdk-for-go` a una sección `[[constraint]]` de su archivo `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="304bb-127">In order to use `dep` support, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="304bb-128">Por ejemplo, para incluir en la carpeta vendor los archivos dependientes de la versión `14.0.0`, agregue la entrada siguiente:</span><span class="sxs-lookup"><span data-stu-id="304bb-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="304bb-129">Inclusión del SDK de Azure para Go en el proyecto</span><span class="sxs-lookup"><span data-stu-id="304bb-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="304bb-130">Para usar los servicios de Azure desde el código de Go, importe todos los servicios con lo que interactúa y los módulos `autorest` necesarios.</span><span class="sxs-lookup"><span data-stu-id="304bb-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="304bb-131">Puede obtener una lista completa de los módulos disponibles en GoDoc para los [servicios disponibles](https://godoc.org/github.com/Azure/azure-sdk-for-go) y los [paquetes de AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="304bb-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="304bb-132">Los paquetes más comunes que necesita de `go-autorest` son:</span><span class="sxs-lookup"><span data-stu-id="304bb-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="304bb-133">Paquete</span><span class="sxs-lookup"><span data-stu-id="304bb-133">Package</span></span> | <span data-ttu-id="304bb-134">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="304bb-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="304bb-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="304bb-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="304bb-136">Objetos para el control de la autenticación del cliente del servicio</span><span class="sxs-lookup"><span data-stu-id="304bb-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="304bb-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="304bb-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="304bb-138">Constantes para las interacciones con los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="304bb-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="304bb-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="304bb-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="304bb-140">Mecanismos de autenticación para el acceso a los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="304bb-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="304bb-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="304bb-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="304bb-142">Aplicaciones auxiliares de aserción de tipos para trabajar con las estructuras de datos del SDK de Azure</span><span class="sxs-lookup"><span data-stu-id="304bb-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="304bb-143">Los módulos para los servicios de Azure tienen versiones independientes de las API del SDK que existen para ellos.</span><span class="sxs-lookup"><span data-stu-id="304bb-143">Modules for Azure services are versioned independently from the SDK APIs for them.</span></span> <span data-ttu-id="304bb-144">Estas versiones son parte de la ruta de acceso de importación del módulo y pueden ser una _versión del servicio_ o un _perfil_.</span><span class="sxs-lookup"><span data-stu-id="304bb-144">These versions are part of the module import path, and are from either a _service version_ or a _profile_.</span></span> <span data-ttu-id="304bb-145">Actualmente, se recomienda que utilice una versión del servicio específica tanto para el desarrollo como para la versión final.</span><span class="sxs-lookup"><span data-stu-id="304bb-145">Currently, it is recommended that you use a specific service version for both development and release.</span></span> <span data-ttu-id="304bb-146">Los servicios se encuentran en el módulo `services`.</span><span class="sxs-lookup"><span data-stu-id="304bb-146">Services are located under the `services` module.</span></span> <span data-ttu-id="304bb-147">La ruta de acceso completa para la importación es el nombre del servicio, seguido de la versión en formato `YYYY-MM-DD` y seguido del nombre de servicio de nuevo.</span><span class="sxs-lookup"><span data-stu-id="304bb-147">The full path for the import is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="304bb-148">Por ejemplo, para incluir la versión `2017-03-30` del servicio de proceso:</span><span class="sxs-lookup"><span data-stu-id="304bb-148">For example, to include the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="304bb-149">Actualmente se recomienda que utilice la versión más reciente de un servicio, a menos que tenga una razón para no hacerlo.</span><span class="sxs-lookup"><span data-stu-id="304bb-149">Right now it is recommended that you use the latest version of a service, unless you have a reason to do otherwise.</span></span>

<span data-ttu-id="304bb-150">Si necesita una instantánea colectiva de los servicios, también puede seleccionar una versión de perfil único.</span><span class="sxs-lookup"><span data-stu-id="304bb-150">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="304bb-151">En este momento, el único perfil bloqueado es la versión `2017-03-09`, que puede no tener las características más recientes de los servicios.</span><span class="sxs-lookup"><span data-stu-id="304bb-151">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="304bb-152">Los perfiles se encuentran en el módulo `profiles`, con la versión en formato `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="304bb-152">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="304bb-153">Los servicios se agrupan bajo su versión de perfil.</span><span class="sxs-lookup"><span data-stu-id="304bb-153">Services are grouped under their profile version.</span></span> <span data-ttu-id="304bb-154">Por ejemplo, para importar el módulo de administración de recursos de Azure desde el perfil `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="304bb-154">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="304bb-155">También están disponibles los perfiles `preview` y `latest`.</span><span class="sxs-lookup"><span data-stu-id="304bb-155">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="304bb-156">No se recomienda su uso.</span><span class="sxs-lookup"><span data-stu-id="304bb-156">Using them is not recommended.</span></span> <span data-ttu-id="304bb-157">Estos perfiles son las sucesivas versiones y el comportamiento del servicio puede cambiar en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="304bb-157">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="304bb-158">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="304bb-158">Next steps</span></span>

<span data-ttu-id="304bb-159">Para empezar a usar el SDK de Azure para Go, pruebe un inicio rápido.</span><span class="sxs-lookup"><span data-stu-id="304bb-159">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="304bb-160">Implementación de una máquina virtual desde una plantilla</span><span class="sxs-lookup"><span data-stu-id="304bb-160">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="304bb-161">Transferencia de objetos a Azure Blob Storage con el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="304bb-161">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="304bb-162">Conexión a Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="304bb-162">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="304bb-163">Si desea empezar a trabajar con otros servicios del SDK para Go inmediatamente, eche un vistazo a algunos de los ejemplos de código disponibles.</span><span class="sxs-lookup"><span data-stu-id="304bb-163">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="304bb-164">Autenticación con los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="304bb-164">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [<span data-ttu-id="304bb-165">Implementación de nuevas máquinas virtuales con autenticación SSH</span><span class="sxs-lookup"><span data-stu-id="304bb-165">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="304bb-166">Implementación de una imagen de contenedor en Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="304bb-166">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="304bb-167">Creación de un clúster en el servicio de Kubernetes de Azure</span><span class="sxs-lookup"><span data-stu-id="304bb-167">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="304bb-168">Uso de los servicios de Azure Storage</span><span class="sxs-lookup"><span data-stu-id="304bb-168">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="304bb-169">Todos los ejemplos para el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="304bb-169">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
