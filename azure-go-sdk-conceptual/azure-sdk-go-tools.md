---
title: Herramientas para desarrolladores de Go
description: Herramientas para trabajar con el SDK de Azure para Go y los servicios de Azure
keywords: azure, go, golang, azure, visual studio, visual studio code
author: sptramer
ms.author: sttramer
ms.date: 01/30/2018
ms.topic: article
ms.devlang: go
manager: routlaw
ms.openlocfilehash: 4753775e608b39c6da43d64fd08c1532e03d5810
ms.sourcegitcommit: aaa8c37880332625f858a38f5918e6cf581bf48d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2018
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Herramientas para los desarrolladores que utilizan el SDK de Azure para Go

Estas son algunas herramientas recomendadas para escribir código de Go eficiente y hacer que funcione sin problemas con los servicios de Azure.

## <a name="azure-cli-20"></a>CLI de Azure 2.0

La CLI de Azure 2.0 proporciona una interfaz de la línea de comandos para crear y configurar recursos de Azure en las suscripciones. La CLI le permitirá empezar a crear recursos de Azure comunes y compartidos rápidamente, para que pueda centrarse en el uso más complejo de los servicios. La CLI tiene características de consulta y filtrado, por lo que puede canalizar la salida directamente a sus herramientas de la línea de comandos favoritas. La CLI está disponible para su instalación en el sistema local como una imagen de Docker o a través de [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code es un editor ligero que tiene compatibilidad completa para el lenguaje Go a través de las extensiones. Estas extensiones incluyen compatibilidad con características como autocompletar, plantillas `impl`, refactorización y depuración. Visual Studio Code también proporciona muchas extensiones para las herramientas de desarrollo más comunes, como el control de código fuente y ofrece incluso extensiones para las interacciones directas con los servicios de Azure. Microsoft mantiene una metaextensión oficial que incluye estas extensiones de Azure, incluida una interfaz interactiva para la CLI de Azure.

* [Instalación de Visual Studio Code](https://code.visualstudio.com/Download)
* [Obtención de la extensión de Visual Studio Code para Go](https://code.visualstudio.com/docs/languages/go)
* [Obtención de la extensión de herramientas de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a>Administración de dependencias con dep

Hay muchas maneras de administrar las dependencias del paquete e incluir los archivos dependientes en la carpeta vendor con Go, ya que todavía no hay ninguna solución oficial. El modo recomendado para realizar esta administración es con el administrador de dependencias `dep`. El SDK de Azure para Go usa dep para incluir los archivos dependientes en la carpeta vendor. Dep garantiza que se obtendrán correctamente las dependencias de cualquier otro proyecto.

> [!div class="nextstepaction"]
> [Obtención del administrador de dependencias dep](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a>Telemetría con Application Insights

[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) es un producto de análisis que permite recopilar información de telemetría de las aplicaciones fácilmente y se integra con el ecosistema de Azure, Visual Studio Team Services y GitHub. Se puede utilizar en muchas aplicaciones y Microsoft ofrece un SDK para Go para trabajar con Application Insights.

> [!div class="nextstepaction"]
> [Obtención del SDK de Application Insights para Go](https://github.com/Microsoft/ApplicationInsights-Go) 
