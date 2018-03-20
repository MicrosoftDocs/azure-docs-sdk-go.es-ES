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
# <a name="quickstart-deploy-an-azure-virtual-machine-from-a-template-with-the-azure-sdk-for-go"></a>Inicio rápido: implementación de una máquina virtual de Azure desde una plantilla con el SDK de Azure para Go

Este inicio rápido se centra en la implementación de recursos desde una plantilla con el SDK de Azure para Go. Las plantillas son instantáneas de todos los recursos dentro de un [grupo de recursos de Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview). A lo largo de la guía, se irá familiarizado con la funcionalidad y las convenciones del SDK al tiempo que realiza una tarea útil.

Al final de este inicio rápido, tendrá una máquina virtual en ejecución en la que podrá iniciar sesión con un nombre de usuario y una contraseña.

[!INCLUDE [quickstarts-free-trial-note](includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](includes/cloud-shell-try-it.md)]

Si usa una instalación local de la CLI de Azure, este inicio rápido requiere la versión 2.0.24 de la CLI o posterior. Ejecute `az --version` para asegurarse de que la instalación de la CLI cumple este requisito. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure 2.0](/cli/azure/install-azure-cli).

## <a name="install-the-azure-sdk-for-go"></a>Instalación del SDK de Azure para Go 

[!INCLUDE [azure-sdk-go-get](includes/azure-sdk-go-get.md)]

## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

Para iniciar sesión de una manera no interactiva con una aplicación, necesita una entidad de servicio. Las entidades de servicio son parte del control de acceso basado en rol (RBAC), que crea una identidad de usuario única. Para crear una nueva entidad de servicio con la CLI, ejecute el comando siguiente:

```azurecli-interactive
az ad sp create-for-rbac --name az-go-vm-quickstart
```

__Asegúrese de__  registrar los valores de `appId`, `password` y `tenant` de la salida. La aplicación utiliza estos valores para autenticarse con Azure.

Para más información sobre la creación y administración de entidades de servicio con la CLI de Azure 2.0, consulte [Creación de una entidad de servicio de Azure con la CLI de Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli).

## <a name="get-the-code"></a>Obtención del código

Obtenga el código de inicio rápido y todas sus dependencias con `go get`.

```bash
go get -u -d github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm/...
```

Este código se compila, pero no se ejecuta correctamente hasta que le proporciona información acerca de la cuenta de Azure y la entidad de seguridad de servicio creada. En `main.go` hay una variable, `config`, que contiene una estructura `authInfo`. Se deben reemplazar los valores de los campos de esta estructura para poder autenticarse correctamente.

```go
    config = authInfo{ // Your application credentials
        TenantID:               "", // Azure account tenantID
        SubscriptionID:         "", // Azure subscription subscriptionID
        ServicePrincipalID:     "", // Service principal appId
        ServicePrincipalSecret: "", // Service principal password/secret
    }
```

* `SubscriptionID`: el identificador de la suscripción, que se puede obtener con los comandos CLI

  ```azurecli-interactive
  az account show --query id -o tsv
  ```

* `TenantID`: el identificador del inquilino, el valor de `tenant` registrado al crear la entidad de servicio
* `ServicePrincipalID`: el valor de `appId` registrado al crear la entidad de servicio
* `ServicePrincipalSecret`: el valor de `password` registrado al crear la entidad de servicio

También debe modificar un valor en el archivo `vm-quickstart-params.json`.

```json
    "vm_password": {
        "value": "_"
    }
```

* `vm_password`: la contraseña de la cuenta del usuario de la máquina virtual. Debe tener de 12 a 72 caracteres de longitud y contener 3 de los siguientes caracteres:
  * Una letra minúscula
  * Una letra mayúscula
  * Un número
  * Un símbolo

## <a name="running-the-code"></a>Ejecución del código

Puede ejecutar el inicio rápido con el comando `go run`.

```bash
cd $GOPATH/src/github.com/azure-samples/azure-sdk-for-go-samples/quickstart/deploy-vm
go run main.go
```

Si se produce un error en la implementación, obtendrá un mensaje que indica que ha habido un problema, pero sin detalles específicos. Mediante la CLI de Azure, puede obtener los detalles del error de implementación con el siguiente comando:

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

### <a name="variable-assignments-and-structs"></a>Asignaciones de variables y estructuras

Puesto que el inicio rápido es independiente, usa variables globales en lugar de opciones de la línea de comandos o variables de entorno.

```go
type authInfo struct {
        TenantID               string
        SubscriptionID         string
        ServicePrincipalID     string
        ServicePrincipalSecret string
}
```

La estructura `authInfo` se declara para encapsular toda la información necesaria para la autorización con los servicios de Azure.

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

Los valores declarados proporcionan los nombres de los recursos creados. La ubicación también se especifica aquí y puede cambiarla para ver cómo se comportan las implementaciones en otros centros de datos. No todos los centros de datos tienen todos los recursos necesarios disponibles.

