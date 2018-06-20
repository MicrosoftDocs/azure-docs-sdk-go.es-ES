---
title: Implementación de una máquina virtual de Azure desde Go
description: Implementación de una máquina virtual mediante el SDK de Azure para Go.
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 04/03/2018
ms.topic: quickstart
ms.prod: azure
ms.technology: azure-sdk-go
ms.service: virtual-machines
ms.devlang: go
ms.openlocfilehash: 1fbcc54df2a2aebce56c5a5800361f3d3aed1ccc
ms.sourcegitcommit: f08abf902b48f8173aa6e261084ff2cfc9043305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "32319941"
---
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Inicio rápido: implementación de una máquina virtual de Azure desde una plantilla con el SDK de Azure para Go

Este inicio rápido se centra en la implementación de recursos desde una plantilla con el SDK de Azure para Go. Las plantillas son instantáneas de todos los recursos dentro de un [grupo de recursos de Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). A lo largo de la guía, se irá familiarizado con la funcionalidad y las convenciones del SDK al tiempo que realiza una tarea útil.

Al final de este inicio rápido, tendrá una máquina virtual en ejecución en la que podrá iniciar sesión con un nombre de usuario y una contraseña.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Si usa una instalación local de la CLI de Azure, esta plantilla de inicio rápido requiere la versión __2.0.28__ de la CLI o versiones posteriores. Ejecute `az --version` para asegurarse de que la instalación de la CLI cumple este requisito. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Instalación del SDK de Azure para Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio


