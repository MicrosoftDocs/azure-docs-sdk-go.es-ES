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
# <a name="install-the-azure-sdk-for-go"></a>Instalación del SDK de Azure para Go

Le damos la bienvenida al SDK de Azure para Go El SDK permite administrar los servicios de Azure desde las aplicaciones de Go, e interactuar con ellos.

## <a name="get-the-azure-sdk-for-go"></a>Obtención del SDK de Azure para Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Algunos servicios de Azure tienen sus propios SDK de Go y no se incluyen en el paquete principal del SDK de Azure para Go. En la tabla siguiente se enumeran los servicios que tienen sus propios SDK, y sus nombres de paquete. Todos estos paquetes están en versión preliminar.

| Servicio | Paquete |
|---------|---------|
| Blob Storage | [github.com/Azure/azure-storage-blob-go](https://github.com/Azure/azure-storage-blob-go) |
| File Storage | [github.com/Azure/azure-storage-file-go](https://github.com/Azure/azure-storage-file-go) |
| Cola de almacenamiento | [github.com/Azure/azure-storage-queue-go](https://github.com/Azure/azure-storage-queue-go) |
| Centro de eventos | [github.com/Azure/azure-event-hubs-go](https://github.com/Azure/azure-event-hubs-go) |
| Azure Service Bus | [github.com/Azure/azure-service-bus-go](https://github.com/Azure/azure-service-bus-go) |
| Application Insights | [github.com/Microsoft/ApplicationInsights-go](https://github.com/Microsoft/ApplicationInsights-go) |

## <a name="vendor-the-azure-sdk-for-go"></a>Inclusión del SDK de Azure para Go en la carpeta vendor

Los archivos dependientes se pueden incluir en la carpeta vendor del SDK de Azure para Go con [dep](https://github.com/golang/dep). Por motivos de estabilidad, se recomienda incluir los archivos dependientes en la carpeta vendor. Para usar `dep` en su propio proyecto, agregue `github.com/Azure/azure-sdk-for-go` a una sección `[[constraint]]` de su `Gopkg.toml`. Por ejemplo, para incluir en la carpeta vendor los archivos dependientes de la versión `14.0.0`, agregue la entrada siguiente:

```toml
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="include-the-azure-sdk-for-go-in-your-project"></a>Inclusión del SDK de Azure para Go en el proyecto

Para usar los servicios de Azure desde el código de Go, importe todos los servicios con lo que interactúa y los módulos `autorest` necesarios.
Puede obtener una lista completa de los módulos disponibles en GoDoc para los [servicios disponibles](https://godoc.org/github.com/Azure/azure-sdk-for-go) y los [paquetes de AutoRest](https://godoc.org/github.com/Azure/go-autorest). Los paquetes más comunes que necesita de `go-autorest` son:

| Paquete | DESCRIPCIÓN |
|---------|-------------|
| [github.com/Azure/go-autorest/autorest][autorest] | Objetos para el control de la autenticación del cliente del servicio |
| [github.com/Azure/go-autorest/autorest/azure][autorest/azure] | Constantes para las interacciones con los servicios de Azure |
| [github.com/Azure/go-autorest/autorest/adal][autorest/adal] | Mecanismos de autenticación para el acceso a los servicios de Azure |
| [github.com/Azure/go-autorest/autorest/to][autorest/to] | Aplicaciones auxiliares de aserción de tipos para trabajar con las estructuras de datos del SDK de Azure |

[autorest]: https://godoc.org/github.com/Azure/go-autorest/autorest
[autorest/azure]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure
[autorest/adal]: https://godoc.org/github.com/Azure/go-autorest/autorest/adal
[autorest/to]: https://godoc.org/github.com/Azure/go-autorest/autorest/to

Las versiones de los paquetes de Go y los servicios de Azure son independientes. Las versiones de los servicios son parte de la ruta de importación del módulo, en el módulo `services`. La ruta de acceso completa del módulo es el nombre del servicio, seguido de la versión en formato `YYYY-MM-DD` y seguido del nombre de servicio de nuevo. Por ejemplo, para importar la versión `2017-03-30` del servicio Compute:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

Se recomienda usar la versión más reciente de un servicio al comenzar a desarrollar y mantener la coherencia.
Los requisitos del servicio pueden cambiar de una versión a otra y estos cambios podrían afectar al código, aunque no haya ninguna actualización de Azure SDK para Go durante ese tiempo.

Si necesita una instantánea colectiva de los servicios, también puede seleccionar una versión de perfil único. En este momento, el único perfil bloqueado es la versión `2017-03-09`, que puede no tener las características más recientes de los servicios. Los perfiles se encuentran en el módulo `profiles`, con la versión en formato `YYYY-MM-DD`. Los servicios se agrupan bajo su versión de perfil. Por ejemplo, para importar el módulo de administración de recursos de Azure desde el perfil `2017-03-09`:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> También están disponibles los perfiles `preview` y `latest`. No se recomienda su uso. Estos perfiles son las sucesivas versiones y el comportamiento del servicio puede cambiar en cualquier momento.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a usar el SDK de Azure para Go, pruebe un inicio rápido.

* [Implementación de una máquina virtual desde una plantilla](azure-sdk-go-qs-vm.md)
* [Transferencia de objetos a Azure Blob Storage con el SDK de Azure para Go](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Conexión a Azure Database for PostgreSQL](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Si desea empezar a trabajar con otros servicios del SDK para Go inmediatamente, eche un vistazo a algunos de los ejemplos de código disponibles.

* [Autenticación con los servicios de Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/internal/iam)
* [Implementación de nuevas máquinas virtuales con autenticación SSH](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Implementación de una imagen de contenedor en Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [Creación de un clúster en el servicio de Kubernetes de Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [Uso de los servicios de Azure Storage](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Todos los ejemplos para el SDK de Azure para Go](https://github.com/azure-samples/azure-sdk-for-go-samples)
