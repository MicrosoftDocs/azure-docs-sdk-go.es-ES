---
title: Autenticación con el SDK de Azure para Go
description: Obtenga información sobre los métodos de autenticación disponibles en el SDK de Azure para Go y cómo utilizarlos.
services: azure
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: conceptual
ms.technology: azure-sdk-go
ms.devlang: go
ms.component: authentication
ms.openlocfilehash: f5c2c56e43828f0bedad0b5781dc71991ce1fd3e
ms.sourcegitcommit: 172f81dd6e4c6a275dc8031815aa87cdb488cbf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47231682"
---
# <a name="authentication-methods-in-the-azure-sdk-for-go"></a>Métodos de autenticación en el SDK de Azure para Go

Azure SDK para Go ofrece varias maneras de autenticarse con Azure. Estos _tipos_ de autenticación se invocan con diferentes _métodos_ de autenticación. En este artículo se describen los tipos y métodos disponibles, y cómo elegir los más adecuados para su aplicación.

## <a name="available-authentication-types-and-methods"></a>Métodos y tipos de autenticación disponibles

El SDK de Azure para Go ofrece varios tipos diferentes de autenticación, mediante el uso de diferentes conjuntos de credenciales. Cada tipo de autenticación está disponible mediante diferentes métodos de autenticación, que explican cómo el SDK toma estas credenciales como datos de entrada. En la siguiente tabla se describen los tipos de autenticación disponibles y las situaciones en las que la aplicación recomienda utilizarlos.

