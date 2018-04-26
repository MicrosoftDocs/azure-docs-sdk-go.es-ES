---
title: Implementación de una máquina virtual de Azure desde Go
description: Implementación de una máquina virtual mediante el SDK de Azure para Go.
author: sptramer
ms.author: sttramer
ms.date: 04/03/2018
ms.topic: quickstart
ms.devlang: go
manager: carmonm
ms.openlocfilehash: 565580e9e6c6ced543bd00bbaa01383834d9a41c
ms.sourcegitcommit: 2b2884ea7673c95ba45b3d6eec647200e75bfc5b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/19/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="1d2ab-103">Inicio rápido: implementación de una máquina virtual de Azure desde una plantilla con el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="1d2ab-103">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="1d2ab-104">Este inicio rápido se centra en la implementación de recursos desde una plantilla con el SDK de Azure para Go.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-104">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="1d2ab-105">Las plantillas son instantáneas de todos los recursos dentro de un [grupo de recursos de Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-105">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="1d2ab-106">A lo largo de la guía, se irá familiarizado con la funcionalidad y las convenciones del SDK al tiempo que realiza una tarea útil.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-106">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="1d2ab-107">Al final de este inicio rápido, tendrá una máquina virtual en ejecución en la que podrá iniciar sesión con un nombre de usuario y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-107">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="1d2ab-108">Si usa una instalación local de la CLI de Azure, esta plantilla de inicio rápido requiere la versión __2.0.28__ de la CLI o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-108">If you use a local install of the Azure CLI, this quickstart requires CLI version __2.0.28__ or later.</span></span> <span data-ttu-id="1d2ab-109">Ejecute `az --version` para asegurarse de que la instalación de la CLI cumple este requisito.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-109">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="1d2ab-110">Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-110">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="1d2ab-111">Instalación del SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="1d2ab-111">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="1d2ab-112">Creación de una entidad de servicio</span><span class="sxs-lookup"><span data-stu-id="1d2ab-112">Create a service principal</span></span>


<span data-ttu-id="1d2ab-113">Para iniciar sesión de una manera no interactiva con una aplicación, necesita una entidad de servicio.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-113">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="1d2ab-114">Las entidades de servicio son parte del control de acceso basado en rol (RBAC), que crea una identidad de usuario única.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-114">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="1d2ab-115">Para crear una nueva entidad de servicio con la CLI, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-115">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

<span data-ttu-id="1d2ab-116">Establezca la variable de entorno `AZURE_AUTH_LOCATION` para que sea la ruta de acceso completa de este archivo.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-116">Set the environment variable `AZURE_AUTH_LOCATION` to be the full path to this file.</span></span> <span data-ttu-id="1d2ab-117">Después, el SDK busca y lee las credenciales directamente desde este archivo, sin que tenga que realizar cambios ni registrar información de la entidad de servicio.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-117">Then the SDK locates and reads the credentials directly from this file, without you having to make any changes or record information from the service principal.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="1d2ab-118">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="1d2ab-118">Get the code</span></span>

<span data-ttu-id="1d2ab-119">Obtenga el código de inicio rápido y todas sus dependencias con `go get`.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-119">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

<span data-ttu-id="1d2ab-120">No es necesario realizar ninguna modificación en el código fuente si la variable `AZURE_AUTH_LOCATION` está establecida correctamente.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-120">You don't need to make any source code modifications if the `AZURE_AUTH_LOCATION` variable is properly set.</span></span> <span data-ttu-id="1d2ab-121">Cuando se ejecuta el programa, carga toda la información de autenticación necesaria desde allí.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-121">When the program runs, it loads all the necessary authentication information from there.</span></span>

## <a name="running-the-code"></a><span data-ttu-id="1d2ab-122">Ejecución del código</span><span class="sxs-lookup"><span data-stu-id="1d2ab-122">Running the code</span></span>

<span data-ttu-id="1d2ab-123">Puede ejecutar el inicio rápido con el comando `go run`.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-123">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

<span data-ttu-id="1d2ab-124">Si se produce un error en la implementación, obtendrá un mensaje que indica que ha habido un problema, pero sin detalles específicos.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-124">If there is a failure in the deployment, you get a message indicating that there was an issue, but it may not include enough detail.</span></span> <span data-ttu-id="1d2ab-125">Mediante la CLI de Azure, puede obtener información completa del error de implementación con el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-125">Using the Azure CLI, get the full details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="1d2ab-126">Si la implementación se realiza correctamente, verá un mensaje que proporciona el nombre de usuario, la dirección IP y la contraseña para iniciar sesión en la máquina virtual recién creada.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-126">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="1d2ab-127">Conéctese mediante SSH en este equipo para confirmar que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-127">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="1d2ab-128">Limpiar</span><span class="sxs-lookup"><span data-stu-id="1d2ab-128">Cleaning up</span></span>