Para iniciar sesión de una manera no interactiva con una aplicación, necesita una entidad de servicio. Las entidades de servicio son parte del control de acceso basado en rol (RBAC), que crea una identidad de usuario única. Para crear una nueva entidad de servicio con la CLI, ejecute el comando siguiente:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart --sdk-auth > quickstart.auth
```

Establezca la variable de entorno `AZURE_AUTH_LOCATION` para que sea la ruta de acceso completa de este archivo. Después, el SDK busca y lee las credenciales directamente desde este archivo, sin que tenga que realizar cambios ni registrar información de la entidad de servicio.

## <a name="get-the-code"></a>Obtención del código

Obtenga el código de inicio rápido y todas sus dependencias con `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm/...
```

No es necesario realizar ninguna modificación en el código fuente si la variable `AZURE_AUTH_LOCATION` está establecida correctamente. Cuando se ejecuta el programa, carga toda la información de autenticación necesaria desde allí.

## <a name="running-the-code"></a>Ejecución del código

Puede ejecutar el inicio rápido con el comando `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstarts/deploy-vm
go run main.go
```

Si se produce un error en la implementación, obtendrá un mensaje que indica que ha habido un problema, pero sin detalles específicos. Mediante la CLI de Azure, puede obtener información completa del error de implementación con el siguiente comando:

```azurecli-interactive
az group deployment show -g GoVMQuickstart -n VMDeployQuickstart
```

Si la implementación se realiza correctamente, verá un mensaje que proporciona el nombre de usuario, la dirección IP y la contraseña para iniciar sesión en la máquina virtual recién creada. Conéctese mediante SSH en este equipo para confirmar que se está ejecutando.

## <a name="cleaning-up"></a>Limpiar

Para limpiar los recursos creados en este inicio rápido, puede eliminar el grupo de recursos con la CLI.

```azurecli-interactive
az group delete -n GoVMQuickstart
```

## <a name="code-in-depth"></a>El código en profundidad

El contenido del código de inicio rápido se desglosa en un bloque de variables y varias funciones pequeñas, cada uno de los cuales se trata aquí.

### <a name="variables-constants-and-types"></a>Variables, constantes y tipos

Puesto que la plantilla de inicio rápido es independiente, usa variables y constantes globales.

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

Los valores declarados proporcionan los nombres de los recursos creados. La ubicación también se especifica aquí y puede cambiarla para ver cómo se comportan las implementaciones en otros centros de datos. No todos los centros de datos tienen todos los recursos necesarios disponibles.

El tipo `clientInfo` se declara para encapsular toda la información que debe cargarse de forma independiente desde el archivo de autenticación para configurar los clientes en el SDK y establecer la contraseña de la máquina virtual.

Las constantes `templateFile` y `parametersFile` apuntan a los archivos necesarios para la implementación. El SDK para Go configurará `authorizer` para autenticación, y la variable `ctx` es un [contexto de Go](https://blog.golang.org/context) para las operaciones de red.

### <a name="authentication-and-initialization"></a>Autenticación e inicialización

La función `init` establece la autenticación. Puesto que la autenticación es una condición previa para todo en la plantilla de inicio rápido, tiene sentido que forme parte de la inicialización. También carga alguna información necesaria desde el archivo de autenticación para configurar los clientes y la máquina virtual.

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

En primer lugar, se invoca a [auth.NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile) para cargar la información de autenticación del archivo se encuentra en `AZURE_AUTH_LOCATION`. Después, la función `readJSON` carga manualmente este archivo (se omite aquí) para extraer los dos valores necesarios para ejecutar el resto del programa: el identificador de la suscripción del cliente y secreto de la entidad de servicio, que también se utiliza para la contraseña de la máquina virtual.

> [!WARNING]
> Para simplificar el inicio rápido, se reutiliza la contraseña de la entidad de servicio. En producción, no olvide que __nunca__ se reutiliza una contraseña que da acceso a los recursos de Azure.

### <a name="flow-of-operations-in-main"></a>Flujo de operaciones en main()

La función `main` es simple, solo indica el flujo de operaciones y realiza la comprobación de errores.

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

Los pasos que ejecuta el código son, en orden:

* Crear el grupo de recursos para la implementación (`createGroup`)
* Crear la implementación dentro del grupo (`createDeployment`)
* Obtener y mostrar la información de inicio de sesión de la máquina virtual implementada (`getLogin`)

### <a name="creating-the-resource-group"></a>Creación del grupo de recursos

La función `createGroup` crea el grupo de recursos. Un examen del flujo de llamadas y los argumentos muestra la forma en la que se estructuran las interacciones del servicio en el SDK.

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

El flujo general de la interacción con un servicio de Azure es:

* Crear el cliente mediante el método `service.New*Client()`, donde `*` es el tipo de recurso de `service` con el que desea interactuar. Esta función siempre tiene un identificador de suscripción.
* Establecer el método de autorización para el cliente, lo que le permite interactuar con la API remota.
* Realizar la llamada al método en el cliente correspondiente a la API remota. Los métodos del cliente del servicio normalmente toman el nombre del recurso y un objeto de metadatos.

La función [`to.StringPtr`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se utiliza aquí para realizar una conversión de tipo. Los parámetros de los métodos del SDK toman casi exclusivamente punteros, por lo que estos métodos se proporcionan para facilitar las conversiones de tipos. Consulte la documentación del módulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) para obtener la lista completa y el comportamiento de los convertidores.

El método `groupsClient.CreateOrUpdate` devuelve un puntero a un tipo de datos que representa al grupo de recursos. Un valor de retorno directo de este tipo indica una operación de ejecución breve que está diseñada para ser sincrónica. En la siguiente sección, verá un ejemplo de una operación de ejecución prolongada y cómo interactuar con ella.

### <a name="performing-the-deployment"></a>Realización de la implementación

Una vez creado el grupo de recursos, es el momento de ejecutar la implementación. Este código se divide en secciones más pequeñas para hacer énfasis en las diferentes partes de su lógica.

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

`readJSON` carga los archivos de la implementación, los detalles de esta operación se omiten aquí. Esta función devuelve un valor del tipo `*map[string]interface{}`, el tipo utilizado para construir los metadatos de la llamada de la implementación de los recursos. La contraseña de la máquina virtual también se establece manualmente en los parámetros de implementación.

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

Este código sigue el mismo patrón que la creación del grupo de recursos. Se crea un nuevo cliente, se le habilita para autenticarse con Azure y, a continuación, se llama a un método. El método incluso tiene el mismo nombre (`CreateOrUpdate`) que el método correspondiente en los grupos de recursos. Este patrón se ve en todo el SDK. Los métodos que realizan un trabajo similar normalmente tienen el mismo nombre.

La principal diferencia está en el valor devuelto por el método `deploymentsClient.CreateOrUpdate`. Este valor es un objeto del tipo [Future](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#Future), que sigue el [patrón de diseño de futuros](https://en.wikipedia.org/wiki/Futures_and_promises). Los futuros representan una operación de ejecución prolongada en Azure que puede sondear, cancelar o bloquear cuando finalice.

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

En este ejemplo, lo mejor es esperar a que finalice la operación. Para esperar a un futuro se necesita tanto un [objeto de contexto](https://blog.golang.org/context) como el cliente que creó el objeto `Future`. Hay dos orígenes de error posibles aquí: un error producido en el cliente cuando se intenta invocar el método y una respuesta de error desde el servidor. El último se devuelve como parte de la llamada a `deploymentFuture.Result`.

Una vez recuperada la información de implementación, si está vacía, hay una solución alternativa que consiste en realizar una llamada manual a `deploymentsClient.Get` para asegurarse de que contiene datos.

### <a name="obtaining-the-assigned-ip-address"></a>Obtención de la dirección IP asignada

Para cualquier operación con la máquina virtual recién creada, se necesita la dirección IP asignada. Las direcciones IP son recursos de Azure independientes, enlazados a los recursos del controlador de interfaz de red (NIC).

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

Este método se basa en la información que se almacena en el archivo de parámetros. El código podría consultar directamente la máquina virtual para obtener la NIC, consultar la NIC para obtener el recurso de IP y, finalmente, consultar el recurso de IP directamente. Es una cadena larga de dependencias y operaciones para resolver, lo que resulta costoso. Puesto que la información de JSON es local, se puede cargar en su lugar.

El valor del usuario de la máquina virtual también se carga desde el código JSON. La contraseña de la máquina virtual se cargó anteriormente desde el archivo de autenticación.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha tomado una plantilla existente para su implementación mediante Go. A continuación, se ha conectado a la máquina virtual recién creada a través de SSH para asegurarse de que está en ejecución.

Para seguir obteniendo información acerca de cómo trabajar con máquinas virtuales en el entorno de Azure con Go, puede consultar los [ejemplos de proceso de Azure para Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) o los [ejemplos de administración de recursos de Azure para Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).

Para obtener más información acerca de los métodos de autenticación disponibles en el SDK y qué tipos de autenticación se admiten, consulte [Autenticación con el SDK de Azure para Go](azure-sdk-go-authorization.md).
