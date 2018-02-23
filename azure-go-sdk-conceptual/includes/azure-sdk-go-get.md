El SDK de Azure para Go es compatible con las versiones de Go 1.8 y posteriores. Para entornos con [perfiles de Azure Stack](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-version-profiles), la versión 1.9 de Go es el requisito mínimo. Si no dispone de Go en el sistema, siga [las instrucciones de instalación Go](https://golang.org/doc/install).

Puede obtener el SDK de Azure para Go y sus dependencias a través de `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Asegúrese de escribir en mayúsculas `Azure` en la dirección URL. De lo contrario puede causar problemas de importación relacionados con las mayúsculas y minúsculas cuando se trabaja con el SDK. También debe escribir en mayúscula `Azure` en las instrucciones de importación.

