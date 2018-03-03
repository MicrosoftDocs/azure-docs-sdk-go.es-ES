---
title: "Instalación del SDK de Azure para Go"
description: "Cómo instalar y configurar el SDK de Azure para Go e incluir los archivos dependientes en la carpeta vendor."
keywords: azure, sdk, go, golang, azure sdk
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 7fc0a3ff71b0b06f616ae43cff311352fe873345
ms.sourcegitcommit: 890f5f01a70e7e376e6bb98a2030afbfc016f538
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2018
---
# <a name="installing-the-azure-sdk-for-go"></a>Instalación del SDK de Azure para Go

Le damos la bienvenida al SDK de Azure para Go Este SDK permite administrar los servicios de Azure desde las aplicaciones de Go, e interactuar con ellos.

## <a name="get-the-azure-sdk-for-go"></a>Obtención del SDK de Azure para Go

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

Para trabajar con instancias de Azure Storage Blob es necesario un SDK independiente.

```bash
go get -u -d github.com/Azure/azure-storage-blob-go/...
```

## <a name="vendoring-the-azure-sdk-for-go"></a>Inclusión de los archivos dependientes en la carpeta vendor del SDK de Azure para Go

Los archivos dependientes se pueden incluir en la carpeta vendor del SDK de Azure para Go con [dep](https://github.com/golang/dep). Por motivos de estabilidad, se recomienda incluir los archivos dependientes en la carpeta vendor. Para usar la compatibilidad con `dep`, agregue `github.com/Azure/azure-sdk-for-go` a una sección `[[constraint]]` de su archivo `Gopkg.toml`. Por ejemplo, para incluir en la carpeta vendor los archivos dependientes de la versión `14.0.0`, agregue la entrada siguiente:

```
[[constraint]]
name = "github.com/Azure/azure-sdk-for-go"
version = "14.0.0"
```

## <a name="including-the-azure-sdk-for-go-in-your-project"></a>Inclusión del SDK de Azure para Go en el proyecto

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

Los módulos para los servicios de Azure tienen versiones independientes de las API del SDK que existen para ellos. Estas versiones son parte de la ruta de acceso de importación del módulo y pueden ser una _versión del servicio_ o un _perfil_. Actualmente, se recomienda que utilice una versión del servicio específica tanto para el desarrollo como para la versión final. Los servicios se encuentran en el módulo `services`. La ruta de acceso completa para la importación es el nombre del servicio, seguido de la versión en formato `YYYY-MM-DD` y seguido del nombre de servicio de nuevo. Por ejemplo, para incluir la versión `2017-03-30` del servicio de proceso:

```go
import "github.com/Azure/azure-sdk-for-go/services/compute/mgmt/2017-03-30/compute"
```

Actualmente se recomienda que utilice la versión más reciente de un servicio, a menos que tenga una razón para no hacerlo.

Si necesita una instantánea colectiva de los servicios, también puede seleccionar una versión de perfil único. En este momento, el único perfil bloqueado es la versión `2017-03-30`, que puede no tener las características más recientes de los servicios. Los perfiles se encuentran en el módulo `profiles`, con la versión en formato `YYYY-MM-DD`. Los servicios se agrupan bajo su versión de perfil. Por ejemplo, para importar el módulo de administración de recursos de Azure desde el perfil `2017-03-09`:

```go
import "github.com/Azure/azure-sdk-for-go/profiles/2017-03-09/resources/mgmt/resources"
```

> [!WARNING]
> También están disponibles los perfiles `preview` y `latest`. No se recomienda su uso. Estos perfiles son las sucesivas versiones y el comportamiento del servicio puede cambiar en cualquier momento.

## <a name="next-steps"></a>pasos siguientes

Para empezar a usar el SDK de Azure para Go, pruebe un inicio rápido.

* [Implementación de una máquina virtual desde una plantilla](azure-sdk-go-qs-vm.md)
* [Transferencia de objetos a Azure Blob Storage con el SDK de Azure para Go](/azure/storage/blobs/storage-quickstart-blobs-go?toc=%2fgo%2fazure%2ftoc.json)
* [Conexión a Azure Database for PostgreSQL](/azure/postgresql/connect-go?toc=%2fgo%2fazure%2ftoc.json)

Si desea empezar a trabajar con otros servicios del SDK para Go inmediatamente, eche un vistazo a algunos de los ejemplos de código disponibles.

* [Autenticación con los servicios de Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/iam)
* [Implementación de nuevas máquinas virtuales con autenticación SSH](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute)
* [Implementación de una imagen de contenedor en Azure Container Instances](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerinstance)
* [Creación de un clúster en el servicio de Kubernetes de Azure](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/containerservice)
* [Uso de los servicios de Azure Storage](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/storage)
* [Todos los ejemplos para el SDK de Azure para Go](https://github.com/azure-samples/azure-sdk-for-go-samples)
