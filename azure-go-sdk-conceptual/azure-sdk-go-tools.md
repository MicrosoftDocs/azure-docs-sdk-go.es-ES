---
title: Herramientas para desarrolladores de Go
description: Herramientas para trabajar con el SDK de Azure para Go y los servicios de Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 07/13/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: dfa3912ac13e6f6d52d607f9dcc150f3a5b57602
ms.sourcegitcommit: d1790b317a8fcb4d672c654dac2a925a976589d4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39039512"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Herramientas para los desarrolladores que utilizan el SDK de Azure para Go

Estas son algunas herramientas recomendadas para escribir código de Go eficiente y hacer que funcione sin problemas con los servicios de Azure.

## <a name="azure-cli"></a>Azure CLI

La CLI de Azure proporciona una interfaz de la línea de comandos para crear y configurar recursos de Azure en las suscripciones. La CLI le permitirá empezar a crear recursos de Azure comunes y compartidos rápidamente, para que pueda centrarse en el uso más complejo de los servicios. La CLI tiene características de consulta y filtrado, por lo que puede canalizar la salida directamente a sus herramientas de la línea de comandos favoritas. La CLI está disponible para su instalación en el sistema local como una imagen de Docker o a través de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Instalación de la CLI de Azure](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code es un editor ligero que tiene compatibilidad completa para el lenguaje Go a través de las extensiones. Estas extensiones incluyen compatibilidad con características como autocompletar, plantillas `impl`, refactorización y depuración. Visual Studio Code también proporciona muchas extensiones para las herramientas de desarrollo más comunes, como el control de código fuente y ofrece incluso extensiones para las interacciones directas con los servicios de Azure. Microsoft mantiene una metaextensión oficial que incluye estas extensiones de Azure, incluida una interfaz interactiva para la CLI de Azure.

* [Instalación de Visual Studio Code](https://code.visualstudio.com/Download)
* [Obtención de la extensión de Visual Studio Code para Go](https://code.visualstudio.com/docs/languages/go)
* [Obtención de la extensión de herramientas de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>Integración continua y entrega continua con Azure DevOps Project

Con la canalización de Azure DevOps Projects, puede configurar una compilación e implementación continuas para las aplicaciones de Go. Todo lo que necesita es un repositorio de git disponible y ya puede configurar la implementación y prueba directamente en los recursos de Azure. La canalización de la configuración es fácil de crear y administrar y, como se aprovisiona directamente en Azure, puede controlarla igual que controla los recursos de Azure.

> [!div class="nextstepaction"]
> [Aprenda cómo crear una canalización de CI/CD con Azure DevOps Projects](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Administración de dependencias con dep

Hay muchas maneras de administrar las dependencias del paquete e incluir los archivos dependientes en la carpeta vendor con Go, ya que todavía no hay ninguna solución oficial. El modo recomendado para realizar esta administración es con el administrador de dependencias `dep`. El SDK de Azure para Go usa dep para incluir los archivos dependientes en la carpeta vendor. Dep garantiza que se obtendrán correctamente las dependencias de cualquier otro proyecto.

> [!div class="nextstepaction"]
> [Obtención del administrador de dependencias dep](https://github.com/golang/dep)
