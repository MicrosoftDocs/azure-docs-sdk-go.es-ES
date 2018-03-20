---
title: "Implementación de una máquina virtual de Azure desde Go"
description: "Implementación de una máquina virtual mediante el SDK de Azure para Go."
keywords: "azure, máquina virtual, vm, go, golang, sdk de azure"
author: sptramer
ms.author: sttramer
ms.date: 02/08/2018
ms.topic: quickstart
ms.devlang: go
manager: routlaw
ms.openlocfilehash: ae460dbf21b13c40f3d564274f8b790afe005aae
ms.sourcegitcommit: af3473779cd7c2978f290fbdc51ee15eb1130840
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a><span data-ttu-id="12b3e-104">Inicio rápido: implementación de una máquina virtual de Azure desde una plantilla con el SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="12b3e-104">Quickstart: Deploy an Azure virtual machine from a template with the Azure SDK for Go</span></span>

<span data-ttu-id="12b3e-105">Este inicio rápido se centra en la implementación de recursos desde una plantilla con el SDK de Azure para Go.</span><span class="sxs-lookup"><span data-stu-id="12b3e-105">This quickstart focuses on deploying resources from a template with the Azure SDK for Go.</span></span> <span data-ttu-id="12b3e-106">Las plantillas son instantáneas de todos los recursos dentro de un [grupo de recursos de Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="12b3e-106">Templates are snapshots of all of the resources contained within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span> <span data-ttu-id="12b3e-107">A lo largo de la guía, se irá familiarizado con la funcionalidad y las convenciones del SDK al tiempo que realiza una tarea útil.</span><span class="sxs-lookup"><span data-stu-id="12b3e-107">Along the way, you'll become familiar with the functionality and conventions of the SDK while performing a useful task.</span></span>

<span data-ttu-id="12b3e-108">Al final de este inicio rápido, tendrá una máquina virtual en ejecución en la que podrá iniciar sesión con un nombre de usuario y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="12b3e-108">At the end of this quickstart, you have a running VM that you log into with a username and password.</span></span>

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

<span data-ttu-id="12b3e-109">Si usa una instalación local de la CLI de Azure, este inicio rápido requiere la versión 2.0.24 de la CLI o posterior.</span><span class="sxs-lookup"><span data-stu-id="12b3e-109">If you use a local install of the Azure CLI, this quickstart requires CLI version 2.0.24 or later.</span></span> <span data-ttu-id="12b3e-110">Ejecute `az --version` para asegurarse de que la instalación de la CLI cumple este requisito.</span><span class="sxs-lookup"><span data-stu-id="12b3e-110">Run `az --version` to make sure your CLI install meets this requirement.</span></span> <span data-ttu-id="12b3e-111">Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="12b3e-111">If you need to install or upgrade, see [Install the Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span>

## <a name="install-the-azure-sdk-for-go"></a><span data-ttu-id="12b3e-112">Instalación del SDK de Azure para Go</span><span class="sxs-lookup"><span data-stu-id="12b3e-112">Install the Azure SDK for Go</span></span> 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a><span data-ttu-id="12b3e-113">Creación de una entidad de servicio</span><span class="sxs-lookup"><span data-stu-id="12b3e-113">Create a service principal</span></span>

<span data-ttu-id="12b3e-114">Para iniciar sesión de una manera no interactiva con una aplicación, necesita una entidad de servicio.</span><span class="sxs-lookup"><span data-stu-id="12b3e-114">To log in non-interactively with an application, you need a service principal.</span></span> <span data-ttu-id="12b3e-115">Las entidades de servicio son parte del control de acceso basado en rol (RBAC), que crea una identidad de usuario única.</span><span class="sxs-lookup"><span data-stu-id="12b3e-115">Service principals are part of role-based access control (RBAC), which creates a unique user identity.</span></span> <span data-ttu-id="12b3e-116">Para crear una nueva entidad de servicio con la CLI, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="12b3e-116">To create a new service principal with the CLI, run the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

<span data-ttu-id="12b3e-117">__Asegúrese de__  registrar los valores de `appId`, `password` y `tenant` de la salida.</span><span class="sxs-lookup"><span data-stu-id="12b3e-117">__Make sure__ to record the `appId`, `password`, and `tenant` values in the output.</span></span> <span data-ttu-id="12b3e-118">La aplicación utiliza estos valores para autenticarse con Azure.</span><span class="sxs-lookup"><span data-stu-id="12b3e-118">These values are used by the application to authenticate with Azure.</span></span>

<span data-ttu-id="12b3e-119">Para más información sobre la creación y administración de entidades de servicio con la CLI de Azure 2.0, consulte [Creación de una entidad de servicio de Azure con la CLI de Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="12b3e-119">For more information on creating and managing service principals with the Azure CLI 2.0, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="12b3e-120">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="12b3e-120">Get the code</span></span>

<span data-ttu-id="12b3e-121">Obtenga el código de inicio rápido y todas sus dependencias con `go get`.</span><span class="sxs-lookup"><span data-stu-id="12b3e-121">Get the quickstart code and all of its dependencies with `go get`.</span></span>

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

<span data-ttu-id="12b3e-122">Este código se compila, pero no se ejecuta correctamente hasta que le proporciona información acerca de la cuenta de Azure y la entidad de seguridad de servicio creada.</span><span class="sxs-lookup"><span data-stu-id="12b3e-122">This code compiles, but doesn't run correctly until you provide it information about your Azure account and the created service principal.</span></span> <span data-ttu-id="12b3e-123">En `main.go` hay una variable, `config`, que contiene una estructura `authInfo`.</span><span class="sxs-lookup"><span data-stu-id="12b3e-123">In `main.go` there is a variable, `config`, which contains an `authInfo` struct.</span></span> <span data-ttu-id="12b3e-124">Se deben reemplazar los valores de los campos de esta estructura para poder autenticarse correctamente.</span><span class="sxs-lookup"><span data-stu-id="12b3e-124">This struct needs to have its field values replaced in order to authenticate correctly.</span></span>

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* <span data-ttu-id="12b3e-125">`SubscriptionID`: el identificador de la suscripción, que se puede obtener con los comandos CLI</span><span class="sxs-lookup"><span data-stu-id="12b3e-125">`SubscriptionID`: Your subscription ID, which can be obtained from the CLI command</span></span>

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* <span data-ttu-id="12b3e-126">`TenantID`: el identificador del inquilino, el valor de `tenant` registrado al crear la entidad de servicio</span><span class="sxs-lookup"><span data-stu-id="12b3e-126">`TenantID`: Your tenant ID, the `tenant` value recorded when creating the service principal</span></span>
* <span data-ttu-id="12b3e-127">`ServicePrincipalID`: el valor de `appId` registrado al crear la entidad de servicio</span><span class="sxs-lookup"><span data-stu-id="12b3e-127">`ServicePrincipalID`: The `appId` value recorded when creating the service principal</span></span>
* <span data-ttu-id="12b3e-128">`ServicePrincipalSecret`: el valor de `password` registrado al crear la entidad de servicio</span><span class="sxs-lookup"><span data-stu-id="12b3e-128">`ServicePrincipalSecret`: The `password` value recorded when creating the service principal</span></span>

<span data-ttu-id="12b3e-129">También debe modificar un valor en el archivo `vm-quickstart-params.json`.</span><span class="sxs-lookup"><span data-stu-id="12b3e-129">You also need to edit a value in the `vm-quickstart-params.json` file.</span></span>

```json
    "vm_password": {
        "value": "_"
    }
```

* <span data-ttu-id="12b3e-130">`vm_password`: la contraseña de la cuenta del usuario de la máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="12b3e-130">`vm_password`: The password for the VM user account.</span></span> <span data-ttu-id="12b3e-131">Debe tener de 12 a 72 caracteres de longitud y contener 3 de los siguientes caracteres:</span><span class="sxs-lookup"><span data-stu-id="12b3e-131">It must be 12-72 characters in length and contain 3 of the following characters:</span></span>
  * <span data-ttu-id="12b3e-132">Una letra minúscula</span><span class="sxs-lookup"><span data-stu-id="12b3e-132">A lowercase letter</span></span>
  * <span data-ttu-id="12b3e-133">Una letra mayúscula</span><span class="sxs-lookup"><span data-stu-id="12b3e-133">An uppercase letter</span></span>
  * <span data-ttu-id="12b3e-134">Un número</span><span class="sxs-lookup"><span data-stu-id="12b3e-134">A number</span></span>
  * <span data-ttu-id="12b3e-135">Un símbolo</span><span class="sxs-lookup"><span data-stu-id="12b3e-135">A symbol</span></span>

## <a name="running-the-code"></a><span data-ttu-id="12b3e-136">Ejecución del código</span><span class="sxs-lookup"><span data-stu-id="12b3e-136">Running the code</span></span>

<span data-ttu-id="12b3e-137">Puede ejecutar el inicio rápido con el comando `go run`.</span><span class="sxs-lookup"><span data-stu-id="12b3e-137">Run the quickstart with the `go run` command.</span></span>

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

<span data-ttu-id="12b3e-138">Si se produce un error en la implementación, obtendrá un mensaje que indica que ha habido un problema, pero sin detalles específicos.</span><span class="sxs-lookup"><span data-stu-id="12b3e-138">If there is a failure in the deployment, you get a message indicating that there was an issue, but without any specific details.</span></span> <span data-ttu-id="12b3e-139">Mediante la CLI de Azure, puede obtener los detalles del error de implementación con el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="12b3e-139">Using the Azure CLI, get the details of the deployment failure with the following command:</span></span>

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

<span data-ttu-id="12b3e-140">Si la implementación se realiza correctamente, verá un mensaje que proporciona el nombre de usuario, la dirección IP y la contraseña para iniciar sesión en la máquina virtual recién creada.</span><span class="sxs-lookup"><span data-stu-id="12b3e-140">If the deployment is successful, you see a message giving the username, IP address, and password for logging into the newly created virtual machine.</span></span> <span data-ttu-id="12b3e-141">Conéctese mediante SSH en este equipo para confirmar que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="12b3e-141">SSH into this machine to confirm that it is up and running.</span></span>

## <a name="cleaning-up"></a><span data-ttu-id="12b3e-142">Limpiar</span><span class="sxs-lookup"><span data-stu-id="12b3e-142">Cleaning up</span></span>

<span data-ttu-id="12b3e-143">Para limpiar los recursos creados en este inicio rápido, puede eliminar el grupo de recursos con la CLI.</span><span class="sxs-lookup"><span data-stu-id="12b3e-143">Clean up the resources created during this quickstart by deleting the resource group with the CLI.</span></span>

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a><span data-ttu-id="12b3e-144">El código en profundidad</span><span class="sxs-lookup"><span data-stu-id="12b3e-144">Code in depth</span></span>

<span data-ttu-id="12b3e-145">El contenido del código de inicio rápido se desglosa en un bloque de variables y varias funciones pequeñas, cada uno de los cuales se trata aquí.</span><span class="sxs-lookup"><span data-stu-id="12b3e-145">What the quickstart code does is broken down into a block of variables and several small functions, each of which are discussed here.</span></span>

### <a name="variable-assignments-and-structs"></a><span data-ttu-id="12b3e-146">Asignaciones de variables y estructuras</span><span class="sxs-lookup"><span data-stu-id="12b3e-146">Variable assignments and structs</span></span>

<span data-ttu-id="12b3e-147">Puesto que el inicio rápido es independiente, usa variables globales en lugar de opciones de la línea de comandos o variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="12b3e-147">Since quickstart is self-contained, it uses global variables rather than command-line options or environment variables.</span></span>

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

<span data-ttu-id="12b3e-148">La estructura `authInfo` se declara para encapsular toda la información necesaria para la autorización con los servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="12b3e-148">The `authInfo` struct is declared to encapsulate all of the information needed for authorization with Azure services.</span></span>

```go
const (
    resourceGroupName     = "GoVMQuickstart"
    resourceGroupLocation = "eastus"

    deploymentName = "VMDeployQuickstart"
    templateFile   = "vm-quickstart-template.json"
    parametersFile = "vm-quickstart-params.json"
)

var (
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }

    ctx = context.Background()

    token *adal.ServicePrincipalToken
)
```

<span data-ttu-id="12b3e-149">Los valores declarados proporcionan los nombres de los recursos creados.</span><span class="sxs-lookup"><span data-stu-id="12b3e-149">Values are declared which give the names of created resources.</span></span> <span data-ttu-id="12b3e-150">La ubicación también se especifica aquí y puede cambiarla para ver cómo se comportan las implementaciones en otros centros de datos.</span><span class="sxs-lookup"><span data-stu-id="12b3e-150">The location is also specified here, which you can change to see how deployments behave in other datacenters.</span></span> <span data-ttu-id="12b3e-151">No todos los centros de datos tienen todos los recursos necesarios disponibles.</span><span class="sxs-lookup"><span data-stu-id="12b3e-151">Not every datacenter has all of the required resources available.</span></span>

<span data-ttu-id="12b3e-152">Las constantes `templateFile` y `parametersFile` apuntan a los archivos necesarios para la implementación.</span><span class="sxs-lookup"><span data-stu-id="12b3e-152">The `templateFile` and `parametersFile` constants point to the files needed for deployment.</span></span> <span data-ttu-id="12b3e-153">El token de la entidad de servicio se trata más adelante y la variable `ctx` es un [contexto de Go](https://blog.golang.org/context) para las operaciones de red.</span><span class="sxs-lookup"><span data-stu-id="12b3e-153">The service principal token is covered later, and the `ctx` variable is a [Go context](https://blog.golang.org/context) for the network operations.</span></span>

### <a name="init-and-authorization"></a><span data-ttu-id="12b3e-154">init() y la autorización</span><span class="sxs-lookup"><span data-stu-id="12b3e-154">init() and authorization</span></span>

<span data-ttu-id="12b3e-155">El método `init()` del código configura la autorización.</span><span class="sxs-lookup"><span data-stu-id="12b3e-155">The `init()` method for the code sets up authorization.</span></span> <span data-ttu-id="12b3e-156">Puesto que la autorización es una condición previa para todo en el inicio rápido, tiene sentido tenerlo como parte de la inicialización.</span><span class="sxs-lookup"><span data-stu-id="12b3e-156">Since authorization is a precondition for everything in the quickstart, it makes sense to have it as part of initialization.</span></span> 

```go
// Authenticate with the Azure services over OAuth, using a service principal.
func init() {
    oauthConfig, err := adal.NewOAuthConfig(azure.PublicCloud.ActiveDirectoryEndpoint, config.TenantID)
    if err != nil {
        log.Fatalf("Failed to get OAuth config: %v\n", err)
    }
    token, err = adal.NewServicePrincipalToken(
        *oauthConfig,
        config.ServicePrincipalID,
        config.ServicePrincipalSecret,
        azure.PublicCloud.ResourceManagerEndpoint)
    if err != nil {
        log.Fatalf("faled to get token: %v\n", err)
    }
}
```

<span data-ttu-id="12b3e-157">Este código realiza dos pasos para la autorización:</span><span class="sxs-lookup"><span data-stu-id="12b3e-157">This code completes two steps for authorization:</span></span>

* <span data-ttu-id="12b3e-158">La información de configuración de OAuth del `TenantID` se recupera mediante una interfaz con Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="12b3e-158">OAuth configuration information for the `TenantID` is retrieved by interfacing with Azure Active Directory.</span></span> <span data-ttu-id="12b3e-159">El objeto [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) contiene los puntos de conexión utilizados en la configuración de Azure estándar.</span><span class="sxs-lookup"><span data-stu-id="12b3e-159">The [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) object contains endpoints used in the standard Azure configuration.</span></span>
* <span data-ttu-id="12b3e-160">Se llama a la función [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken).</span><span class="sxs-lookup"><span data-stu-id="12b3e-160">The [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken) function is called.</span></span> <span data-ttu-id="12b3e-161">Esta función toma la información de OAuth junto con el inicio de sesión de la entidad de servicio, así como el estilo de administración de Azure que se está usando.</span><span class="sxs-lookup"><span data-stu-id="12b3e-161">This function takes the OAuth information along with the service principal login, as well as which style of Azure management is being used.</span></span> <span data-ttu-id="12b3e-162">A menos que tenga requisitos específicos y sepa lo que está haciendo, este valor debe ser siempre `.ResourceManagerEndpoint`.</span><span class="sxs-lookup"><span data-stu-id="12b3e-162">Unless you have specific requirements and know what you're doing, this value should always be `.ResourceManagerEndpoint`.</span></span>

### <a name="flow-of-operations-in-main"></a><span data-ttu-id="12b3e-163">Flujo de operaciones en main()</span><span class="sxs-lookup"><span data-stu-id="12b3e-163">Flow of operations in main()</span></span>

<span data-ttu-id="12b3e-164">La función `main()` es simple, solo indica el flujo de operaciones y realiza la comprobación de errores.</span><span class="sxs-lookup"><span data-stu-id="12b3e-164">The `main()` function is simple, only indicating the flow of operations and performing error-checking.</span></span>

```go
func main() {
    group, err := createGroup()
    if err != nil {
        log.Fatalf("failed to create group: %v", err)
    }
    log.Printf("created group: %v\n", *group.Name)

    log.Println("starting deployment")
    result, err := createDeployment()
    if err != nil {
        log.Fatalf("Failed to deploy correctly: %v", err)
    }
    log.Printf("Completed deployment: %v", *result.Name)
    getLogin()
}
```

<span data-ttu-id="12b3e-165">Los pasos que ejecuta el código son, en orden:</span><span class="sxs-lookup"><span data-stu-id="12b3e-165">The steps that the code runs through are, in order:</span></span>

* <span data-ttu-id="12b3e-166">Crear el grupo de recursos para la implementación (`createGroup()`)</span><span class="sxs-lookup"><span data-stu-id="12b3e-166">Create the resource group to deploy to (`createGroup()`)</span></span>
* <span data-ttu-id="12b3e-167">Crear la implementación dentro del grupo (`createDeployment()`)</span><span class="sxs-lookup"><span data-stu-id="12b3e-167">Create the deployment within this group (`createDeployment()`)</span></span>
* <span data-ttu-id="12b3e-168">Obtener y mostrar la información de inicio de sesión de la máquina virtual implementada (`getLogin()`)</span><span class="sxs-lookup"><span data-stu-id="12b3e-168">Obtain and display login information for the deployed VM (`getLogin()`)</span></span>

### <a name="creating-the-resource-group"></a><span data-ttu-id="12b3e-169">Creación del grupo de recursos</span><span class="sxs-lookup"><span data-stu-id="12b3e-169">Creating the resource group</span></span>

<span data-ttu-id="12b3e-170">La función `createGroup()` crea el grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="12b3e-170">The `createGroup()` function creates the resource group.</span></span> <span data-ttu-id="12b3e-171">Un examen del flujo de llamadas y los argumentos muestra la forma en la que se estructuran las interacciones del servicio en el SDK.</span><span class="sxs-lookup"><span data-stu-id="12b3e-171">Looking at the call flow and arguments demonstrates the way that service interactions are structured in the SDK.</span></span>

```go
func createGroup() (group resources.Group, err error) {
        groupsClient := resources.NewGroupsClient(config.SubscriptionID)
        groupsClient.Authorizer = autorest.NewBearerAuthorizer(token)

        return groupsClient.CreateOrUpdate(
                ctx,
                resourceGroupName,
                resources.Group{
                        Location: to.StringPtr(resourceGroupLocation)})
}
```

<span data-ttu-id="12b3e-172">El flujo general de la interacción con un servicio de Azure es:</span><span class="sxs-lookup"><span data-stu-id="12b3e-172">The general flow of interacting with an Azure service is:</span></span>

* <span data-ttu-id="12b3e-173">Crear el cliente mediante el método `service.NewXClient()`, donde `X` es el tipo de recurso de `service` con el que desea interactuar.</span><span class="sxs-lookup"><span data-stu-id="12b3e-173">Create the client using the `service.NewXClient()` method, where `X` is the resource type of the `service` that you want to interact with.</span></span> <span data-ttu-id="12b3e-174">Esta función siempre tiene un identificador de suscripción.</span><span class="sxs-lookup"><span data-stu-id="12b3e-174">This function always takes a subscription ID.</span></span>
* <span data-ttu-id="12b3e-175">Establecer el método de autorización para el cliente, lo que le permite interactuar con la API remota.</span><span class="sxs-lookup"><span data-stu-id="12b3e-175">Set the authorization method for the client, allowing it to interact with the remote API.</span></span>
* <span data-ttu-id="12b3e-176">Realizar la llamada al método en el cliente correspondiente a la API remota.</span><span class="sxs-lookup"><span data-stu-id="12b3e-176">Make the method call on the client corresponding to the remote API.</span></span> <span data-ttu-id="12b3e-177">Los métodos del cliente del servicio normalmente toman el nombre del recurso y un objeto de metadatos.</span><span class="sxs-lookup"><span data-stu-id="12b3e-177">Service client methods usually take the name of the resource and a metadata object.</span></span>

<span data-ttu-id="12b3e-178">La función [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se utiliza aquí para realizar una conversión de tipo.</span><span class="sxs-lookup"><span data-stu-id="12b3e-178">The [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) function is used to perform a type conversion here.</span></span> <span data-ttu-id="12b3e-179">Las estructuras de parámetros de los métodos del SDK toman casi exclusivamente punteros, por lo que estos métodos se proporcionan para facilitar las conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="12b3e-179">The parameters structs for methods of the SDK almost exclusively take pointers, so these methods are provided to make the type conversions easy.</span></span> <span data-ttu-id="12b3e-180">Consulte la documentación del módulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) para obtener la lista completa y el comportamiento de los convertidores de comodidad.</span><span class="sxs-lookup"><span data-stu-id="12b3e-180">See the documentation for the [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) module for the complete list and behavior of convenience converters.</span></span>

<span data-ttu-id="12b3e-181">La operación `groupsClient.CreateOrUpdate()` devuelve un puntero a una estructura de datos que representa al grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="12b3e-181">The `groupsClient.CreateOrUpdate()` operation returns a pointer to a data struct representing the resource group.</span></span> <span data-ttu-id="12b3e-182">Un valor de retorno directo de este tipo indica una operación de ejecución breve que está diseñada para ser sincrónica.</span><span class="sxs-lookup"><span data-stu-id="12b3e-182">A direct return value of this kind indicates a short-running operation that is meant to be synchronous.</span></span> <span data-ttu-id="12b3e-183">En la siguiente sección, verá un ejemplo de una operación de ejecución prolongada y cómo interactuar con ella.</span><span class="sxs-lookup"><span data-stu-id="12b3e-183">In the next section, you'll see an example of a long-running operation and how to interact with them.</span></span>

### <a name="performing-the-deployment"></a><span data-ttu-id="12b3e-184">Realización de la implementación</span><span class="sxs-lookup"><span data-stu-id="12b3e-184">Performing the deployment</span></span>

<span data-ttu-id="12b3e-185">Una vez creado el grupo que contiene los recursos, es el momento de ejecutar la implementación.</span><span class="sxs-lookup"><span data-stu-id="12b3e-185">Once the group to contain its resources is created, it's time to run the deployment.</span></span> <span data-ttu-id="12b3e-186">Este código se divide en secciones más pequeñas para hacer énfasis en las diferentes partes de su lógica.</span><span class="sxs-lookup"><span data-stu-id="12b3e-186">This code is broken up into smaller sections to emphasize different parts of its logic.</span></span>


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

        // ...
```

<span data-ttu-id="12b3e-187">`readJSON` carga los archivos de la implementación, los detalles de esta operación se omiten aquí.</span><span class="sxs-lookup"><span data-stu-id="12b3e-187">The deployment files are loaded by `readJSON`, the details of which are skipped here.</span></span> <span data-ttu-id="12b3e-188">Esta función devuelve un valor del tipo `*map[string]interface{}`, el tipo utilizado para construir los metadatos de la llamada de la implementación de los recursos.</span><span class="sxs-lookup"><span data-stu-id="12b3e-188">This function returns a `*map[string]interface{}`, the type used in constructing the metadata for the resource deployment call.</span></span>

```go
        // ...
        
        deploymentsClient := resources.NewDeploymentsClient(config.SubscriptionID)
        deploymentsClient.Authorizer = autorest.NewBearerAuthorizer(token)

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
                log.Fatalf("Failed to create deployment: %v", err)
        }
        //...
```

<span data-ttu-id="12b3e-189">Este código sigue el mismo patrón que la creación del grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="12b3e-189">This code follows the same pattern as with creating the resource group.</span></span> <span data-ttu-id="12b3e-190">Se crea un nuevo cliente, se le habilita para autenticarse con Azure y, a continuación, se llama a un método.</span><span class="sxs-lookup"><span data-stu-id="12b3e-190">A new client is created, given the ability to authenticate with Azure, and then a method is called.</span></span> <span data-ttu-id="12b3e-191">El método incluso tiene el mismo nombre (`CreateOrUpdate`) que el método correspondiente en los grupos de recursos.</span><span class="sxs-lookup"><span data-stu-id="12b3e-191">The method even has the same name (`CreateOrUpdate`) as the corresponding method for resource groups.</span></span> <span data-ttu-id="12b3e-192">Este patrón se ve una y otra vez en el SDK.</span><span class="sxs-lookup"><span data-stu-id="12b3e-192">This pattern is seen again and again in the SDK.</span></span> <span data-ttu-id="12b3e-193">Los métodos que realizan un trabajo similar normalmente tienen el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="12b3e-193">Methods that perform similar work normally have the same name.</span></span>

<span data-ttu-id="12b3e-194">La principal diferencia está en el valor devuelto por el método `deploymentsClient.CreateOrUpdate()`.</span><span class="sxs-lookup"><span data-stu-id="12b3e-194">The biggest difference comes in the return value of the `deploymentsClient.CreateOrUpdate()` method.</span></span> <span data-ttu-id="12b3e-195">Este valor es un objeto `Future`, que sigue el [patrón de diseño futuros y promesas](https://en.wikipedia.org/wiki/Futures_and_promises).</span><span class="sxs-lookup"><span data-stu-id="12b3e-195">This value is a `Future` object, which follows the [future design pattern](https://en.wikipedia.org/wiki/Futures_and_promises).</span></span> <span data-ttu-id="12b3e-196">Los futuros representan una operación de larga duración en Azure que desea sondear ocasionalmente mientras realiza otro trabajo.</span><span class="sxs-lookup"><span data-stu-id="12b3e-196">Futures represent a long-running operation in Azure that you may want to occasionally poll while performing other work.</span></span>

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

<span data-ttu-id="12b3e-197">En este ejemplo, lo mejor es esperar a que finalice la operación.</span><span class="sxs-lookup"><span data-stu-id="12b3e-197">For this example, the best thing to do is to wait for the operation to complete.</span></span> <span data-ttu-id="12b3e-198">Esperar a un futuro requiere tanto un [objeto de contexto](https://blog.golang.org/context) como el cliente que creó el objeto futuro.</span><span class="sxs-lookup"><span data-stu-id="12b3e-198">Waiting on a future requires both a [context object](https://blog.golang.org/context) and the client that created the Future object.</span></span> <span data-ttu-id="12b3e-199">Hay dos orígenes de error posibles aquí: un error producido en el cliente cuando se intenta invocar el método y una respuesta de error desde el servidor.</span><span class="sxs-lookup"><span data-stu-id="12b3e-199">There are two possible error sources here: An error caused on the client side when trying to invoke the method, and an error response from the server.</span></span> <span data-ttu-id="12b3e-200">El último se devuelve como parte de la llamada a `deploymentFuture.Result()`.</span><span class="sxs-lookup"><span data-stu-id="12b3e-200">The latter is returned as part of the `deploymentFuture.Result()` call.</span></span>

### <a name="obtaining-the-assigned-ip-address"></a><span data-ttu-id="12b3e-201">Obtención de la dirección IP asignada</span><span class="sxs-lookup"><span data-stu-id="12b3e-201">Obtaining the assigned IP address</span></span>

<span data-ttu-id="12b3e-202">Para cualquier operación con la máquina virtual recién creada, se necesita la dirección IP asignada.</span><span class="sxs-lookup"><span data-stu-id="12b3e-202">To do anything with the newly created VM, you need the assigned IP address.</span></span> <span data-ttu-id="12b3e-203">Las direcciones IP son recursos de Azure independientes, enlazados a los recursos del controlador de interfaz de red (NIC).</span><span class="sxs-lookup"><span data-stu-id="12b3e-203">IP addresses are their own separate Azure resource, bound to Network Interface Controller (NIC) resources.</span></span>

```go
func getLogin() {
        params, err := readJSON(parametersFile)
        if err != nil {
                log.Fatalf("Unable to read parameters. Get login information with `az network public-ip list -g %s", resourceGroupName)
        }

        addressClient := network.NewPublicIPAddressesClient(config.SubscriptionID)
        addressClient.Authorizer = autorest.NewBearerAuthorizer(token)
        ipName := (*params)["publicIPAddresses_QuickstartVM_ip_name"].(map[string]interface{})
        ipAddress, err := addressClient.Get(ctx, resourceGroupName, ipName["value"].(string), "")
        if err != nil {
                log.Fatalf("Unable to get IP information. Try using `az network public-ip list -g %s", resourceGroupName)
        }

        vmUser := (*params)["vm_user"].(map[string]interface{})
        vmPass := (*params)["vm_password"].(map[string]interface{})

        log.Printf("Log in with ssh: %s@%s, password: %s",
                vmUser["value"].(string),
                *ipAddress.PublicIPAddressPropertiesFormat.IPAddress,
                vmPass["value"].(string))
}
```

<span data-ttu-id="12b3e-204">Este método se basa en la información que se almacena en el archivo de parámetros.</span><span class="sxs-lookup"><span data-stu-id="12b3e-204">This method relies on information that is stored in the parameters file.</span></span> <span data-ttu-id="12b3e-205">El código podría consultar directamente la máquina virtual para obtener la NIC, consultar la NIC para obtener el recurso de IP y, finalmente, consultar el recurso de IP directamente.</span><span class="sxs-lookup"><span data-stu-id="12b3e-205">The code could query the VM directly to get its NIC, query the NIC to get its IP resource, and then query the IP resource directly.</span></span> <span data-ttu-id="12b3e-206">Es una cadena larga de dependencias y operaciones para resolver, lo que resulta costoso.</span><span class="sxs-lookup"><span data-stu-id="12b3e-206">That's a long chain of dependencies and operations to resolve, making it expensive.</span></span> <span data-ttu-id="12b3e-207">Puesto que la información de JSON es local, se puede cargar en su lugar.</span><span class="sxs-lookup"><span data-stu-id="12b3e-207">Since the JSON information is local, it can be loaded instead.</span></span>

<span data-ttu-id="12b3e-208">Del mismo modo, los valores para el usuario y la contraseña de la máquina virtual se cargan desde el archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="12b3e-208">The values for the VM user and password are likewise loaded from the JSON.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12b3e-209">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="12b3e-209">Next steps</span></span>

<span data-ttu-id="12b3e-210">En este inicio rápido, ha tomado una plantilla existente para su implementación mediante Go.</span><span class="sxs-lookup"><span data-stu-id="12b3e-210">In this quickstart, you took an existing template and deployed it through Go.</span></span> <span data-ttu-id="12b3e-211">A continuación, se ha conectado a la máquina virtual recién creada a través de SSH para asegurarse de que está en ejecución.</span><span class="sxs-lookup"><span data-stu-id="12b3e-211">Then you connected to the newly created VM via SSH to ensure that it's running.</span></span>

<span data-ttu-id="12b3e-212">Para seguir obteniendo información acerca de cómo trabajar con máquinas virtuales en el entorno de Azure con Go, puede consultar los [ejemplos de proceso de Azure para Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) o los [ejemplos de administración de recursos de Azure para Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span><span class="sxs-lookup"><span data-stu-id="12b3e-212">To continue learning about working with virtual machines in the Azure environment with Go, take a look at the [Azure compute samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) or [Azure resource management samples for Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).</span></span>
