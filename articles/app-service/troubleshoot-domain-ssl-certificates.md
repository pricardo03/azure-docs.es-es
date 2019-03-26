---
title: 'Solución de problemas de dominio y certificados SSL: Azure App Service | Microsoft Docs'
description: Solución de problemas de dominio y certificado SSL en Azure App Service
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: genli
ms.custom: seodec18
ms.openlocfilehash: b367251e6799031ed96058d21e606446e14636c7
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407613"
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-app-service"></a>Solución de problemas de dominio y certificado SSL en Azure App Service

En este artículo se indican los problemas que pueden surgir al configurar un dominio o un certificado SSL para las aplicaciones web en Azure App Service. También se describen las posibles causas de estos problemas y sus soluciones.

Si necesita más ayuda con cualquier aspecto de este artículo, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN o Stack Overflow](https://azure.microsoft.com/support/forums/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Soporte técnico**.

## <a name="certificate-problems"></a>Problemas con el certificado

### <a name="you-cant-add-an-ssl-certificate-binding-to-an-app"></a>No puede agregar un enlace de certificado SSL a una aplicación 

#### <a name="symptom"></a>Síntoma

Cuando agrega un enlace SSL, recibe el mensaje de error siguiente:

"Error al agregar enlace SSL. No se puede configurar el certificado para la IP virtual existente porque otra IP virtual ya usa ese certificado".

#### <a name="cause"></a>Causa

Este problema puede producirse si tiene varios enlaces SSL basados en IP para la misma dirección IP en varias aplicaciones. Por ejemplo, la aplicación A tiene un enlace SSL basado en IP con un certificado antiguo. La aplicación B tiene un enlace SSL basado en IP con un certificado nuevo para la misma dirección IP. Cuando se actualiza el enlace SSL de la aplicación con el nuevo certificado, se produce este error porque se usa la misma dirección IP para otra aplicación. 

#### <a name="solution"></a>Solución 

Para corregirlo, use uno de los siguientes métodos:

- Elimine el enlace SSL basado en IP de la aplicación que usa el certificado antiguo. 
- Cree un nuevo enlace SSL basado en IP que use el nuevo certificado.

### <a name="you-cant-delete-a-certificate"></a>No se puede eliminar un certificado 

#### <a name="symptom"></a>Síntoma

Al intentar eliminar un certificado, recibirá el mensaje de error siguiente:

"No se puede eliminar el certificado porque se está utilizando actualmente en un enlace SSL. Es necesario quitar el enlace SSL para eliminar el certificado".

#### <a name="cause"></a>Causa

Este problema puede producirse si otra aplicación usa el certificado.

#### <a name="solution"></a>Solución

Quite el enlace SSL para el certificado de las aplicaciones. A continuación, intente eliminar el certificado. Si todavía no puede eliminarlo, borre la caché del explorador de Internet y vuelva a abrir Azure Portal en una nueva ventana del explorador. A continuación, intente eliminar el certificado.

### <a name="you-cant-purchase-an-app-service-certificate"></a>No se puede adquirir un certificado de App Service 

#### <a name="symptom"></a>Síntoma
No puede adquirir un [certificado de Azure App Service](./web-sites-purchase-ssl-web-site.md) desde Azure Portal.

#### <a name="cause-and-solution"></a>Causa y solución
Este problema puede producirse por alguno de los siguientes motivos:

- El plan de App Service es Gratis o Compartido. Estos planes de tarifa no admiten SSL. 

    **Solución**: actualice el plan de App Service para la aplicación al Estándar.

- La suscripción no tiene una tarjeta de crédito válida.

    **Solución**: agregue una tarjeta de crédito válida a la suscripción. 

- La oferta de suscripción no es compatible con la adquisición de un certificado de App Service, como Microsoft Student.  

    **Solución**: actualice su suscripción. 

- La suscripción alcanzó el límite de adquisiciones permitidas en una suscripción.

    **Solución**: los certificados de App Service tienen un límite de 10 adquisiciones de certificado para los tipos de suscripción Pago por uso y EA. Para otros tipos de suscripción, el límite es 3. Para aumentar el límite, póngase en contacto con el [soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- El certificado de App Service se ha marcado como fraude. Recibió el siguiente mensaje de error: "Su certificado se ha marcado por posible fraude. La solicitud está actualmente bajo revisión. Si el certificado no es utilizable en 24 horas, póngase en contacto con soporte técnico de Azure."

    **Solución**: si el certificado se ha marcado como fraude y no se resuelve en 24 horas, siga estos pasos:

    1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
    2. Vaya a **Certificados de App Service** y seleccione el certificado.
    3. Seleccione **Configuración del certificado** > **Paso 2: Comprobar** > **Comprobación del dominio**. Este paso envía un aviso por correo electrónico al proveedor de certificados de Azure para que resuelva el problema.

## <a name="custom-domain-problems"></a>Problemas de dominio personalizado

### <a name="a-custom-domain-returns-a-404-error"></a>Un dominio personalizado devuelve un error 404 

#### <a name="symptom"></a>Síntoma

Cuando va al sitio usando el nombre de dominio personalizado, recibe el mensaje de error siguiente:

"Error 404-Web app not found" (Error 404: No se encontró la aplicación web).

#### <a name="cause-and-solution"></a>Causa y solución

**Causa 1** 

En el dominio personalizado configurado falta un registro A o CNAME. 

**Solución para la causa 1**

- Si agregó un registro A, asegúrese de que también se agregó un registro TXT. Para más información, consulte [Crear el registro A](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Si no tiene que usar el dominio raíz de la aplicación, se recomienda usar un registro CNAME en lugar de un registro A.
- No use un registro CNAME y un registro A para el mismo dominio. Este problema puede producir un conflicto y evitar que se resuelve el dominio. 

**Causa 2** 

El explorador de Internet podría seguir almacenando en caché la dirección IP antigua del dominio. 

**Solución para la causa 2**

Desactive el explorador. Para dispositivos Windows, puede ejecutar el comando `ipconfig /flushdns`. Use [WhatsmyDNS.net](https://www.whatsmydns.net/) para comprobar que su dominio apunta a la dirección IP de la aplicación. 

### <a name="you-cant-add-a-subdomain"></a>No se puede agregar un subdominio 

#### <a name="symptom"></a>Síntoma

No se puede agregar un nuevo nombre de host a una aplicación para asignar un subdominio.

#### <a name="solution"></a>Solución

- Compruebe con el administrador de suscripciones que tiene permisos para agregar un nombre de host a la aplicación.
- Si necesita más subdominios, se recomienda que cambie el dominio que hospeda a servicio de nombres de dominio (DNS) de Azure. Mediante el uso de Azure DNS, puede agregar 500 nombres de host a la aplicación. Para más información, consulte el artículo sobre [agregar un subdominio](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).

### <a name="dns-cant-be-resolved"></a>El DNS no se puede resolver

#### <a name="symptom"></a>Síntoma

Recibió el siguiente mensaje de error:

"The DNS record could not be located" (No se pudo localizar el registro DNS).

#### <a name="cause"></a>Causa
Este problema se produce por uno de los siguientes motivos:

- No ha expirado el período de vida (TTL). Compruebe la configuración de DNS del dominio para determinar el valor de TTL y espere a que expire el período.
- La configuración de DNS es incorrecta.

#### <a name="solution"></a>Solución
- Espere 48 horas para que este problema se resuelva por sí mismo.
- Si puede cambiar la configuración de TTL en la configuración de DNS, cambie el valor a 5 minutos para ver si esto resuelve el problema.
- Use [WhatsmyDNS.net](https://www.whatsmydns.net/) para comprobar que su dominio apunta a la dirección IP de la aplicación. Si no es así, configure el registro A con la dirección IP correcta de la aplicación.

### <a name="you-need-to-restore-a-deleted-domain"></a>Es necesario restaurar un dominio eliminado 

#### <a name="symptom"></a>Síntoma
El dominio ya no está visible en Azure Portal.

#### <a name="cause"></a>Causa 
El propietario de la suscripción puede haber eliminado accidentalmente el dominio.

#### <a name="solution"></a>Solución
Si hace menos de siete días se eliminó el dominio, aún no se ha iniciado el proceso de eliminación del dominio. En este caso, puede volver a comprar el mismo dominio en Azure Portal bajo la misma suscripción. (Asegúrese de escribir el nombre de dominio exacto en el cuadro de búsqueda). No se le volverá a cobrar por este dominio. Si el dominio se eliminó hace más de siete días, póngase en contacto con [soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obtener ayuda con la restauración del dominio.

## <a name="domain-problems"></a>Problemas de dominio

### <a name="you-purchased-an-ssl-certificate-for-the-wrong-domain"></a>Adquirió un certificado SSL para un dominio incorrecto

#### <a name="symptom"></a>Síntoma

Adquirió un certificado de App Service para un dominio incorrecto No se puede actualizar el certificado para usar el dominio correcto.

#### <a name="solution"></a>Solución

Elimine ese certificado y, a continuación, compre un certificado nuevo.

Si el certificado actual que utiliza el dominio incorrecto se encuentra en el estado "Emitido", también se le facturará ese certificado. Los certificados de App Service no son reembolsables, pero puede ponerse en contacto con el [soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para ver si hay otras opciones. 

### <a name="an-app-service-certificate-was-renewed-but-the-app-shows-the-old-certificate"></a>Se renovó un certificado de App Service, pero la aplicación muestra el certificado antiguo 

#### <a name="symptom"></a>Síntoma

Se renovó el certificado de App Service, pero la aplicación que usa el certificado de App Service sigue usando el certificado antiguo. Además, recibe la advertencia de que se requiere el protocolo HTTPS.

#### <a name="cause"></a>Causa 
Azure App Service ejecuta un trabajo en segundo plano cada ocho horas que sincroniza el recurso de certificado, si hay algún cambio. Cuando se alterna o actualiza un certificado, la aplicación sigue recuperando, en ocasiones, el certificado antiguo y no el recién actualizado. Esto se debe a que el trabajo de sincronización del recurso de certificado no se ha ejecutado todavía. 
 
#### <a name="solution"></a>Solución

Puede forzar una sincronización del certificado:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Seleccione **Certificados de App Service** y luego el certificado.
2. Seleccione **Regenerar claves y sincronizar** y luego seleccione**Sincronizar**. La sincronización tarda algo de tiempo en finalizar. 
3. Cuando se complete, verá la siguiente notificación: "Se actualizaron correctamente todos los recursos con el certificado más reciente."

### <a name="domain-verification-is-not-working"></a>La comprobación del dominio no funciona 

#### <a name="symptom"></a>Síntoma 
Es necesario comprobar el dominio para poder usar el certificado de App Service. Cuando selecciona **Comprobar**, se produce un error en el proceso.

#### <a name="solution"></a>Solución
Compruebe manualmente el dominio mediante la adición de un registro TXT:
 
1.  Vaya al proveedor del Servicio de nombres de dominio (DNS) que hospeda el nombre de dominio.
2.  Agregue un registro TXT para el dominio que use el valor del token de dominio que se muestra en Azure Portal. 

Espere unos minutos a que tenga lugar la propagación de DNS y seleccione el botón **Actualizar** para desencadenar la comprobación. 

Como alternativa, puede usar el método de página web HTML para comprobar manualmente el dominio. Este método permite que la entidad de certificación confirme la propiedad de dominio del dominio para el que se emitió el certificado.

1.  Cree un archivo HTML denominado {domain verification token}.html. El contenido de este archivo debe ser el valor del token de comprobación de dominio.
3.  Cargue este archivo en la raíz del servidor web que hospeda el dominio.
4.  Seleccione **Actualizar** para comprobar el estado del certificado. La comprobación podría tardar unos minutos en finalizar.

Por ejemplo, si compra un certificado estándar para azure.com con el token de comprobación de dominio 1234abcd, una solicitud web realizada a https://azure.com/1234abcd.html devolvería 1234abcd. 

> [!IMPORTANT]
> Una orden de certificado dispone de tan solo 15 días para completar la operación de comprobación de dominio. Una vez transcurridos esos 15 días, la entidad de certificación denegará el certificado, el que no se le cobrará. En esta situación, elimine el certificado y vuelva a intentarlo.
>
> 

### <a name="you-cant-purchase-a-domain"></a>No se puede adquirir un dominio

#### <a name="symptom"></a>Síntoma
No se puede comprar un dominio de App Service en Azure Portal.

#### <a name="cause-and-solution"></a>Causa y solución

Este problema se produce por uno de los siguientes motivos:

- No hay ninguna tarjeta de crédito en la suscripción de Azure, o bien la tarjeta de crédito no es válida.

    **Solución**: agregue una tarjeta de crédito válida a la suscripción.

- No es el propietario de la suscripción, por lo que no tiene permiso para comprar un dominio.

    **Solución**: [asigne el rol Propietario](../role-based-access-control/role-assignments-portal.md) a la cuenta. O póngase en contacto con el administrador de la suscripción para obtener permiso para comprar un dominio.
- Ha alcanzado el límite de adquisición de dominios de su suscripción. El límite actual es de 20.

    **Solución**: para solicitar un aumento del límite, póngase en contacto con el [soporte técnico de Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- El tipo de suscripción de Azure no admite la adquisición de un dominio de App Service.

    **Solución**: actualice la suscripción de Azure a otro tipos de suscripción, como una suscripción de Pago por uso.

### <a name="you-cant-add-a-host-name-to-an-app"></a>No se puede agregar un nombre de host a una aplicación 

#### <a name="symptom"></a>Síntoma

Cuando agrega un nombre de host, el proceso no puede validar y comprobar el dominio.

#### <a name="cause"></a>Causa 

Este problema se produce por uno de los siguientes motivos:

- No tiene permiso para agregar un nombre de host.

    **Solución**: pida al administrador de la suscripción que le dé permiso para agregar un nombre de host.
- No se pudo comprobar la propiedad del dominio.

    **Solución**: compruebe que los registros CNAME o A están configurados correctamente. Para asignar un dominio personalizado a la aplicación, cree un registro CNAME o A. Si quiere usar un dominio raíz, debe usar registros A y TXT:

    |Tipo de registro|Host|Apunta a|
    |------|------|-----|
    |Una |@|Dirección IP de la aplicación|
    |TXT|@|<nombre-aplicación>.azurewebsites.net|
    |CNAME|www|<nombre-aplicación>.azurewebsites.net|

## <a name="faq"></a>Preguntas más frecuentes

**¿Es necesario configurar mi dominio personalizado para mi sitio Web una vez que lo compraría?**

Al comprar un dominio desde el portal de Azure, se configura automáticamente la aplicación de App Service para usar ese dominio personalizado. No tiene que realizar ningún paso adicional. Para obtener más información, vea [ayuda de autoservicio de Azure App Service: Agregar un nombre de dominio personalizado](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name) en Channel 9.

**¿Puedo usar un dominio comprado en el portal de Azure para que apunte a una máquina virtual de Azure en su lugar?**

Sí, puede señalar el dominio a una máquina virtual. Para más información, consulte [Usar Azure DNS para proporcionar la configuración de un dominio personalizado para un servicio de Azure](../dns/dns-custom-domain.md).

**¿Mi dominio está hospedado por GoDaddy o DNS de Azure?**

Dominios de App Service use GoDaddy para el registro de dominio y DNS de Azure para hospedar los dominios. 

**Tengo la renovación automática habilitada, pero todavía recibe un aviso de renovación para mi dominio a través del correo electrónico. ¿qué debo hacer?**

Si dispone de renovación automática está habilitada, no es necesario realizar ninguna acción. El correo electrónico de aviso se proporciona informar de que el dominio está a punto de expirar y renovar manualmente si la renovación automática no está habilitado.

**¿Se cobrarán para DNS de Azure hospeda mi dominio?**

El costo inicial de la compra de dominio se aplica a solo el registro del dominio. Además del costo de registro, hay ningún costo para Azure DNS en función del uso. Para obtener más información, consulte [precios de Azure DNS](https://azure.microsoft.com/pricing/details/dns/) para obtener más detalles.

**Adquirido mi dominio anteriormente desde el portal de Azure y desea mover de hospedaje de GoDaddy al hospedaje de DNS de Azure. ¿Cómo puedo hacerlo?**

No es obligatorio para migrar al hospedaje de DNS de Azure. Si desea migrar a DNS de Azure, la experiencia de administración de dominio en el portal de Azure sobre proporciona información sobre los pasos necesarios para mover a DNS de Azure. Si se adquirió el dominio a través de App Service, la migración de hospedaje de GoDaddy a DNS de Azure es relativamente sencillo procedimiento.

**Me gustaría adquiere mi dominio del dominio de App Service, pero, ¿puedo hospedar mi dominio en GoDaddy en lugar de DNS de Azure?**

A partir del 24 de julio de 2017, los dominios de App Service adquiridos en el portal se hospedan en Azure DNS. Si prefiere usar un proveedor de hospedaje diferente, debe ir a su sitio Web para obtener una solución de hospedaje de dominios.

**¿Tengo que pagar para la protección de privacidad para mi dominio?**

Al comprar un dominio a través del portal de Azure, puede elegir agregar privacidad sin costo adicional. Esta es una de las ventajas de adquirir el dominio a través de Azure App Service.

**Si decido que ya no deseo que mi dominio, ¿puedo obtener de mi dinero atrás?**

Al comprar un dominio, no se cobran durante un período de cinco días, durante el cual puede decidir que no desea que el dominio. Si decide que no desea que el dominio dentro de ese período de cinco días, no se cobra. (.uk dominios son una excepción a esto. Si adquiere un dominio .uk, se le cobrará inmediatamente y no se reembolsarán.)

**¿Puedo usar el dominio en otra aplicación de Azure App Service en mi suscripción?**

Sí. Cuando tenga acceso a la hoja dominios personalizados y SSL en el portal de Azure, consulte los dominios que haya adquirido. Puede configurar la aplicación para usar cualquiera de esos dominios.

**¿Puedo transferir un dominio de una suscripción a otra suscripción?**

Puede mover un dominio a otro grupo de recursos/suscripción utilizando el [Move-AzureRmResource](https://docs.microsoft.com/powershell/module/AzureRM.Resources/Move-AzureRmResource?view=azurermps-6.13.0) cmdlet de PowerShell.

**¿Cómo puedo administrar mi dominio personalizado si no hay actualmente una aplicación de Azure App Service?**

Puede administrar el dominio, incluso si no tiene una aplicación Web de App Service. Dominio puede usarse para servicios de Azure como Máquina Virtual, etcetera de almacenamiento. Si piensa usar el dominio de App Service Web Apps, deberá incluir una aplicación Web que no está en el plan de App Service gratis con el fin de enlazar el dominio a la aplicación web.

**¿Puedo mover una aplicación web con un dominio personalizado a otra suscripción o de App Service Environment v1 a V2?**

Sí, puede mover la aplicación web en distintas suscripciones. Siga las instrucciones de [cómo trasladar recursos de Azure](../azure-resource-manager/resource-group-move-resources.md). Existen algunas limitaciones al mover la aplicación web. Para obtener más información, consulte [limitaciones para mover recursos de App Service](../azure-resource-manager/resource-group-move-resources.md#app-service-limitations
).

Después de mover la aplicación web, los enlaces de nombre de host de los dominios dentro de la configuración de dominios personalizados deben permanecer igual. Se necesita ningún paso adicional para configurar los enlaces de nombre de host.
