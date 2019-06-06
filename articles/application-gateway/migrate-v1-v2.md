---
title: Migrar Azure Application Gateway y Firewall de aplicaciones de v1 a v2 Web
description: En este artículo se muestra cómo migrar Azure Application Gateway y Firewall de aplicaciones Web de v1 a v2
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/5/2019
ms.author: victorh
ms.openlocfilehash: 44d5ce3e194c873a564039934f518cb3a0e142e3
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497177"
---
# <a name="migrate-azure-application-gateway-and-web-application-firewall-from-v1-to-v2"></a>Migrar Azure Application Gateway y Firewall de aplicaciones de v1 a v2 Web

[Azure Application Gateway y Firewall de aplicaciones Web (WAF) v2](application-gateway-autoscaling-zone-redundant.md) ya está disponible, ofrece características adicionales como redundancia de zona de disponibilidad y de escalado automático. Sin embargo, las puertas de enlace de v1 existente no se actualizan automáticamente a v2. Si desea migrar de v1 a v2, siga los pasos descritos en este artículo.

Hay dos fases en una migración:

1. Migrar la configuración
2. Migrar el tráfico de cliente

Este artículo trata sobre la migración de la configuración. Migración de tráfico de cliente varía dependiendo de su entorno específico. Sin embargo, algunas recomendaciones generales, de alto nivel [se proporcionan](#migrate-client-traffic).

## <a name="migration-overview"></a>Información general sobre migración

Está disponible un script de PowerShell de Azure que hace lo siguiente:

* Crea una nueva puerta de enlace Standard_v2 o WAF_v2 en una subred de red virtual que especifique.
* Copia a la perfección la configuración asociada con la puerta de enlace v1 estándar o WAF a la puerta de enlace Standard_V2 o WAF_V2 recién creado.

### <a name="caveatslimitations"></a>Caveats\Limitations

* La nueva puerta de enlace de v2 tiene nuevas direcciones IP públicas y privadas. No es posible mover las direcciones IP asociadas con la puerta de enlace de v1 existente sin problemas a v2. Sin embargo, puede asignar una (sin asignar) pública o privada dirección IP existente a la nueva puerta de enlace de v2.
* Debe proporcionar un espacio de direcciones IP para otra subred dentro de la red virtual donde se encuentra la puerta de enlace de v1. La secuencia de comandos no puede crear la puerta de enlace de v2 en las subredes existentes que ya tienen una puerta de enlace de v1. Sin embargo, si la subred existente ya tiene una puerta de enlace de v2, que puede funcionar siempre que haya suficiente espacio de direcciones IP.
* Para migrar una configuración de SSL, debe especificar todos los certificados SSL utilizados en la puerta de enlace de v1.
* Si tiene el modo FIPS habilitado para la puerta de enlace de V1, no se migrarán a la nueva puerta de enlace de v2. No se admite el modo FIPS en v2.
* v2 no es compatible con IPv6, por lo que no se migran las puertas de enlace de v1 de IPv6 habilitado. Si ejecuta la secuencia de comandos, no puede completar.
* Si la puerta de enlace de v1 tiene sólo una dirección IP privada, el script crea una dirección IP pública y una dirección IP privada para la nueva puerta de enlace de v2. las puertas de enlace v2 actualmente no admiten sólo las direcciones IP privadas.

## <a name="download-the-script"></a>Descargue el script

Descargar el script de migración desde el [Galería de PowerShell](https://www.powershellgallery.com/packages/AzureAppGWMigration).

## <a name="use-the-script"></a>Use el script

Hay dos opciones para usted según sus preferencias y la configuración del entorno de PowerShell local:

* Si no dispone de los módulos de Azure Az instalados o no le importa al desinstalar los módulos de Azure Az, la mejor opción es usar el `Install-Script` opción para ejecutar el script.
* Si necesita mantener los módulos de Azure Az, la mejor opción es descargar el script y ejecutarlo directamente.

Para determinar si dispone de los módulos de Azure Az instalados, ejecute `Get-InstalledModule -Name az`. Si no ve ninguno instalado módulos de Az, a continuación, puede usar el `Install-Script` método.

### <a name="install-using-the-install-script-method"></a>Instalar mediante el método Install-Script

Para usar esta opción, no debe tener los módulos de Azure Az instalados en el equipo. Si están instalados, el comando siguiente muestra un error. Puede desinstalar los módulos de Azure Az o usar la otra opción para descargar manualmente el script y ejecútelo.
  
Ejecute el siguiente comando para ejecutar el script:

`Install-Script -Name AzureAppGWMigration`

Este comando también instala los módulos necesarios de Az.  

### <a name="install-using-the-script-directly"></a>Instalar mediante el script directamente

Si tiene algunos módulos de Azure Az instalados y no se puede desinstalar ellos (o los no quiere desinstalarlas), puede descargar manualmente el script mediante el **descarga Manual** ficha en el vínculo de descarga de la secuencia de comandos. El script se descarga como un archivo nupkg sin procesar. Para instalar la secuencia de comandos de este archivo nupkg, consulte [Manual paquete descargar](https://docs.microsoft.com/powershell/gallery/how-to/working-with-packages/manual-download).

Para ejecutar el script:

1. Use `Connect-AzAccount` para conectarse a Azure.

1. Use `Import-Module Az` para importar los módulos de Az.

1. Ejecute `Get-Help AzureAppGWMigration.ps1` para examinar los parámetros necesarios:

   `AzureAppGwMigration.ps1
    -resourceId <v1 application gateway Resource ID>
    -subnetAddressRange <subnet space you want to use>
    -appgwName <string to use to append>
    -sslCertificates <comma-separated SSLCert objects as above>
    -trustedRootCertificates <comma-separated Trusted Root Cert objects as above>
    -privateIpAddress <private IP string>
    -publicIpResourceName <public IP name string>
    -validateMigration -enableAutoScale`

   Parámetros del script:
   * **resourceId: [String]: Requiere** : este es el identificador de recurso de Azure para su v1 estándar existente o una puerta de enlace de v1 de WAF. Para buscar este valor de cadena, vaya a Azure portal, seleccione la puerta de enlace de aplicaciones o recursos de WAF y haga clic en el **propiedades** vínculo para la puerta de enlace. El identificador de recurso se encuentra en la página.

     También puede ejecutar los siguientes comandos de PowerShell de Azure para obtener el identificador de recurso:

     ```azurepowershell
     $appgw = Get-AzApplicationGateway -Name <v1 gateway name> -ResourceGroupName <resource group Name> 
     $appgw.Id
     ```

   * **subnetAddressRange: [String]:  Requiere** -se trata el espacio de direcciones IP que ha asignado (o si desea asignar) para una nueva subred que contiene la nueva puerta de enlace de v2. Esto se debe especificar en la notación CIDR. Por ejemplo: 10.0.0.0/24. No es necesario crear de antemano esta subred. El script crea automáticamente si no existe.
   * **appgwName: [String]: Opcional**. Se trata de una cadena que especifica para usar como el nombre de la nueva puerta de enlace Standard_v2 o WAF_v2. Si no se proporciona este parámetro, se usará el nombre de la puerta de enlace de v1 existente con el sufijo *_v2* anexado.
   * **sslCertificates: [PSApplicationGatewaySslCertificate]: Opcional**.  Una lista separada por comas de objetos PSApplicationGatewaySslCertificate que cree para representar los certificados SSL de la puerta de enlace de v1 debe cargarse en la nueva puerta de enlace de v2. Para cada uno de los certificados SSL configurados para su v1 estándar o una puerta de enlace de v1 de WAF, puede crear un nuevo objeto PSApplicationGatewaySslCertificate a través de la `New-AzApplicationGatewaySslCertificate` comando que se muestra aquí. Necesita la ruta de acceso al archivo de certificado SSL y la contraseña.

       Este parámetro solo es opcional si no dispone de los agentes de escucha HTTPS configurados para la puerta de enlace de v1 o WAF. Si tiene al menos un programa de instalación de agente de escucha HTTPS, debe especificar este parámetro.

      ```azurepowershell  
      $password = ConvertTo-SecureString <cert-password> -AsPlainText -Force
      $mySslCert1 = New-AzApplicationGatewaySslCertificate -Name "Cert01" `
        -CertificateFile <Cert-File-Path-1> `
        -Password $password 
      $mySslCert2 = New-AzApplicationGatewaySslCertificate -Name "Cert02" `
        -CertificateFile <Cert-File-Path-2> `
        -Password $password
      ```

      Puede pasar en `$mySslCert1, $mySslCert2` (separados por comas) en el ejemplo anterior como valores para este parámetro en la secuencia de comandos.
   * **trustedRootCertificates: [PSApplicationGatewayTrustedRootCertificate]: Opcional**. Una lista separada por comas de objetos PSApplicationGatewayTrustedRootCertificate que cree para representar el [certificados raíz de confianza](ssl-overview.md) para la autenticación de las instancias de back-end de la puerta de enlace de v2.  

      Para crear una lista de objetos PSApplicationGatewayTrustedRootCertificate, consulte [New AzApplicationGatewayTrustedRootCertificate](https://docs.microsoft.com/powershell/module/Az.Network/New-AzApplicationGatewayTrustedRootCertificate?view=azps-2.1.0&viewFallbackFrom=azps-2.0.0).
   * **privateIpAddress: [String]: Opcional**. Una dirección privada específica IP que desea asociar a la nueva puerta de enlace de v2.  Debe tratarse de la misma red virtual que asignar a la nueva puerta de enlace de v2. Si esto no se especifica, la secuencia de comandos asigna una dirección IP privada para la puerta de enlace de v2.
    * **publicIpResourceId: [String]: Opcional**. El valor de resourceId de un recurso de dirección IP pública en la suscripción que desea asignar a la nueva puerta de enlace de v2. Si no se especifica, la secuencia de comandos asigna una nueva dirección IP pública en el mismo grupo de recursos. El nombre es el nombre de la puerta de enlace del v2 con *- IP* anexado.
   * **validateMigration: [switch]: Opcional**. Utilice este parámetro si desea que la secuencia de comandos para realizar alguna configuración básica de validaciones de comparación después de la creación de la puerta de enlace de v2 y la copia de configuración. De forma predeterminada, no se realiza ninguna validación.
   * **enableAutoScale: [cambiar]: Opcional**. Utilice este parámetro si desea que la secuencia de comandos para habilitar el escalado automático en la nueva puerta de enlace de v2 después de crearlo. De forma predeterminada, el escalado automático está deshabilitado. Puede siempre manualmente habilitarla más adelante en la puerta de enlace de v2 recién creado.

1. Ejecute el script con los parámetros adecuados.

    **Ejemplo**

   ```azurepowershell
   AzureAppGWMigration.ps1 `
      -resourceId /subscriptions/8b1d0fea-8d57-4975-adfb-308f1f4d12aa/resourceGroups/MyResourceGroup/providers/Microsoft.Network/applicationGateways/myv1appgateway `
      -subnetAddressRange 10.0.0.0/24 `
      -appgwname "MynewV2gw" `
      -sslCertificates $Certs `
      -trustedRootCertificates $trustedCert `
      -privateIpAddress "10.0.0.1" `
      -publicIpResourceName "MyPublicIP" `
      -validateMigration -enableAutoScale
   ```

## <a name="migrate-client-traffic"></a>Migración de tráfico del cliente

En primer lugar, compruebe que el script creado correctamente una nueva puerta de enlace de v2 con la configuración exacta se migrado a través de la puerta de enlace de v1. Puede comprobarlo en Azure portal.

Además, puede enviar una pequeña cantidad de tráfico a través de la puerta de enlace de v2 como una prueba manual.
  
Estos son algunos escenarios donde la puerta de enlace de la aplicación actual (estándar) puede recibir tráfico de clientes y nuestras recomendaciones para cada uno de ellos:

* **Una zona DNS personalizada (por ejemplo, contoso.com) que señala a la dirección IP de front-end (mediante un registro) asociados al v1 estándar o una puerta de enlace de v1 WAF**.

    Puede actualizar el registro DNS para que apunte a la etiqueta DNS o IP de front-end asociada con la puerta de enlace de la aplicación Standard_v2. Según el valor de TTL configurado en el registro de DNS, puede tardar un rato para todo el tráfico de cliente migrar a la nueva puerta de enlace de v2.
* **Una zona DNS personalizada (por ejemplo, contoso.com) que apunta a la etiqueta de DNS (por ejemplo: *myappgw.eastus.cloudapp.azure.com* con un registro CNAME) asociado con la puerta de enlace de v1**.

   Tiene dos opciones:

  * Si usa direcciones IP públicas en la puerta de enlace de aplicaciones, puede hacer controlado, migración granular mediante un perfil de Traffic Manager para enrutar el tráfico (método de enrutamiento de tráfico ponderado) de forma incremental a la nueva puerta de enlace de v2.

    Puede hacerlo mediante la adición de las etiquetas DNS de v1 y v2 application gateway para la [perfil de Traffic Manager](../traffic-manager/traffic-manager-routing-methods.md#weighted-traffic-routing-method)y daremos un el registro de DNS personalizado (por ejemplo, www.contoso.com) al dominio (por ejemplo, el Administrador de tráfico contoso.trafficmanager.NET).
  * O bien, puede actualizar el registro DNS del dominio personalizado para que apunte a la etiqueta DNS de la nueva puerta de enlace de aplicaciones v2. Según el valor de TTL configurado en el registro de DNS, puede tardar un rato para todo el tráfico de cliente migrar a la nueva puerta de enlace de v2.
* **Los clientes conectarse al front-end de dirección IP de la puerta de enlace de la aplicación**.

   Actualice los clientes para usar las direcciones IP asociadas con la puerta de enlace de aplicaciones v2 recién creado. Se recomienda que no use direcciones IP directamente. Considere el uso de la etiqueta de nombre DNS (por ejemplo, yourgateway.eastus.cloudapp.azure.com) asociado con la puerta de enlace de aplicaciones que pueden CNAME a su propia zona DNS personalizada (por ejemplo, contoso.com).

## <a name="common-questions"></a>Preguntas frecuentes

### <a name="are-there-any-limitations-with-the-azure-powershell-script-to-migrate-the-configuration-from-v1-to-v2"></a>¿Hay alguna limitación con el script de PowerShell de Azure para migrar la configuración de v1 a v2?

Sí. Consulte [advertencias y limitaciones](#caveatslimitations).

### <a name="is-this-article-and-the-azure-powershell-script-applicable-for-application-gateway-waf-product-as-well"></a>¿Es aplicable para el producto de WAF de Application Gateway también en este artículo y el script de PowerShell de Azure? 

Sí.

### <a name="does-the-azure-powershell-script-also-switch-over-the-traffic-from-my-v1-gateway-to-the-newly-created-v2-gateway"></a>¿El script de PowerShell de Azure también cambiará el tráfico desde mi puerta de enlace de v1 a la puerta de enlace recién creado v2?

No. El script de PowerShell de Azure solo migra la configuración. Migración de tráfico real es su responsabilidad y bajo su control.

### <a name="is-the-new-v2-gateway-created-by-the-azure-powershell-script-sized-appropriately-to-handle-all-of-the-traffic-that-is-currently-served-by-my-v1-gateway"></a>¿Es la nueva puerta de enlace v2 creado por el script de PowerShell de Azure tiene el tamaño adecuado para controlar todo el tráfico que actualmente es atendido por mi puerta de enlace de v1?

El script de PowerShell de Azure crea una nueva puerta de enlace de v2 con un tamaño adecuado para controlar el tráfico en la puerta de enlace de V1 existente. El escalado automático está deshabilitado de forma predeterminada, pero puede habilitar el escalado automático cuando se ejecuta la secuencia de comandos.

### <a name="i-ran-into-some-issues-with-using-this-script-how-can-i-get-help"></a>Topé con algunos problemas con el uso de esta secuencia de comandos. ¿Cómo puedo obtener ayuda?
  
Puede enviar un correo electrónico a appgwmigrationsup@microsoft.com, abra una incidencia con soporte técnico de Azure, o ambas cosas.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información acerca de la aplicación v2 de la puerta de enlace](application-gateway-autoscaling-zone-redundant.md)
