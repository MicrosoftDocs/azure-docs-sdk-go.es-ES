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
ms.openlocfilehash: 7990ec8bde5622078aa822fc7e66ba5c4384d682
ms.sourcegitcommit: 3d26b464f196f8675c636ae792637d4c882fb92c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52337150"
---
# <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="efc84-103">Instalación del SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="efc84-103">Install the Azure SDK for Go</span></span>

<span data-ttu-id="efc84-104">Le damos la bienvenida al SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="efc84-104">Welcome to the Azure SDK for Go!</span></span> <span data-ttu-id="efc84-105">El SDK permite administrar los servicios de Azure desde las aplicaciones de Go, e interactuar con ellos.</span><span class="sxs-lookup"><span data-stu-id="efc84-105">The SDK allows you to manage and interact with Azure services from your Go applications.</span></span>

## <a name="get-the-azure-sdk-for-go"></a><span data-ttu-id="efc84-106">Obtención del SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="efc84-106">Get the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

<span data-ttu-id="efc84-107">Algunos servicios de Azure tienen sus propios SDK de Go y no se incluyen en el paquete principal del SDK de Azure para Go.</span><span class="sxs-lookup"><span data-stu-id="efc84-107">Some Azure services have their own Go SDK and aren't included in the core Azure SDK for Go package.</span></span> <span data-ttu-id="efc84-108">En la tabla siguiente se enumeran los servicios que tienen sus propios SDK, y sus nombres de paquete.</span><span class="sxs-lookup"><span data-stu-id="efc84-108">The following table lists the services with their own SDKs and their package names.</span></span> <span data-ttu-id="efc84-109">Todos estos paquetes están en versión preliminar.</span><span class="sxs-lookup"><span data-stu-id="efc84-109">These packages are all considered to be in preview.</span></span>

