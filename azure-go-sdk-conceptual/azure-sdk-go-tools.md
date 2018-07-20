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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="21939-103">Herramientas para los desarrolladores que utilizan el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="21939-103">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="21939-104">Estas son algunas herramientas recomendadas para escribir código de Go eficiente y hacer que funcione sin problemas con los servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="21939-104">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli"></a><span data-ttu-id="21939-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="21939-105">Azure CLI</span></span>

<span data-ttu-id="21939-106">La CLI de Azure proporciona una interfaz de la línea de comandos para crear y configurar recursos de Azure en las suscripciones.</span><span class="sxs-lookup"><span data-stu-id="21939-106">The Azure CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="21939-107">La CLI le permitirá empezar a crear recursos de Azure comunes y compartidos rápidamente, para que pueda centrarse en el uso más complejo de los servicios.</span><span class="sxs-lookup"><span data-stu-id="21939-107">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="21939-108">La CLI tiene características de consulta y filtrado, por lo que puede canalizar la salida directamente a sus herramientas de la línea de comandos favoritas.</span><span class="sxs-lookup"><span data-stu-id="21939-108">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="21939-109">La CLI está disponible para su instalación en el sistema local como una imagen de Docker o a través de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="21939-109">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="21939-110">Instalación de la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="21939-110">Install the Azure CLI</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="21939-111">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="21939-111">Visual Studio Code</span></span>

<span data-ttu-id="21939-112">Visual Studio Code es un editor ligero que tiene compatibilidad completa para el lenguaje Go a través de las extensiones.</span><span class="sxs-lookup"><span data-stu-id="21939-112">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="21939-113">Estas extensiones incluyen compatibilidad con características como autocompletar, plantillas `impl`, refactorización y depuración.</span><span class="sxs-lookup"><span data-stu-id="21939-113">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="21939-114">Visual Studio Code también proporciona muchas extensiones para las herramientas de desarrollo más comunes, como el control de código fuente y ofrece incluso extensiones para las interacciones directas con los servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="21939-114">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="21939-115">Microsoft mantiene una metaextensión oficial que incluye estas extensiones de Azure, incluida una interfaz interactiva para la CLI de Azure.</span><span class="sxs-lookup"><span data-stu-id="21939-115">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="21939-116">Instalación de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="21939-116">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="21939-117">Obtención de la extensión de Visual Studio Code para Go</span><span class="sxs-lookup"><span data-stu-id="21939-117">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="21939-118">Obtención de la extensión de herramientas de Azure</span><span class="sxs-lookup"><span data-stu-id="21939-118">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a><span data-ttu-id="21939-119">Integración continua y entrega continua con Azure DevOps Project</span><span class="sxs-lookup"><span data-stu-id="21939-119">CI/CD with Azure DevOps Project</span></span>

<span data-ttu-id="21939-120">Con la canalización de Azure DevOps Projects, puede configurar una compilación e implementación continuas para las aplicaciones de Go.</span><span class="sxs-lookup"><span data-stu-id="21939-120">With the Azure DevOps Project pipeline, you can set up a continuous build and deploy for your Go applications.</span></span> <span data-ttu-id="21939-121">Todo lo que necesita es un repositorio de git disponible y ya puede configurar la implementación y prueba directamente en los recursos de Azure.</span><span class="sxs-lookup"><span data-stu-id="21939-121">All you need is an available git repo, and you can set up to deploy and test directly on your Azure resources.</span></span> <span data-ttu-id="21939-122">La canalización de la configuración es fácil de crear y administrar y, como se aprovisiona directamente en Azure, puede controlarla igual que controla los recursos de Azure.</span><span class="sxs-lookup"><span data-stu-id="21939-122">The configuration pipeline is easy to create and manage, and since it's provisioned directly on Azure, you can control it in the same way that you handle your other Azure resources.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="21939-123">Aprenda cómo crear una canalización de CI/CD con Azure DevOps Projects</span><span class="sxs-lookup"><span data-stu-id="21939-123">Learn how to create a CI/CD pipeline with Azure DevOps Project</span></span>](/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="21939-124">Administración de dependencias con dep</span><span class="sxs-lookup"><span data-stu-id="21939-124">Dependency management with dep</span></span>

<span data-ttu-id="21939-125">Hay muchas maneras de administrar las dependencias del paquete e incluir los archivos dependientes en la carpeta vendor con Go, ya que todavía no hay ninguna solución oficial.</span><span class="sxs-lookup"><span data-stu-id="21939-125">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="21939-126">El modo recomendado para realizar esta administración es con el administrador de dependencias `dep`.</span><span class="sxs-lookup"><span data-stu-id="21939-126">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="21939-127">El SDK de Azure para Go usa dep para incluir los archivos dependientes en la carpeta vendor. Dep garantiza que se obtendrán correctamente las dependencias de cualquier otro proyecto.</span><span class="sxs-lookup"><span data-stu-id="21939-127">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="21939-128">Obtención del administrador de dependencias dep</span><span class="sxs-lookup"><span data-stu-id="21939-128">Get the dep dependency manager</span></span>](https://github.com/golang/dep)
