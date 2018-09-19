---
title: Implementación de una máquina virtual de Azure desde Go
description: Implementación de una máquina virtual mediante el SDK de Azure para Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: quickstart
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: a7970be0857fd414d776241b033af0c23457790c
ms.sourcegitcommit: 8b9e10b960150dc08f046ab840d6a5627410db29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44059142"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="9f381-103">Inicio rápido: implementación de una máquina virtual de Azure desde una plantilla con el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="9f381-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="9f381-104">En esta guía de inicio rápido se muestra cómo implementar recursos desde una plantilla de Azure Resource Manager, mediante Azure SDK para Go.</span><span class="sxs-lookup"><span data-stu-id="9f381-104">This quickstart shows you how to deploy resources from an Azure Resource Manager template, using the Azure SDK for Go.</span></span> <span data-ttu-id="9f381-105">Las plantillas son instantáneas de todos los recursos dentro de un [grupo de recursos de Azure](/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="9f381-105">Templates are snapshots of all of the resources within an [Azure resource group](/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="9f381-106">A lo largo de la guía, se irá familiarizado con la funcionalidad y las convenciones del SDK.</span><span class="sxs-lookup"><span data-stu-id="9f381-106">Along the way, you'll become familiar with the functionality and conventions of the SDK.</span></span>

<span data-ttu-id="9f381-107">Al final de este inicio rápido, tendrá una máquina virtual en ejecución en la que podrá iniciar sesión con un nombre de usuario y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="9f381-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

> [!NOTE]
> <span data-ttu-id="9f381-108">Para ver cómo crear una máquina virtual en Go sin una plantilla de Resource Manager, hay un [ejemplo imperativo](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) que muestra cómo crear y configurar todos los recursos de máquina virtual con el SDK.</span><span class="sxs-lookup"><span data-stu-id="9f381-108">To see the creation of a VM in Go without the use of a Resource Manager template, there is an [imperative sample](https://github.com/Azure-Samples/azure-sdk-for-go-samples/blob/master/compute/vm.go) demonstrating how to build and configure all VM resources with the SDK.</span></span> <span data-ttu-id="9f381-109">Usar una plantilla de este ejemplo permite centrarse en las convenciones del SDK sin entrar en demasiados detalles acerca de la arquitectura del servicio de Azure.</span><span class="sxs-lookup"><span data-stu-id="9f381-109">Using a template in this sample allows a focus on SDK conventions without getting into too many details about Azure service architecture.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="9f381-110">Si usa una instalación local de la CLI de Azure, esta plantilla de inicio rápido requiere la versión __2.0.28__ de la CLI o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="9f381-110">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="9f381-111">Ejecute `az --version` para asegurarse de que la instalación de la CLI cumple este requisito.</span><span class="sxs-lookup"><span data-stu-id="9f381-111">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="9f381-112">Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="9f381-112">If you need to install or upgrade, see [Install the Azure CLI](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="9f381-113">Instalación del SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="9f381-113">Install the Azure SDK for Go</span></span>

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="9f381-114">Creación de una entidad de servicio</span><span class="sxs-lookup"><span data-stu-id="9f381-114">Create a service principal</span></span>

<span data-ttu-id="9f381-115">Para iniciar sesión de una manera no interactiva en Azure con una aplicación, necesita una entidad de servicio.</span><span class="sxs-lookup"><span data-stu-id="9f381-115">To sign in non-interactively to Azure with an application, you need a service principal.</span></span> <span data-ttu-id="9f381-116">Las entidades de servicio son parte del control de acceso basado en rol (RBAC), que crea una identidad de usuario única.</span><span class="sxs-lookup"><span data-stu-id="9f381-116">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="9f381-117">Para crear una nueva entidad de servicio con la CLI, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="9f381-117">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth > quickstart.auth
```

<span data-ttu-id="9f381-118">Establezca la variable de entorno `AZURE_AUTH_LOCATION` para que sea la ruta de acceso completa de este archivo.</span><span class="sxs-lookup"><span data-stu-id="9f381-118">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="9f381-119">Después, el SDK busca y lee las credenciales directamente desde este archivo, sin que tenga que realizar cambios ni registrar información de la entidad de servicio.</span><span class="sxs-lookup"><span data-stu-id="9f381-119">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="9f381-120">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="9f381-120">Get the code</span></span>

<span data-ttu-id="9f381-121">Obtenga el código de inicio rápido y todas sus dependencias con `go get`.</span><span class="sxs-lookup"><span data-stu-id="9f381-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="9f381-122">No es necesario realizar ninguna modificación en el código fuente si la variable `AZURE_AUTH_LOCATION` está establecida correctamente.</span><span class="sxs-lookup"><span data-stu-id="9f381-122">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="9f381-123">Cuando se ejecuta el programa, carga toda la información de autenticación necesaria desde allí.</span><span class="sxs-lookup"><span data-stu-id="9f381-123">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="9f381-124">Ejecución del código</span><span class="sxs-lookup"><span data-stu-id="9f381-124">Running the code</span></span>

<span data-ttu-id="9f381-125">Puede ejecutar el inicio rápido con el comando `go run`.</span><span class="sxs-lookup"><span data-stu-id="9f381-125">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="9f381-126">Si la implementación se realiza correctamente, verá un mensaje que proporciona el nombre de usuario, la dirección IP y la contraseña para iniciar sesión en la máquina virtual recién creada.</span><span class="sxs-lookup"><span data-stu-id="9f381-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="9f381-127">Conéctese mediante SSH con este equipo para confirmar que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="9f381-127">SSH into this machine to see if it's up and running.</span></span> 

## <a name="cleaning-up"></a><span data-ttu-id="9f381-128">Limpiar</span><span class="sxs-lookup"><span data-stu-id="9f381-128">Cleaning up</span></span>

<span data-ttu-id="9f381-129">Para limpiar los recursos creados en este inicio rápido, puede eliminar el grupo de recursos con la CLI.</span><span class="sxs-lookup"><span data-stu-id="9f381-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

<span data-ttu-id="9f381-130">También elimina la entidad de servicio que se creó.</span><span class="sxs-lookup"><span data-stu-id="9f381-130">Also delete the service principal that was created.</span></span> <span data-ttu-id="9f381-131">En el archivo `quickstart.auth`, hay una clave JSON para `clientId`.</span><span class="sxs-lookup"><span data-stu-id="9f381-131">In the `quickstart.auth` file, there's a JSON key for `clientId`.</span></span> <span data-ttu-id="9f381-132">Copie este valor en la variable de entorno `CLIENT_ID_VALUE` y ejecute el siguiente comando de la CLI de Azure:</span><span class="sxs-lookup"><span data-stu-id="9f381-132">Copy this value to the `CLIENT_ID_VALUE` environment variable and run the following Azure CLI command:</span></span>

```azurecli-interactive
az ad sp delete --id ${CLIENT_ID_VALUE}
```

<span data-ttu-id="9f381-133">Donde proporciona el valor para `CLIENT_ID_VALUE` desde `quickstart.auth`.</span><span class="sxs-lookup"><span data-stu-id="9f381-133">Where you supply the value for `CLIENT_ID_VALUE` from `quickstart.auth`.</span></span>

> [!WARNING]
> <span data-ttu-id="9f381-134">Si no se elimina la entidad de servicio de esta aplicación, se queda activa en su inquilino de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="9f381-134">Failing to delete the service principal for this application leaves it active in your Azure Active Directory tenant.</span></span>
> <span data-ttu-id="9f381-135">Aunque el nombre y la contraseña de la entidad de servicio se generan como valores UUID, asegúrese de seguir los procedimientos recomendados de seguridad y elimine las entidades de servicio y las aplicaciones de Azure Active Directory que no use.</span><span class="sxs-lookup"><span data-stu-id="9f381-135">While both the name and password for the service principal are generated as UUIDs, make sure that you follow good security practices by deleting any unused service principals and Azure Active Directory Applications.</span></span>

## <a name="code-in-depth"></a><span data-ttu-id="9f381-136">El código en profundidad</span><span class="sxs-lookup"><span data-stu-id="9f381-136">Code in depth</span></span>

<span data-ttu-id="9f381-137">El contenido del código de inicio rápido se desglosa en un bloque de variables y varias funciones pequeñas, cada uno de los cuales se trata aquí.</span><span class="sxs-lookup"><span data-stu-id="9f381-137">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="9f381-138">Variables, constantes y tipos</span><span class="sxs-lookup"><span data-stu-id="9f381-138">Variables, constants, and types</span></span>

<span data-ttu-id="9f381-139">Puesto que la plantilla de inicio rápido es independiente, usa variables y constantes globales.</span><span class="sxs-lookup"><span data-stu-id="9f381-139">Since quickstart is self-contained, it uses global constants and variables.</span></span>

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

// Information loaded from the authorization file to identify the client
type clientInfo struct {
    SubscriptionID string
    VMPassword     string
}

var (
    ctx        = context.Background()
    clientData clientInfo
    authorizer autorest.Authorizer
)
```

<span data-ttu-id="9f381-140">Los valores declarados proporcionan los nombres de los recursos creados.</span><span class="sxs-lookup"><span data-stu-id="9f381-140">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="9f381-141">La ubicación también se especifica aquí y puede cambiarla para ver cómo se comportan las implementaciones en otros centros de datos.</span><span class="sxs-lookup"><span data-stu-id="9f381-141">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="9f381-142">No todos los centros de datos tienen todos los recursos necesarios disponibles.</span><span class="sxs-lookup"><span data-stu-id="9f381-142">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="9f381-143">El tipo `clientInfo` guarda toda la información que se carga desde el archivo de autenticación para configurar los clientes en el SDK y establecer la contraseña de la máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="9f381-143">The `clientInfo` type holds the information loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="9f381-144">Las constantes `templateFile` y `parametersFile` apuntan a los archivos necesarios para la implementación.</span><span class="sxs-lookup"><span data-stu-id="9f381-144">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="9f381-145">El SDK para Go configurará `authorizer` para autenticación, y la variable `ctx` es un [contexto de Go](https://blog.golang.org/context) para las operaciones de red.</span><span class="sxs-lookup"><span data-stu-id="9f381-145">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="9f381-146">Autenticación e inicialización</span><span class="sxs-lookup"><span data-stu-id="9f381-146">Authentication and initialization</span></span>

<span data-ttu-id="9f381-147">La función `init` establece la autenticación.</span><span class="sxs-lookup"><span data-stu-id="9f381-147">The `init` function sets up authentication.</span></span> <span data-ttu-id="9f381-148">Puesto que la autenticación es una condición previa para todo en la plantilla de inicio rápido, tiene sentido que forme parte de la inicialización.</span><span class="sxs-lookup"><span data-stu-id="9f381-148">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="9f381-149">También carga alguna información necesaria desde el archivo de autenticación para configurar los clientes y la máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="9f381-149">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

```go
func init() {
    var err error
    authorizer, err = auth.NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v", err)
    }

    authInfo, err := readJSON(os.Getenv("AZURE_AUTH_LOCATION"))
    clientData.SubscriptionID = (*authInfo)["subscriptionId"].(string)
    clientData.VMPassword = (*authInfo)["clientSecret"].(string)
}
```

<span data-ttu-id="9f381-150">En primer lugar, se invoca a [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) para cargar la información de autenticación del archivo se encuentra en `AZURE_AUTH_LOCATION`.</span><span class="sxs-lookup"><span data-stu-id="9f381-150">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="9f381-151">Después, la función `readJSON` carga manualmente este archivo (se omite aquí) para extraer los dos valores necesarios para ejecutar el resto del programa: el identificador de la suscripción del cliente y secreto de la entidad de servicio, que también se utiliza para la contraseña de la máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="9f381-151">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="9f381-152">Para simplificar el inicio rápido, se reutiliza la contraseña de la entidad de servicio.</span><span class="sxs-lookup"><span data-stu-id="9f381-152">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="9f381-153">En producción, no olvide que __nunca__ se reutiliza una contraseña que da acceso a los recursos de Azure.</span><span class="sxs-lookup"><span data-stu-id="9f381-153">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="9f381-154">Flujo de operaciones en main()</span><span class="sxs-lookup"><span data-stu-id="9f381-154">Flow of operations in main()</span></span>

<span data-ttu-id="9f381-155">La función `main` es simple, solo indica el flujo de operaciones y realiza la comprobación de errores.</span><span class="sxs-lookup"><span data-stu-id="9f381-155">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("Created group: %v", *group.Name)

    log.Printf("Starting deployment: %s", deploymentName)
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy: %v", err)
    }
    if result.Name != nil {
        log.Printf("Completed deployment %v: %v", deploymentName, *result.Properties.ProvisioningState)
    } else {
        log.Printf("Completed deployment %v (no data returned to SDK)", deploymentName)
    }
    getLogin()
}
```

<span data-ttu-id="9f381-156">Los pasos que ejecuta el código son, en orden:</span><span class="sxs-lookup"><span data-stu-id="9f381-156">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="9f381-157">Crear el grupo de recursos para la implementación (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="9f381-157">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="9f381-158">Crear la implementación dentro del grupo (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="9f381-158">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="9f381-159">Obtener y mostrar la información de inicio de sesión de la máquina virtual implementada (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="9f381-159">Get and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="9f381-160">Creación del grupo de recursos</span><span class="sxs-lookup"><span data-stu-id="9f381-160">Create the resource group</span></span>

<span data-ttu-id="9f381-161">La función `createGroup` crea el grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="9f381-161">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="9f381-162">Un examen del flujo de llamadas y los argumentos muestra la forma en la que se estructuran las interacciones del servicio en el SDK.</span><span class="sxs-lookup"><span data-stu-id="9f381-162">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

```go
func createGroup() (group resources.Group, err error) {
    groupsClient := resources.NewGroupsClient(clientData.SubscriptionID)
    groupsClient.Authorizer = authorizer

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

<span data-ttu-id="9f381-163">El flujo general de la interacción con un servicio de Azure es:</span><span class="sxs-lookup"><span data-stu-id="9f381-163">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="9f381-164">Crear el cliente mediante el método `service.New*Client()`, donde `*` es el tipo de recurso de `service` con el que desea interactuar.</span><span class="sxs-lookup"><span data-stu-id="9f381-164">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="9f381-165">Esta función siempre tiene un identificador de suscripción.</span><span class="sxs-lookup"><span data-stu-id="9f381-165">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="9f381-166">Establecer el método de autorización para el cliente, lo que le permite interactuar con la API remota.</span><span class="sxs-lookup"><span data-stu-id="9f381-166">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="9f381-167">Realizar la llamada al método en el cliente correspondiente a la API remota.</span><span class="sxs-lookup"><span data-stu-id="9f381-167">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="9f381-168">Los métodos del cliente del servicio normalmente toman el nombre del recurso y un objeto de metadatos.</span><span class="sxs-lookup"><span data-stu-id="9f381-168">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="9f381-169">La función [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se utiliza aquí para realizar una conversión de tipo.</span><span class="sxs-lookup"><span data-stu-id="9f381-169">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="9f381-170">Los parámetros de los métodos del SDK toman casi exclusivamente punteros, por lo que estos métodos se proporcionan para facilitar las conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="9f381-170">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="9f381-171">Consulte la documentación del módulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) para obtener la lista completa y el comportamiento de los convertidores.</span><span class="sxs-lookup"><span data-stu-id="9f381-171">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="9f381-172">El método `groupsClient.CreateOrUpdate` devuelve un puntero a un tipo de datos que representa al grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="9f381-172">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="9f381-173">Un valor de retorno directo de este tipo indica una operación de ejecución breve que está diseñada para ser sincrónica.</span><span class="sxs-lookup"><span data-stu-id="9f381-173">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="9f381-174">En la siguiente sección, verá un ejemplo de una operación de ejecución prolongada y cómo interactuar con ella.</span><span class="sxs-lookup"><span data-stu-id="9f381-174">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="perform-the-deployment"></a><span data-ttu-id="9f381-175">Implementación</span><span class="sxs-lookup"><span data-stu-id="9f381-175">Perform the deployment</span></span>

<span data-ttu-id="9f381-176">Una vez creado el grupo de recursos, es el momento de ejecutar la implementación.</span><span class="sxs-lookup"><span data-stu-id="9f381-176">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="9f381-177">Este código se divide en secciones más pequeñas para hacer énfasis en las diferentes partes de su lógica.</span><span class="sxs-lookup"><span data-stu-id="9f381-177">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

```go
func createDeployment() (deployment resources.DeploymentExtended, err error) {
    template, err := readJSON(templateFile)
    if err != nil {
        return
    }
    params, err := readJSON(parametersFile)
    if err != nil {
        return
    }
    (*params)["vm_password"] = map[string]string{
        "value": clientData.VMPassword,
    }
        // ...
```

<span data-ttu-id="9f381-178">`readJSON` carga los archivos de la implementación, los detalles de esta operación se omiten aquí.</span><span class="sxs-lookup"><span data-stu-id="9f381-178">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="9f381-179">Esta función devuelve un valor del tipo `*map[string]interface{}`, el tipo utilizado para construir los metadatos de la llamada de la implementación de los recursos.</span><span class="sxs-lookup"><span data-stu-id="9f381-179">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="9f381-180">La contraseña de la máquina virtual también se establece manualmente en los parámetros de implementación.</span><span class="sxs-lookup"><span data-stu-id="9f381-180">The VM's password is also set manually on the deployment parameters.</span></span>

```go
        // ...

    deploymentsClient := resources.NewDeploymentsClient(clientData.SubscriptionID)
    deploymentsClient.Authorizer = authorizer

    deploymentFuture, err := deploymentsClient.CreateOrUpdate(
        ctx,
        resourceGroupName,
        deploymentName,
        resources.Deployment{
            Properties: &resources.DeploymentProperties{
                Template:   template,
                Parameters: params,
                Mode:       resources.Incremental,
            },
        },
    )
    if err != nil {
        return
    }
```

<span data-ttu-id="9f381-181">Este código sigue el mismo patrón que la creación del grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="9f381-181">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="9f381-182">Se crea un nuevo cliente, se le habilita para autenticarse con Azure y, a continuación, se llama a un método.</span><span class="sxs-lookup"><span data-stu-id="9f381-182">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span>
<span data-ttu-id="9f381-183">El método incluso tiene el mismo nombre (`CreateOrUpdate`) que el método correspondiente en los grupos de recursos.</span><span class="sxs-lookup"><span data-stu-id="9f381-183">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="9f381-184">Este patrón se ve en todo el SDK.</span><span class="sxs-lookup"><span data-stu-id="9f381-184">This pattern is seen throughout the SDK.</span></span>
<span data-ttu-id="9f381-185">Los métodos que realizan un trabajo similar normalmente tienen el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="9f381-185">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="9f381-186">La principal diferencia está en el valor devuelto por el método `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="9f381-186">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="9f381-187">Este valor es un objeto del tipo [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), que sigue el [patrón de diseño de futuros](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="9f381-187">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="9f381-188">Los futuros representan una operación de ejecución prolongada en Azure que puede sondear, cancelar o bloquear cuando finalice.</span><span class="sxs-lookup"><span data-stu-id="9f381-188">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="9f381-189">En este ejemplo, lo mejor es esperar a que finalice la operación.</span><span class="sxs-lookup"><span data-stu-id="9f381-189">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="9f381-190">Para esperar a un futuro se necesita tanto un [objeto de contexto](https://blog.golang.org/context) como el cliente que creó el objeto `Future`.</span><span class="sxs-lookup"><span data-stu-id="9f381-190">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="9f381-191">Hay dos orígenes de error posibles aquí: un error producido en el cliente cuando se intenta invocar el método y una respuesta de error desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="9f381-191">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="9f381-192">El último se devuelve como parte de la llamada a `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="9f381-192">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

### <a name="get-the-assigned-ip-address"></a><span data-ttu-id="9f381-193">Obtención de la dirección IP asignada</span><span class="sxs-lookup"><span data-stu-id="9f381-193">Get the assigned IP address</span></span>

<span data-ttu-id="9f381-194">Para cualquier operación con la máquina virtual recién creada, se necesita la dirección IP asignada.</span><span class="sxs-lookup"><span data-stu-id="9f381-194">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="9f381-195">Las direcciones IP son recursos de Azure independientes, enlazados a los recursos del controlador de interfaz de red (NIC).</span><span class="sxs-lookup"><span data-stu-id="9f381-195">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

```go
func getLogin() {
    params, err := readJSON(parametersFile)
    if err != nil {
        log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
    }

    addressClient := network.NewPublicIPAddressesClient(clientData.SubscriptionID)
    addressClient.Authorizer = authorizer
    ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
    ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
    if err != nil {
        log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
    }

    vmUser := (*params)["vm_user"].(map[string]interface{})

    log.Printf("Log in with ssh: %s@%s, password: %s",
        vmUser["value"].(string),
        *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
        clientData.VMPassword)
}
```

<span data-ttu-id="9f381-196">Este método se basa en la información que se almacena en el archivo de parámetros.</span><span class="sxs-lookup"><span data-stu-id="9f381-196">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="9f381-197">El código podría consultar directamente la máquina virtual para obtener la NIC, consultar la NIC para obtener el recurso de IP y, finalmente, consultar el recurso de IP directamente.</span><span class="sxs-lookup"><span data-stu-id="9f381-197">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="9f381-198">Es una cadena larga de dependencias y operaciones para resolver, lo que resulta costoso.</span><span class="sxs-lookup"><span data-stu-id="9f381-198">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="9f381-199">Puesto que la información de JSON es local, se puede cargar en su lugar.</span><span class="sxs-lookup"><span data-stu-id="9f381-199">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="9f381-200">El valor del usuario de la máquina virtual también se carga desde el código JSON.</span><span class="sxs-lookup"><span data-stu-id="9f381-200">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="9f381-201">La contraseña de la máquina virtual se cargó anteriormente desde el archivo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="9f381-201">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f381-202">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="9f381-202">Next steps</span></span>

<span data-ttu-id="9f381-203">En este inicio rápido, ha tomado una plantilla existente para su implementación mediante Go.</span><span class="sxs-lookup"><span data-stu-id="9f381-203">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="9f381-204">Después se ha conectado mediante SSH a la máquina virtual recién creada.</span><span class="sxs-lookup"><span data-stu-id="9f381-204">Then you connected to the newly created VM via SSH.</span></span>

<span data-ttu-id="9f381-205">Para seguir obteniendo información acerca de cómo trabajar con máquinas virtuales en el entorno de Azure con Go, puede consultar los [ejemplos de proceso de Azure para Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) o los [ejemplos de administración de recursos de Azure para Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="9f381-205">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="9f381-206">Para obtener más información acerca de los métodos de autenticación disponibles en el SDK y qué tipos de autenticación se admiten, consulte [Autenticación con el SDK de Azure para Go](azure-sdk-go-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="9f381-206">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