Las constantes `templateFile` y `parametersFile` apuntan a los archivos necesarios para la implementación. El token de la entidad de servicio se trata más adelante y la variable `ctx` es un [contexto de Go](https://blog.golang.org/context) para las operaciones de red.

### <a name="init-and-authorization"></a>init() y la autorización

El método `init()` del código configura la autorización. Puesto que la autorización es una condición previa para todo en el inicio rápido, tiene sentido tenerlo como parte de la inicialización. 

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

Este código realiza dos pasos para la autorización:

* La información de configuración de OAuth del `TenantID` se recupera mediante una interfaz con Azure Active Directory. El objeto [`azure.PublicCloud`](https://godoc.org/github.com/Azure/go-autorest/autorest/azure#PublicCloud) contiene los puntos de conexión utilizados en la configuración de Azure estándar.
* Se llama a la función [`adal.NewServicePrincipalToken()`](https://godoc.org/github.com/Azure/go-autorest/autorest/adal#NewServicePrincipalToken). Esta función toma la información de OAuth junto con el inicio de sesión de la entidad de servicio, así como el estilo de administración de Azure que se está usando. A menos que tenga requisitos específicos y sepa lo que está haciendo, este valor debe ser siempre `.ResourceManagerEndpoint`.

### <a name="flow-of-operations-in-main"></a>Flujo de operaciones en main()

La función `main()` es simple, solo indica el flujo de operaciones y realiza la comprobación de errores.

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

Los pasos que ejecuta el código son, en orden:

* Crear el grupo de recursos para la implementación (`createGroup()`)
* Crear la implementación dentro del grupo (`createDeployment()`)
* Obtener y mostrar la información de inicio de sesión de la máquina virtual implementada (`getLogin()`)

### <a name="creating-the-resource-group"></a>Creación del grupo de recursos

La función `createGroup()` crea el grupo de recursos. Un examen del flujo de llamadas y los argumentos muestra la forma en la que se estructuran las interacciones del servicio en el SDK.

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

El flujo general de la interacción con un servicio de Azure es:

* Crear el cliente mediante el método `service.NewXClient()`, donde `X` es el tipo de recurso de `service` con el que desea interactuar. Esta función siempre tiene un identificador de suscripción.
* Establecer el método de autorización para el cliente, lo que le permite interactuar con la API remota.
* Realizar la llamada al método en el cliente correspondiente a la API remota. Los métodos del cliente del servicio normalmente toman el nombre del recurso y un objeto de metadatos.

La función [`to.StringPtr()`](https://godoc.org/github.com/Azure/go-autorest/autorest/to#StringPtr) se utiliza aquí para realizar una conversión de tipo. Las estructuras de parámetros de los métodos del SDK toman casi exclusivamente punteros, por lo que estos métodos se proporcionan para facilitar las conversiones de tipos. Consulte la documentación del módulo [autorest/to](https://godoc.org/github.com/Azure/go-autorest/autorest/to) para obtener la lista completa y el comportamiento de los convertidores de comodidad.

La operación `groupsClient.CreateOrUpdate()` devuelve un puntero a una estructura de datos que representa al grupo de recursos. Un valor de retorno directo de este tipo indica una operación de ejecución breve que está diseñada para ser sincrónica. En la siguiente sección, verá un ejemplo de una operación de ejecución prolongada y cómo interactuar con ella.

### <a name="performing-the-deployment"></a>Realización de la implementación

Una vez creado el grupo que contiene los recursos, es el momento de ejecutar la implementación. Este código se divide en secciones más pequeñas para hacer énfasis en las diferentes partes de su lógica.


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

`readJSON` carga los archivos de la implementación, los detalles de esta operación se omiten aquí. Esta función devuelve un valor del tipo `*map[string]interface{}`, el tipo utilizado para construir los metadatos de la llamada de la implementación de los recursos.

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

Este código sigue el mismo patrón que la creación del grupo de recursos. Se crea un nuevo cliente, se le habilita para autenticarse con Azure y, a continuación, se llama a un método. El método incluso tiene el mismo nombre (`CreateOrUpdate`) que el método correspondiente en los grupos de recursos. Este patrón se ve una y otra vez en el SDK. Los métodos que realizan un trabajo similar normalmente tienen el mismo nombre.

La principal diferencia está en el valor devuelto por el método `deploymentsClient.CreateOrUpdate()`. Este valor es un objeto `Future`, que sigue el [patrón de diseño futuros y promesas](https://en.wikipedia.org/wiki/Futures_and_promises). Los futuros representan una operación de larga duración en Azure que desea sondear ocasionalmente mientras realiza otro trabajo.

```go
        //...
        err = deploymentFuture.Future.WaitForCompletion(ctx, deploymentsClient.BaseClient.Client)
        if err != nil {
                log.Fatalf("Error while waiting for deployment creation: %v", err)
        }
        return deploymentFuture.Result(deploymentsClient)
}
```

En este ejemplo, lo mejor es esperar a que finalice la operación. Esperar a un futuro requiere tanto un [objeto de contexto](https://blog.golang.org/context) como el cliente que creó el objeto futuro. Hay dos orígenes de error posibles aquí: un error producido en el cliente cuando se intenta invocar el método y una respuesta de error desde el servidor. El último se devuelve como parte de la llamada a `deploymentFuture.Result()`.

### <a name="obtaining-the-assigned-ip-address"></a>Obtención de la dirección IP asignada

Para cualquier operación con la máquina virtual recién creada, se necesita la dirección IP asignada. Las direcciones IP son recursos de Azure independientes, enlazados a los recursos del controlador de interfaz de red (NIC).

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

Este método se basa en la información que se almacena en el archivo de parámetros. El código podría consultar directamente la máquina virtual para obtener la NIC, consultar la NIC para obtener el recurso de IP y, finalmente, consultar el recurso de IP directamente. Es una cadena larga de dependencias y operaciones para resolver, lo que resulta costoso. Puesto que la información de JSON es local, se puede cargar en su lugar.

Del mismo modo, los valores para el usuario y la contraseña de la máquina virtual se cargan desde el archivo JSON.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha tomado una plantilla existente para su implementación mediante Go. A continuación, se ha conectado a la máquina virtual recién creada a través de SSH para asegurarse de que está en ejecución.

Para seguir obteniendo información acerca de cómo trabajar con máquinas virtuales en el entorno de Azure con Go, puede consultar los [ejemplos de proceso de Azure para Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/compute) o los [ejemplos de administración de recursos de Azure para Go](https://github.com/Azure-Samples/azure-sdk-for-go-samples/tree/master/resources).
