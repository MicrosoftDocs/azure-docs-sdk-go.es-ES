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
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a><span data-ttu-id="dec4e-104">Herramientas para los desarrolladores que utilizan el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="dec4e-104">Tools for developers using the Azure SDK for Go</span></span>

<span data-ttu-id="dec4e-105">Estas son algunas herramientas recomendadas para escribir código de Go eficiente y hacer que funcione sin problemas con los servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="dec4e-105">To be effective at writing Go code and have it work seamlessly with Azure services, here are some recommended tools.</span></span>

## <a name="azure-cli-20"></a><span data-ttu-id="dec4e-106">CLI de Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="dec4e-106">Azure CLI 2.0</span></span>

<span data-ttu-id="dec4e-107">La CLI de Azure 2.0 proporciona una interfaz de la línea de comandos para crear y configurar recursos de Azure en las suscripciones.</span><span class="sxs-lookup"><span data-stu-id="dec4e-107">The Azure 2.0 CLI provides a command-line interface to create and configure Azure resources in your subscriptions.</span></span> <span data-ttu-id="dec4e-108">La CLI le permitirá empezar a crear recursos de Azure comunes y compartidos rápidamente, para que pueda centrarse en el uso más complejo de los servicios.</span><span class="sxs-lookup"><span data-stu-id="dec4e-108">The CLI can help you get started building common and shared Azure resources quickly, so that you can focus on more complex usage of services.</span></span> <span data-ttu-id="dec4e-109">La CLI tiene características de consulta y filtrado, por lo que puede canalizar la salida directamente a sus herramientas de la línea de comandos favoritas.</span><span class="sxs-lookup"><span data-stu-id="dec4e-109">The CLI has query and filtering features so you can pipe output directly to your favorite command-line tools.</span></span> <span data-ttu-id="dec4e-110">La CLI está disponible para su instalación en el sistema local como una imagen de Docker o a través de [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span><span class="sxs-lookup"><span data-stu-id="dec4e-110">The CLI is available for installation on your local system, as a Docker image, or through [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dec4e-111">Instalación de la CLI de Azure 2.0</span><span class="sxs-lookup"><span data-stu-id="dec4e-111">Install the Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a><span data-ttu-id="dec4e-112">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dec4e-112">Visual Studio Code</span></span>

<span data-ttu-id="dec4e-113">Visual Studio Code es un editor ligero que tiene compatibilidad completa para el lenguaje Go a través de las extensiones.</span><span class="sxs-lookup"><span data-stu-id="dec4e-113">Visual Studio Code is a lightweight editor that has comprehensive support for the Go language through extensions.</span></span> <span data-ttu-id="dec4e-114">Estas extensiones incluyen compatibilidad con características como autocompletar, plantillas `impl`, refactorización y depuración.</span><span class="sxs-lookup"><span data-stu-id="dec4e-114">These extensions include support for features like autocomplete, `impl` templates, refactoring, and debugging.</span></span> <span data-ttu-id="dec4e-115">Visual Studio Code también proporciona muchas extensiones para las herramientas de desarrollo más comunes, como el control de código fuente y ofrece incluso extensiones para las interacciones directas con los servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="dec4e-115">Visual Studio Code also offers many extensions for common developer tools such as source control, and even offers extensions for direct interactions with Azure services.</span></span> <span data-ttu-id="dec4e-116">Microsoft mantiene una metaextensión oficial que incluye estas extensiones de Azure, incluida una interfaz interactiva para la CLI de Azure.</span><span class="sxs-lookup"><span data-stu-id="dec4e-116">Microsoft maintains an official meta-extension including these Azure extensions, including an interactive interface for the Azure CLI.</span></span>

* [<span data-ttu-id="dec4e-117">Instalación de Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dec4e-117">Install Visual Studio Code</span></span>](https://code.visualstudio.com/Download)
* [<span data-ttu-id="dec4e-118">Obtención de la extensión de Visual Studio Code para Go</span><span class="sxs-lookup"><span data-stu-id="dec4e-118">Get the Visual Studio Code Go extension</span></span>](https://code.visualstudio.com/docs/languages/go)
* [<span data-ttu-id="dec4e-119">Obtención de la extensión de herramientas de Azure</span><span class="sxs-lookup"><span data-stu-id="dec4e-119">Get the Azure Tools extension</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="dependency-management-with-dep"></a><span data-ttu-id="dec4e-120">Administración de dependencias con dep</span><span class="sxs-lookup"><span data-stu-id="dec4e-120">Dependency management with dep</span></span>

<span data-ttu-id="dec4e-121">Hay muchas maneras de administrar las dependencias del paquete e incluir los archivos dependientes en la carpeta vendor con Go, ya que todavía no hay ninguna solución oficial.</span><span class="sxs-lookup"><span data-stu-id="dec4e-121">There are many ways to manage your package dependencies and do vendoring with Go, since there is no official solution yet.</span></span> <span data-ttu-id="dec4e-122">El modo recomendado para realizar esta administración es con el administrador de dependencias `dep`.</span><span class="sxs-lookup"><span data-stu-id="dec4e-122">The recommended way to perform this management is with the `dep` dependency manager.</span></span> <span data-ttu-id="dec4e-123">El SDK de Azure para Go usa dep para incluir los archivos dependientes en la carpeta vendor. Dep garantiza que se obtendrán correctamente las dependencias de cualquier otro proyecto.</span><span class="sxs-lookup"><span data-stu-id="dec4e-123">The Azure SDK for Go uses dep for its vendoring, and is guaranteed to correctly get dependencies for any other project using dep.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dec4e-124">Obtención del administrador de dependencias dep</span><span class="sxs-lookup"><span data-stu-id="dec4e-124">Get the dep dependency manager</span></span>](https://github.com/tools/godep)

## <a name="telemetry-with-application-insights"></a><span data-ttu-id="dec4e-125">Telemetría con Application Insights</span><span class="sxs-lookup"><span data-stu-id="dec4e-125">Telemetry with Application Insights</span></span>

<span data-ttu-id="dec4e-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) es un producto de análisis que permite recopilar información de telemetría de las aplicaciones fácilmente y se integra con el ecosistema de Azure, Visual Studio Team Services y GitHub.</span><span class="sxs-lookup"><span data-stu-id="dec4e-126">[Application Insights](https://azure.microsoft.com/en-us/services/application-insights/) is an analytics product that allows you to easily collect telemetry information from your applications and integrates with the Azure ecosystem, Visual Studio Team Services, and GitHub.</span></span> <span data-ttu-id="dec4e-127">Se puede utilizar en muchas aplicaciones y Microsoft ofrece un SDK para Go para trabajar con Application Insights.</span><span class="sxs-lookup"><span data-stu-id="dec4e-127">It can be used in many applications, and Microsoft offers a Go SDK for working with Application Insights.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="dec4e-128">Obtención del SDK de Application Insights para Go</span><span class="sxs-lookup"><span data-stu-id="dec4e-128">Get the Application Insights for Go SDK</span></span>](https://github.com/Microsoft/ApplicationInsights-Go) 
