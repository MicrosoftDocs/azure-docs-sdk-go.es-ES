---
title: Herramientas para los desarrolladores que utilizan el SDK de Azure para Go
description: Herramientas para trabajar con el SDK de Azure para Go y los servicios de Azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.prod: azure
ms.technology: azure-sdk-go
ms.devlang: go
ms.openlocfilehash: 70cf7d645f47df29e8e42599a0acd75858144783
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059210"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="5981b-103">Herramientas para los desarrolladores que utilizan el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="5981b-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="5981b-104">Estas son algunas herramientas recomendadas para escribir código de Go eficiente y hacer que funcione sin problemas con los servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="5981b-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="5981b-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5981b-105">Azure CLI</span></span>

<span data-ttu-id="5981b-106">La CLI de Azure proporciona una interfaz de la línea de comandos para crear y configurar recursos de Azure en las suscripciones.</span><span class="sxs-lookup"><span data-stu-id="5981b-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="5981b-107">La CLI le permitirá empezar a crear recursos de Azure comunes y compartidos rápidamente, para que pueda centrarse en el uso más complejo de los servicios.</span><span class="sxs-lookup"><span data-stu-id="5981b-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="5981b-108">La CLI tiene características de consulta y filtrado, por lo que puede canalizar la salida directamente a sus herramientas de la línea de comandos favoritas.</span><span class="sxs-lookup"><span data-stu-id="5981b-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="5981b-109">La CLI está disponible para su instalación en el sistema local como una imagen de Docker o a través de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="5981b-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5981b-110">Instalación de la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="5981b-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="5981b-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5981b-111">Visual Studio Code</span></span>

<span data-ttu-id="5981b-112">Visual Studio Code es un editor ligero que ofrece compatibilidad para Go.</span><span class="sxs-lookup"><span data-stu-id="5981b-112">Visual Studio Code is a lightweight editor that offers Go support.</span></span> <span data-ttu-id="5981b-113">Esta extensión incluye compatibilidad con características como autocompletar, plantillas de `impl`, refactorización y depuración.</span><span class="sxs-lookup"><span data-stu-id="5981b-113">This extension offers features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="5981b-114">Visual Studio Code también permite el acceso en el editor para el control de código fuente, así como extensiones para trabajar con servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="5981b-114">Visual Studio Code also offers support for in-editor access to source control, and extensions for working with Azure services.</span></span>

* [<span data-ttu-id="5981b-115">Instalación de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5981b-115">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="5981b-116">Obtención de la extensión de Visual Studio Code para Go</span><span class="sxs-lookup"><span data-stu-id="5981b-116">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="5981b-117">Obtención de la extensión de Visual Studio Code Azure Tools</span><span class="sxs-lookup"><span data-stu-id="5981b-117">Get the Visual Studio Code Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="5981b-118">Integración continua y entrega continua con Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="5981b-118">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="5981b-119">Canalizaciones de Azure DevOps Projects le permiten configurar un sistema de integración continua para las aplicaciones de Go.</span><span class="sxs-lookup"><span data-stu-id="5981b-119">Azure DevOps Project pipelines allow you to set up a continuous integration system for your Go applications.</span></span> <span data-ttu-id="5981b-120">Solo se necesita un repositorio de Git y puede implementar y probar directamente en Azure.</span><span class="sxs-lookup"><span data-stu-id="5981b-120">All it takes is a git repo, and you can deploy and test directly on Azure.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5981b-121">Aprenda cómo crear una canalización de CI/CD con Azure DevOps Projects</span><span class="sxs-lookup"><span data-stu-id="5981b-121">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="5981b-122">Administración de dependencias con dep</span><span class="sxs-lookup"><span data-stu-id="5981b-122">Dependency management with dep</span></span>

<span data-ttu-id="5981b-123">Azure SDK para Go usa dep para la administración de dependencias.</span><span class="sxs-lookup"><span data-stu-id="5981b-123">The Azure SDK for Go uses dep for dependency management.</span></span> <span data-ttu-id="5981b-124">El comando dep permite incorporar los cambios y preparar la aplicación de Go, evitar conflictos de versión y asegurarse de que el proyecto funcionará correctamente.</span><span class="sxs-lookup"><span data-stu-id="5981b-124">The dep command allows you to pull and vendor requirements for your Go application, avoiding version conflicts and ensuring that your project works correctly.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5981b-125">Obtención del administrador de dependencias dep</span><span class="sxs-lookup"><span data-stu-id="5981b-125">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