<span data-ttu-id="1d2ab-129">Para limpiar los recursos creados en este inicio rápido, puede eliminar el grupo de recursos con la CLI.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-129">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="1d2ab-130">El código en profundidad</span><span class="sxs-lookup"><span data-stu-id="1d2ab-130">Code in depth</span></span>

<span data-ttu-id="1d2ab-131">El contenido del código de inicio rápido se desglosa en un bloque de variables y varias funciones pequeñas, cada uno de los cuales se trata aquí.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-131">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variables-constants-and-types"></a><span data-ttu-id="1d2ab-132">Variables, constantes y tipos</span><span class="sxs-lookup"><span data-stu-id="1d2ab-132">Variables, constants, and types</span></span>

<span data-ttu-id="1d2ab-133">Puesto que la plantilla de inicio rápido es independiente, usa variables y constantes globales.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-133">Since quickstart is self-contained, it uses global constants and variables.</span></span>

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

<span data-ttu-id="1d2ab-134">Los valores declarados proporcionan los nombres de los recursos creados.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-134">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="1d2ab-135">La ubicación también se especifica aquí y puede cambiarla para ver cómo se comportan las implementaciones en otros centros de datos.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-135">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="1d2ab-136">No todos los centros de datos tienen todos los recursos necesarios disponibles.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-136">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="1d2ab-137">El tipo `clientInfo` se declara para encapsular toda la información que debe cargarse de forma independiente desde el archivo de autenticación para configurar los clientes en el SDK y establecer la contraseña de la máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-137">The `clientInfo` type is declared to encapsulate all of the information that must be independently loaded from the authentication file to set up clients in the SDK and set the VM password.</span></span>

<span data-ttu-id="1d2ab-138">Las constantes `templateFile` y `parametersFile` apuntan a los archivos necesarios para la implementación.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-138">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="1d2ab-139">El SDK para Go configurará `authorizer` para autenticación, y la variable `ctx` es un [contexto de Go](https://blog.golang.org/context) para las operaciones de red.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-139">The `authorizer` will be configured by the Go SDK for authentication, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="authentication-and-initialization"></a><span data-ttu-id="1d2ab-140">Autenticación e inicialización</span><span class="sxs-lookup"><span data-stu-id="1d2ab-140">Authentication and initialization</span></span>

<span data-ttu-id="1d2ab-141">La función `init` establece la autenticación.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-141">The `init` function sets up authentication.</span></span> <span data-ttu-id="1d2ab-142">Puesto que la autenticación es una condición previa para todo en la plantilla de inicio rápido, tiene sentido que forme parte de la inicialización.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-142">Since authentication is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> <span data-ttu-id="1d2ab-143">También carga alguna información necesaria desde el archivo de autenticación para configurar los clientes y la máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-143">It also loads some information needed from the authentication file to configure clients and the VM.</span></span>

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

<span data-ttu-id="1d2ab-144">En primer lugar, se invoca a [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) para cargar la información de autenticación del archivo se encuentra en `AZURE_AUTH_LOCATION`.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-144">First, [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) is called to load the authentication information from the file located at `AZURE_AUTH_LOCATION`.</span></span> <span data-ttu-id="1d2ab-145">Después, la función `readJSON` carga manualmente este archivo (se omite aquí) para extraer los dos valores necesarios para ejecutar el resto del programa: el identificador de la suscripción del cliente y secreto de la entidad de servicio, que también se utiliza para la contraseña de la máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-145">Next, this file is loaded manually by the `readJSON` function (omitted here) to pull the two values needed to run the rest of the program: The subscription ID of the client, and the service principal's secret, which is also used for the VM's password.</span></span>

> [!WARNING]
> <span data-ttu-id="1d2ab-146">Para simplificar el inicio rápido, se reutiliza la contraseña de la entidad de servicio.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-146">To keep the quickstart simple, the service principal password is reused.</span></span> <span data-ttu-id="1d2ab-147">En producción, no olvide que __nunca__ se reutiliza una contraseña que da acceso a los recursos de Azure.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-147">In production, take care to __never__ reuse a password which gives access to your Azure resources.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="1d2ab-148">Flujo de operaciones en main()</span><span class="sxs-lookup"><span data-stu-id="1d2ab-148">Flow of operations in main()</span></span>

<span data-ttu-id="1d2ab-149">La función `main` es simple, solo indica el flujo de operaciones y realiza la comprobación de errores.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-149">The `main` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

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