| <span data-ttu-id="efc84-110">Servicio</span><span class="sxs-lookup"><span data-stu-id="efc84-110">Service</span></span> | <span data-ttu-id="efc84-111">Paquete</span><span class="sxs-lookup"><span data-stu-id="efc84-111">Package</span></span> |
|---------|---------|
| <span data-ttu-id="efc84-112">Blob Storage</span><span class="sxs-lookup"><span data-stu-id="efc84-112">Blob Storage</span></span> | [<span data-ttu-id="efc84-113">github.com/Azure/azure-storage-blob-go</span><span class="sxs-lookup"><span data-stu-id="efc84-113">github.com/Azure/azure-storage-blob-go</span></span>](https://github.com/Azure/azure-storage-blob-go) |
| <span data-ttu-id="efc84-114">File Storage</span><span class="sxs-lookup"><span data-stu-id="efc84-114">File Storage</span></span> | [<span data-ttu-id="efc84-115">github.com/Azure/azure-storage-file-go</span><span class="sxs-lookup"><span data-stu-id="efc84-115">github.com/Azure/azure-storage-file-go</span></span>](https://github.com/Azure/azure-storage-file-go) |
| <span data-ttu-id="efc84-116">Cola de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="efc84-116">Storage Queue</span></span> | [<span data-ttu-id="efc84-117">github.com/Azure/azure-storage-queue-go</span><span class="sxs-lookup"><span data-stu-id="efc84-117">github.com/Azure/azure-storage-queue-go</span></span>](https://github.com/Azure/azure-storage-queue-go) |
| <span data-ttu-id="efc84-118">Centro de eventos</span><span class="sxs-lookup"><span data-stu-id="efc84-118">Event Hub</span></span> | [<span data-ttu-id="efc84-119">github.com/Azure/azure-event-hubs-go</span><span class="sxs-lookup"><span data-stu-id="efc84-119">github.com/Azure/azure-event-hubs-go</span></span>](https://github.com/Azure/azure-event-hubs-go) |
| <span data-ttu-id="efc84-120">Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="efc84-120">Service Bus</span></span> | [<span data-ttu-id="efc84-121">github.com/Azure/azure-service-bus-go</span><span class="sxs-lookup"><span data-stu-id="efc84-121">github.com/Azure/azure-service-bus-go</span></span>](https://github.com/Azure/azure-service-bus-go) |
| <span data-ttu-id="efc84-122">Application Insights</span><span class="sxs-lookup"><span data-stu-id="efc84-122">Application Insights</span></span> | [<span data-ttu-id="efc84-123">github.com/Microsoft/ApplicationInsights-go</span><span class="sxs-lookup"><span data-stu-id="efc84-123">github.com/Microsoft/ApplicationInsights-go</span></span>](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a><span data-ttu-id="efc84-124">Inclusión del SDK de Azure para Go en la carpeta vendor</span><span class="sxs-lookup"><span data-stu-id="efc84-124">Vendor the Azure SDK for Go</span></span>

<span data-ttu-id="efc84-125">Los archivos dependientes se pueden incluir en la carpeta vendor del SDK de Azure para Go con [dep](https://github.com/golang/dep).</span><span class="sxs-lookup"><span data-stu-id="efc84-125">The Azure SDK for Go may be vendored through [dep](https://github.com/golang/dep).</span></span> <span data-ttu-id="efc84-126">Por motivos de estabilidad, se recomienda incluir los archivos dependientes en la carpeta vendor.</span><span class="sxs-lookup"><span data-stu-id="efc84-126">For stability reasons, vendoring is recommended.</span></span> <span data-ttu-id="efc84-127">Para usar `dep` en su propio proyecto, agregue `github.com/Azure/azure-sdk-for-go` a una sección `[[constraint]]` de su `Gopkg.toml`.</span><span class="sxs-lookup"><span data-stu-id="efc84-127">To use `dep` in your own project, add `github.com/Azure/azure-sdk-for-go` to a `[[constraint]]` section of your `Gopkg.toml`.</span></span> <span data-ttu-id="efc84-128">Por ejemplo, para incluir en la carpeta vendor los archivos dependientes de la versión `14.0.0`, agregue la entrada siguiente:</span><span class="sxs-lookup"><span data-stu-id="efc84-128">For example, to vendor on version `14.0.0`, add the following entry:</span></span>

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a><span data-ttu-id="efc84-129">Inclusión del SDK de Azure para Go en el proyecto</span><span class="sxs-lookup"><span data-stu-id="efc84-129">Include the Azure SDK for Go in your project</span></span>

<span data-ttu-id="efc84-130">Para usar los servicios de Azure desde el código de Go, importe todos los servicios con lo que interactúa y los módulos `autorest` necesarios.</span><span class="sxs-lookup"><span data-stu-id="efc84-130">To use Azure services from your Go code, import any services you interact with and the required `autorest` modules.</span></span>
<span data-ttu-id="efc84-131">Puede obtener una lista completa de los módulos disponibles en GoDoc para los [servicios disponibles](https://godoc.org/github.com/Azure/azure-sdk-for-go) y los [paquetes de AutoRest](https://godoc.org/github.com/Azure/go-autorest).</span><span class="sxs-lookup"><span data-stu-id="efc84-131">You get a complete list of the available modules from GoDoc for [available services](https://godoc.org/github.com/Azure/azure-sdk-for-go) and [AutoRest packages](https://godoc.org/github.com/Azure/go-autorest).</span></span> <span data-ttu-id="efc84-132">Los paquetes más comunes que necesita de `go-autorest` son:</span><span class="sxs-lookup"><span data-stu-id="efc84-132">The most common packages you need from `go-autorest` are:</span></span>

| <span data-ttu-id="efc84-133">Paquete</span><span class="sxs-lookup"><span data-stu-id="efc84-133">Package</span></span> | <span data-ttu-id="efc84-134">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="efc84-134">Description</span></span> |
|---------|-------------|
| <span data-ttu-id="efc84-135">[github.com/Azure/go-autorest/autorest][autorest]</span><span class="sxs-lookup"><span data-stu-id="efc84-135">[github.com/Azure/go-autorest/autorest][autorest]</span></span> | <span data-ttu-id="efc84-136">Objetos para el control de la autenticación del cliente del servicio</span><span class="sxs-lookup"><span data-stu-id="efc84-136">Objects for handling service client authentication</span></span> |
| <span data-ttu-id="efc84-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span><span class="sxs-lookup"><span data-stu-id="efc84-137">[github.com/Azure/go-autorest/autorest/azure][autorest/azure]</span></span> | <span data-ttu-id="efc84-138">Constantes para las interacciones con los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="efc84-138">Constants for interactions with Azure services</span></span> |
| <span data-ttu-id="efc84-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span><span class="sxs-lookup"><span data-stu-id="efc84-139">[github.com/Azure/go-autorest/autorest/adal][autorest/adal]</span></span> | <span data-ttu-id="efc84-140">Mecanismos de autenticación para el acceso a los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="efc84-140">Authentication mechanisms for accessing Azure services</span></span> |
| <span data-ttu-id="efc84-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span><span class="sxs-lookup"><span data-stu-id="efc84-141">[github.com/Azure/go-autorest/autorest/to][autorest/to]</span></span> | <span data-ttu-id="efc84-142">Aplicaciones auxiliares de aserción de tipos para trabajar con las estructuras de datos del SDK de Azure</span><span class="sxs-lookup"><span data-stu-id="efc84-142">Type assertion helpers for working with Azure SDK data structures</span></span> |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

<span data-ttu-id="efc84-143">Las versiones de los paquetes de Go y los servicios de Azure son independientes.</span><span class="sxs-lookup"><span data-stu-id="efc84-143">Go packages and Azure services are versioned independently.</span></span> <span data-ttu-id="efc84-144">Las versiones de los servicios son parte de la ruta de importación del módulo, en el módulo `services`.</span><span class="sxs-lookup"><span data-stu-id="efc84-144">The service versions are part of the module import path, underneath the `services` module.</span></span> <span data-ttu-id="efc84-145">La ruta de acceso completa del módulo es el nombre del servicio, seguido de la versión en formato `YYYY-MM-DD` y seguido del nombre de servicio de nuevo.</span><span class="sxs-lookup"><span data-stu-id="efc84-145">The full path for the module is the name of the service, followed by the version in `YYYY-MM-DD` format, followed by the service name again.</span></span> <span data-ttu-id="efc84-146">Por ejemplo, para importar la versión `2017-03-30` del servicio Compute:</span><span class="sxs-lookup"><span data-stu-id="efc84-146">For example, to import the `2017-03-30` version of the Compute service:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

<span data-ttu-id="efc84-147">Se recomienda usar la versión más reciente de un servicio al comenzar a desarrollar y mantener la coherencia.</span><span class="sxs-lookup"><span data-stu-id="efc84-147">It's recommended that you use the latest version of a service when starting development and keep it consistent.</span></span>
<span data-ttu-id="efc84-148">Los requisitos del servicio pueden cambiar de una versión a otra y estos cambios podrían afectar al código, aunque no haya ninguna actualización de Azure SDK para Go durante ese tiempo.</span><span class="sxs-lookup"><span data-stu-id="efc84-148">Service requirements may change between versions that could break your code, even if there are no Go SDK updates during that time.</span></span>

<span data-ttu-id="efc84-149">Si necesita una instantánea colectiva de los servicios, también puede seleccionar una versión de perfil único.</span><span class="sxs-lookup"><span data-stu-id="efc84-149">If you need a collective snapshot of services, you can also select a single profile version.</span></span> <span data-ttu-id="efc84-150">En este momento, el único perfil bloqueado es la versión `2017-03-09`, que puede no tener las características más recientes de los servicios.</span><span class="sxs-lookup"><span data-stu-id="efc84-150">Right now, the only locked profile is version `2017-03-09`, which may not have the latest features of services.</span></span> <span data-ttu-id="efc84-151">Los perfiles se encuentran en el módulo `profiles`, con la versión en formato `YYYY-MM-DD`.</span><span class="sxs-lookup"><span data-stu-id="efc84-151">Profiles are located under the `profiles` module, with their version in the `YYYY-MM-DD` format.</span></span> <span data-ttu-id="efc84-152">Los servicios se agrupan bajo su versión de perfil.</span><span class="sxs-lookup"><span data-stu-id="efc84-152">Services are grouped under their profile version.</span></span> <span data-ttu-id="efc84-153">Por ejemplo, para importar el módulo de administración de recursos de Azure desde el perfil `2017-03-09`:</span><span class="sxs-lookup"><span data-stu-id="efc84-153">For example, to import the Azure Resources management module from the `2017-03-09` profile:</span></span>

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> <span data-ttu-id="efc84-154">También están disponibles los perfiles `preview` y `latest`.</span><span class="sxs-lookup"><span data-stu-id="efc84-154">There are also `preview` and `latest` profiles available.</span></span> <span data-ttu-id="efc84-155">No se recomienda su uso.</span><span class="sxs-lookup"><span data-stu-id="efc84-155">Using them is not recommended.</span></span> <span data-ttu-id="efc84-156">Estos perfiles son las sucesivas versiones y el comportamiento del servicio puede cambiar en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="efc84-156">These profiles are rolling versions and service behavior may change at any time.</span></span>

## <a name="next-steps"></a><span data-ttu-id="efc84-157">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="efc84-157">Next steps</span></span>

<span data-ttu-id="efc84-158">Para empezar a usar el SDK de Azure para Go, pruebe un inicio rápido.</span><span class="sxs-lookup"><span data-stu-id="efc84-158">To begin using the Azure SDK for Go, try out a quickstart.</span></span>

* [<span data-ttu-id="efc84-159">Implementación de una máquina virtual desde una plantilla</span><span class="sxs-lookup"><span data-stu-id="efc84-159">Deploy a virtual machine from a template</span></span>](azure-sdk-go-qs-vm.md)
* [<span data-ttu-id="efc84-160">Transferencia de objetos a Azure Blob Storage con el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="efc84-160">Transfer objects to Azure Blob Storage with the Azure Blob SDK for Go</span></span>](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [<span data-ttu-id="efc84-161">Conexión a Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="efc84-161">Connect to Azure Database for PostgreSQL</span></span>](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

<span data-ttu-id="efc84-162">Si desea empezar a trabajar con otros servicios del SDK para Go inmediatamente, eche un vistazo a algunos de los ejemplos de código disponibles.</span><span class="sxs-lookup"><span data-stu-id="efc84-162">If you want to get started with other services in the Go SDK immediately, take a look at some of the available sample code.</span></span>

* [<span data-ttu-id="efc84-163">Autenticación con los servicios de Azure</span><span class="sxs-lookup"><span data-stu-id="efc84-163">Authenticate with Azure services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [<span data-ttu-id="efc84-164">Implementación de nuevas máquinas virtuales con autenticación SSH</span><span class="sxs-lookup"><span data-stu-id="efc84-164">Deploy new virtual machines with SSH authentication</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [<span data-ttu-id="efc84-165">Implementación de una imagen de contenedor en Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="efc84-165">Deploy a container image to Azure Container Instances</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [<span data-ttu-id="efc84-166">Creación de un clúster en el servicio de Kubernetes de Azure</span><span class="sxs-lookup"><span data-stu-id="efc84-166">Create a cluster in Azure Kubernetes Service</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [<span data-ttu-id="efc84-167">Uso de los servicios de Azure Storage</span><span class="sxs-lookup"><span data-stu-id="efc84-167">Work with Azure Storage services</span></span>](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [<span data-ttu-id="efc84-168">Todos los ejemplos para el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="efc84-168">All samples for the Azure SDK for Go</span></span>](https://github.com/azure-samples/azure-sdk-for-go-samples)