| Tipo de autenticación | Recomendado si... |
|---------------------|---------------------|
| Autenticación basada en certificados | Tiene un certificado X509 configurado para un usuario o entidad de servicio de Azure Active Directory (AAD). Para más información, consulte [Introducción a la autenticación basada en certificados de Azure Active Directory]. |
| Credenciales de cliente | Tiene una entidad de servicio configurada que está preparada para esta aplicación o una clase de aplicaciones a las que pertenece. Para más información, consulte [Creación de una entidad de servicio con la CLI de Azure]. |
| Identidades administradas de recursos de Azure | La aplicación se ejecuta en un recurso de Azure que se ha configurado con una identidad administrada. Para más información, consulte [Identidades administradas para recursos de Azure]. |
| Token del dispositivo | La aplicación está diseñada para usarse __solo__ interactivamente. Los usuarios pueden tener habilitada la autenticación multifactor. Los usuarios tienen acceso a un explorador web para iniciar sesión. Para más información, consulte [Use device token authentication](#use-device-token-authentication) (Uso de la autenticación por tokens del dispositivo).|
| Nombre de usuario/contraseña | Tiene una aplicación interactiva que no puede utilizar ningún otro método de autenticación. Los usuarios no tienen habilitada la autenticación multifactor para su inicio de sesión de AAD. |

> [!IMPORTANT]
> Si usa un tipo de autenticación diferente a las credenciales de cliente, la aplicación debe estar registrada en Azure Active Directory. Para más información, consulte [Integración de aplicaciones con Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).
>
> [!NOTE]
> A menos que tenga requisitos especiales, evite la autenticación por nombre de usuario o contraseña. En situaciones en las que el inicio de sesión basado en usuario es apropiado, normalmente se puede utilizar la autenticación por tokens del dispositivo.

[Introducción a la autenticación basada en certificados de Azure Active Directory]: /azure/active-directory/active-directory-certificate-based-authentication-get-started
[Creación de una entidad de servicio con la CLI de Azure]: /cli/azure/create-an-azure-service-principal-azure-cli
[Identidades administradas para recursos de Azure]: /azure/active-directory/managed-identities-azure-resources/overview

Estos tipos de autenticación están disponibles a través de diferentes métodos.

* [_La autenticación basada en entornos_](#use-environment-based-authentication) lee las credenciales directamente desde el entorno del programa.
* [_La autenticación basada en archivos_ ](#use-file-based-authentication) carga un archivo que contiene las credenciales de la entidad de servicio.
* [_La autenticación basada en cliente_](#use-an-authentication-client) utiliza un objeto en el código y le hace responsable de proporcionar las credenciales durante la ejecución del programa.
* [_La autenticación de token de dispositivo_](#use-device-token-authentication) requiere que los usuarios inicien sesión interactivamente mediante un explorador web con un token.

Todas las funciones y tipos de autenticación están disponibles en el paquete `github.com/Azure/go-autorest/autorest/azure/auth`.

> [!NOTE]
> A menos que tenga requisitos especiales, evite la autenticación basada en cliente. Este método de autenticación fomenta las malas prácticas. En particular, el uso de la autenticación basada en cliente hace que sea tentador codificar de forma rígida las credenciales. La escritura de código personalizado para la autenticación también puede interrumpirse en futuras versiones del SDK si cambian los requisitos de autenticación.

## <a name="use-environment-based-authentication"></a>Uso de la autenticación basada en entornos

Si ejecuta la aplicación en un entorno controlado, la autenticación basada en entornos es la opción natural. Con este método de autenticación, el shell de entorno se configura antes de ejecutar la aplicación. En tiempo de ejecución, el SDK para Go lee estas variables de entorno para autenticarse con Azure.

La autenticación basada en entornos es compatible con todos los métodos de autenticación excepto con los tokens de dispositivo, que se evalúan en el siguiente orden:

* Credenciales de cliente
* Certificados X509
* Nombre de usuario/contraseña
* Identidades administradas de recursos de Azure

Si un tipo de autenticación tiene valores no establecidos o se rechaza, el SDK intenta automáticamente con el siguiente tipo de autenticación. Cuando no hay más tipos disponibles para probar, el SDK de devuelve un error.

En la tabla siguiente se detallan las variables de entorno que deben configurarse para cada tipo de autenticación admitida por la autenticación basada en entornos.

| Tipo de autenticación | Variable de entorno | DESCRIPCIÓN |
| ------------------- | -------------------- | ----------- |
| __Credenciales de cliente__ | `AZURE_TENANT_ID` | El identificador del inquilino de Active Directory al que pertenece la entidad de servicio. |
| | `AZURE_CLIENT_ID` | El nombre o identificador de la entidad de servicio. |
| | `AZURE_CLIENT_SECRET` | Secreto asociado a la entidad de servicio. |
| __Certificate__ | `AZURE_TENANT_ID` | El identificador del inquilino de Active Directory con el que está registrado el certificado. |
| | `AZURE_CLIENT_ID` | El identificador del cliente de aplicación asociado al certificado. |
| | `AZURE_CERTIFICATE_PATH` | La ruta de acceso a un archivo de certificado de cliente. |
| | `AZURE_CERTIFICATE_PASSWORD` | La contraseña para el certificado de cliente. |
| __Nombre de usuario/contraseña__ | `AZURE_TENANT_ID` | El identificador del inquilino de Active Directory al que pertenece el usuario. |
| | `AZURE_CLIENT_ID` | El identificador del cliente de aplicación. |
| | `AZURE_USERNAME` | El nombre de usuario con el que se va a iniciar sesión. |
| | `AZURE_PASSWORD` | La contraseña con la que se va a iniciar sesión. |
| __Identidad administrada__ | | No se requieren credenciales para la autenticación con identidades administradas. La aplicación debe ejecutarse en un recurso de Azure configurado para usar identidades administradas. Para más información, consulte [Identidades administradas para recursos de Azure]. |

Si necesita conectarse a un punto de conexión de nube o de administración distinto de la nube pública Azure predeterminada, establezca las siguientes variables de entorno. Las razones más comunes para establecerlas son si se usa Azure Stack, una nube en una región geográfica diferente o el modelo de implementación clásico de Azure.

| Variable de entorno | DESCRIPCIÓN  |
|----------------------|--------------|
| `AZURE_ENVIRONMENT` | El nombre del entorno de nube al que se conectará. |
| `AZURE_AD_RESOURCE` | El identificador de recurso de Active Directory que se usará al conectarse, como el URI de un punto de conexión de administración. |

Cuando se usa la autenticación basada en entornos, llame a la función [NewAuthorizerFromEnvironment](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromEnvironment) para obtener el objeto autorizador. Este objeto se establece entonces en la propiedad `Authorizer` de los clientes para permitirles el acceso a Azure.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := auth.NewAuthorizerFromEnvironment()
```

### <a name="authentication-on-azure-stack"></a>Autenticación en Azure Stack

Para autenticarse en Azure Stack, debe establecer las siguientes variables:

| Variable de entorno | DESCRIPCIÓN  |
|----------------------|--------------|
| `AZURE_AD_ENDPOINT` | El punto de conexión de Active Directory. |
| `AZURE_AD_RESOURCE` | El identificador de recurso de Active Directory. |

Estas variables se pueden recuperar de la información de metadatos de Azure Stack. Para recuperar los metadatos, abra un explorador web en su entorno de Azure Stack y use la dirección URL: `(ResourceManagerURL)/metadata/endpoints?api-version=1.0`

El valor de `ResourceManagerURL` varía según el nombre de la región, el nombre de la máquina y el nombre de dominio completo (FQDN) externo de su implementación de Azure Stack:

| Entorno | ResourceManagerURL |
|----------------------|--------------|
| Kit de desarrollo | `https://management.local.azurestack.external/` |
| Sistemas integrados | `https://management.(region).ext-(machine-name).(FQDN)` |

Para más información sobre cómo usar Azure SDK para Go en Azure Stack, consulte [Uso de perfiles de versión de la API con Go en Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles-go).

## <a name="use-file-based-authentication"></a>Uso de la autenticación basada en archivos

La autenticación basada en archivos usa un formato de archivo generado por [la CLI de Azure](/cli/azure). Puede crear fácilmente este archivo cuando se crea una nueva entidad de servicio con el parámetro `--sdk-auth`. Si planea utilizar la autenticación basada en archivos, asegúrese de que se proporciona este argumento al crear una entidad de servicio. Como la CLI imprime la salida en `stdout`, redirija la salida a un archivo.

```azurecli
az ad sp create-for-rbac --sdk-auth > azure.auth
```

Establezca la variable de entorno `AZURE_AUTH_LOCATION` en donde se encuentra el archivo de autorización. La aplicación lee esta variable de entorno y analiza las credenciales que contiene. Si tiene que seleccionar el archivo de autorización en tiempo de ejecución, manipule el entorno del programa mediante la función [os.Setenv](https://golang.org/pkg/os/#Setenv).

Para cargar la información de autenticación, llame a la función [NewAuthorizerFromFile](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewAuthorizerFromFile). A diferencia de la autorización basada en entornos, la autorización basada en archivos requiere un punto de conexión de recursos.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
authorizer, err := NewAuthorizerFromFile(azure.PublicCloud.ResourceManagerEndpoint)
```

Para más información acerca del uso de entidades de servicio y la administración de los permisos de acceso, consulte [Creación de una entidad de servicio con la CLI de Azure].

## <a name="use-device-token-authentication"></a>Uso de la autenticación por tokens del dispositivo

Si desea que los usuarios inicien sesión de forma interactiva, la mejor manera es mediante la autenticación por token de dispositivo. Este flujo de autenticación pasa al usuario un token para pegarlo en un sitio de inicio de sesión de Microsoft, donde luego se autentica con una cuenta de Azure Active Directory (AAD). Este método de autenticación admite cuentas que tienen habilitada la autenticación multifactor, a diferencia de la autenticación estándar de nombre de usuario y contraseña.

Para usar la autenticación por tokens del dispositivo, cree un autorizador de [DeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig) con la función [NewDeviceFlowConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#NewDeviceFlowConfig). Llame al [autorizador](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig.Authorizer) en el objeto resultante para iniciar el proceso de autenticación. La autenticación por flujo de dispositivo bloquea la ejecución del programa hasta que se complete todo el flujo de autenticación.

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
deviceConfig := auth.NewDeviceFlowConfig(applicationID, tenantID)
authorizer, err := deviceConfig.Authorizer()
```

## <a name="use-an-authentication-client"></a>Uso de un cliente de autenticación

Si necesita un tipo específico de autenticación y está dispuesto a que el programa haga el trabajo de cargar la información de autenticación del usuario, puede utilizar cualquier cliente que se ajuste a la interfaz [auth.AuthorizerConfig](https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#AuthorizerConfig). Use un tipo que implementa esta interfaz si:

* Está escribiendo un programa interactivo
* Usa archivos de configuración especializados
* Tiene un requisito que impide el uso de un método de autenticación integrado

> [!WARNING]
> Nunca codifique de forma rígida las credenciales de Azure en una aplicación. Poner secretos en un archivo binario de una aplicación hace más fácil su extracción por parte de un atacante, ya se esté ejecutando la aplicación o no. Esto pone en riesgo todos los recursos de los Azure cuyas credenciales están autorizadas.

En la tabla siguiente se enumeran los tipos en el SDK que se ajustan a la interfaz `AuthorizerConfig`.

| Tipo de autenticación | Tipo de autorizador |
|---------------------|-----------------------|
| Autenticación basada en certificados | [ClientCertificateConfig] |
| Credenciales de cliente | [ClientCredentialsConfig] |
| Identidades administradas de recursos de Azure | [MSIConfig] |
| Nombre de usuario/contraseña | [UsernamePasswordConfig] |

[ClientCertificateConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCertificateConfig
[ClientCredentialsConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#ClientCredentialsConfig
[MSIConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#MSIConfig
[DeviceFlowConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#DeviceFlowConfig
[UsernamePasswordConfig]: https://godoc.org/github.com/Azure/go-autorest/autorest/azure/auth#UsernamePasswordConfig

Cree un autenticador con su función `New` asociada y, después, llame a `Authorize` en el objeto resultante para realizar la autenticación. Por ejemplo, para usar la autenticación basada en certificado:

```go
import "github.com/Azure/go-autorest/autorest/azure/auth"
certificateAuthorizer := auth.NewClientCertificateConfig(certificatePath, certificatePassword, clientID, tenantID)
authorizerToken, err := certificateAuthorizer.Authorize()
```