<span data-ttu-id="1d2ab-150">Los pasos que ejecuta el código son, en orden:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-150">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="1d2ab-151">Crear el grupo de recursos para la implementación (`createGroup`)</span><span class="sxs-lookup"><span data-stu-id="1d2ab-151">Create the resource group to deploy to (`createGroup`)</span></span>
* <span data-ttu-id="1d2ab-152">Crear la implementación dentro del grupo (`createDeployment`)</span><span class="sxs-lookup"><span data-stu-id="1d2ab-152">Create the deployment within this group (`createDeployment`)</span></span>
* <span data-ttu-id="1d2ab-153">Obtener y mostrar la información de inicio de sesión de la máquina virtual implementada (`getLogin`)</span><span class="sxs-lookup"><span data-stu-id="1d2ab-153">Obtain and display login information for the deployed VM (`getLogin`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="1d2ab-154">Creación del grupo de recursos</span><span class="sxs-lookup"><span data-stu-id="1d2ab-154">Creating the resource group</span></span>

<span data-ttu-id="1d2ab-155">La función `createGroup` crea el grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-155">The `createGroup` function creates the resource group.</span></span> <span data-ttu-id="1d2ab-156">Un examen del flujo de llamadas y los argumentos muestra la forma en la que se estructuran las interacciones del servicio en el SDK.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-156">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

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

<span data-ttu-id="1d2ab-157">El flujo general de la interacción con un servicio de Azure es:</span><span class="sxs-lookup"><span data-stu-id="1d2ab-157">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="1d2ab-158">Crear el cliente mediante el método `service.New*Client()`, donde `*` es el tipo de recurso de `service` con el que desea interactuar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-158">Create the client using the `service.New*Client()` method, where `*` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="1d2ab-159">Esta función siempre tiene un identificador de suscripción.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-159">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="1d2ab-160">Establecer el método de autorización para el cliente, lo que le permite interactuar con la API remota.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-160">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="1d2ab-161">Realizar la llamada al método en el cliente correspondiente a la API remota.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-161">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="1d2ab-162">Los métodos del cliente del servicio normalmente toman el nombre del recurso y un objeto de metadatos.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-162">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="1d2ab-163">La función [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se utiliza aquí para realizar una conversión de tipo.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-163">The [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="1d2ab-164">Los parámetros de los métodos del SDK toman casi exclusivamente punteros, por lo que estos métodos se proporcionan para facilitar las conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-164">The parameters for SDK methods almost exclusively take pointers, so convenience methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="1d2ab-165">Consulte la documentación del módulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) para obtener la lista completa y el comportamiento de los convertidores.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-165">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list of convenience converters and their behavior.</span></span>

<span data-ttu-id="1d2ab-166">El método `groupsClient.CreateOrUpdate` devuelve un puntero a un tipo de datos que representa al grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-166">The `groupsClient.CreateOrUpdate` method returns a pointer to a data type representing the resource group.</span></span> <span data-ttu-id="1d2ab-167">Un valor de retorno directo de este tipo indica una operación de ejecución breve que está diseñada para ser sincrónica.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-167">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="1d2ab-168">En la siguiente sección, verá un ejemplo de una operación de ejecución prolongada y cómo interactuar con ella.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-168">In the next section, you'll see an example of a long-running operation and how to interact with it.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="1d2ab-169">Realización de la implementación</span><span class="sxs-lookup"><span data-stu-id="1d2ab-169">Performing the deployment</span></span>

<span data-ttu-id="1d2ab-170">Una vez creado el grupo de recursos, es el momento de ejecutar la implementación.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-170">Once the resource group is created, it's time to run the deployment.</span></span> <span data-ttu-id="1d2ab-171">Este código se divide en secciones más pequeñas para hacer énfasis en las diferentes partes de su lógica.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-171">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>

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

<span data-ttu-id="1d2ab-172">`readJSON` carga los archivos de la implementación, los detalles de esta operación se omiten aquí.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-172">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="1d2ab-173">Esta función devuelve un valor del tipo `*map[string]interface{}`, el tipo utilizado para construir los metadatos de la llamada de la implementación de los recursos.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-173">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span> <span data-ttu-id="1d2ab-174">La contraseña de la máquina virtual también se establece manualmente en los parámetros de implementación.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-174">The VM's password is also set manually on the deployment parameters.</span></span>

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

<span data-ttu-id="1d2ab-175">Este código sigue el mismo patrón que la creación del grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-175">This code follows the same pattern as creating the resource group.</span></span> <span data-ttu-id="1d2ab-176">Se crea un nuevo cliente, se le habilita para autenticarse con Azure y, a continuación, se llama a un método.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-176">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="1d2ab-177">El método incluso tiene el mismo nombre (`CreateOrUpdate`) que el método correspondiente en los grupos de recursos.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-177">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="1d2ab-178">Este patrón se ve en todo el SDK.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-178">This pattern is seen throughout the SDK.</span></span> <span data-ttu-id="1d2ab-179">Los métodos que realizan un trabajo similar normalmente tienen el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-179">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="1d2ab-180">La principal diferencia está en el valor devuelto por el método `deploymentsClient.CreateOrUpdate`.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-180">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate` method.</span></span> <span data-ttu-id="1d2ab-181">Este valor es un objeto del tipo [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), que sigue el [patrón de diseño de futuros](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-181">This value is of the [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future) type, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="1d2ab-182">Los futuros representan una operación de ejecución prolongada en Azure que puede sondear, cancelar o bloquear cuando finalice.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-182">Futures represent a long-running operation in Azure that you can poll, cancel, or block on their completion.</span></span>

```go
        //...
    err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
    if err != nil {
        return
    }
    deployment, err = deploymentFuture.Result(deploymentsClient)

    // Work around possible bugs or late-stage failures
    if deployment.Name == nil || err != nil {
        deployment, _ = deploymentsClient.Get(ctx, resourceGroupName, deploymentName)
    }
    return
```

<span data-ttu-id="1d2ab-183">En este ejemplo, lo mejor es esperar a que finalice la operación.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-183">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="1d2ab-184">Para esperar a un futuro se necesita tanto un [objeto de contexto](https://blog.golang.org/context) como el cliente que creó el objeto `Future`.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-184">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the `Future`.</span></span> <span data-ttu-id="1d2ab-185">Hay dos orígenes de error posibles aquí: un error producido en el cliente cuando se intenta invocar el método y una respuesta de error desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-185">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="1d2ab-186">El último se devuelve como parte de la llamada a `deploymentFuture.Result`.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-186">The latter is returned as part of the `deploymentFuture.Result` call.</span></span>

<span data-ttu-id="1d2ab-187">Una vez recuperada la información de implementación, si está vacía, hay una solución alternativa que consiste en realizar una llamada manual a `deploymentsClient.Get` para asegurarse de que contiene datos.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-187">Once the deployment information is retrieved, there is a workaround for possible bugs where the deployment information may be empty with a manual call to `deploymentsClient.Get` to ensure that the data is populated.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="1d2ab-188">Obtención de la dirección IP asignada</span><span class="sxs-lookup"><span data-stu-id="1d2ab-188">Obtaining the assigned IP address</span></span>

<span data-ttu-id="1d2ab-189">Para cualquier operación con la máquina virtual recién creada, se necesita la dirección IP asignada.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-189">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="1d2ab-190">Las direcciones IP son recursos de Azure independientes, enlazados a los recursos del controlador de interfaz de red (NIC).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-190">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

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

<span data-ttu-id="1d2ab-191">Este método se basa en la información que se almacena en el archivo de parámetros.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-191">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="1d2ab-192">El código podría consultar directamente la máquina virtual para obtener la NIC, consultar la NIC para obtener el recurso de IP y, finalmente, consultar el recurso de IP directamente.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-192">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="1d2ab-193">Es una cadena larga de dependencias y operaciones para resolver, lo que resulta costoso.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-193">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="1d2ab-194">Puesto que la información de JSON es local, se puede cargar en su lugar.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-194">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="1d2ab-195">El valor del usuario de la máquina virtual también se carga desde el código JSON.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-195">The value for the VM user is also loaded from the JSON.</span></span> <span data-ttu-id="1d2ab-196">La contraseña de la máquina virtual se cargó anteriormente desde el archivo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-196">The VM password was loaded earlier from the authentication file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d2ab-197">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1d2ab-197">Next steps</span></span>

<span data-ttu-id="1d2ab-198">En este inicio rápido, ha tomado una plantilla existente para su implementación mediante Go.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-198">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="1d2ab-199">A continuación, se ha conectado a la máquina virtual recién creada a través de SSH para asegurarse de que está en ejecución.</span><span class="sxs-lookup"><span data-stu-id="1d2ab-199">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="1d2ab-200">Para seguir obteniendo información acerca de cómo trabajar con máquinas virtuales en el entorno de Azure con Go, puede consultar los [ejemplos de proceso de Azure para Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) o los [ejemplos de administración de recursos de Azure para Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-200">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>

<span data-ttu-id="1d2ab-201">Para obtener más información acerca de los métodos de autenticación disponibles en el SDK y qué tipos de autenticación se admiten, consulte [Autenticación con el SDK de Azure para Go](azure-sdk-go-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="1d2ab-201">To learn more about the available authentication methods in the SDK, and which authentication types they support, see [Authentication with the Azure SDK for Go](azure-sdk-go-authorization.md).</span></span>
