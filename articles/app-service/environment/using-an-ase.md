---
title: Uso y administración de una instancia de App Service Environment
description: Cómo crear, publicar y escalar aplicaciones en Azure App Service Environment. Consulte las tareas habituales en un solo documento.
author: ccompy
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.topic: article
ms.date: 01/01/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 7be1676c8949cd30d5e1fe93a73afd75a5a9b67f
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2020
ms.locfileid: "77565705"
---
# <a name="use-an-app-service-environment"></a>Uso de una instancia de App Service Environment #

App Service Environment (ASE) es una implementación de Azure App Service en una subred de la red virtual de Azure de un cliente. Una instancia de ASE se compone de:

- **Servidores front-end**: es donde las solicitudes HTTP/HTTPS finalizan en las instancias de App Service Environment.
- **Trabajos**: los trabajos son los recursos que hospedan las aplicaciones.
- **Base de datos**: la base de datos contiene información que define el entorno.
- **Almacenamiento**: El almacenamiento se usa para hospedar las aplicaciones publicadas del cliente.

Puede implementar una instancia de ASE con una IP virtual externa o interna que permita el acceso de las aplicaciones. La implementación con una VIP externa suele llamarse ASE externo. La versión interna se llama ASE con un ILB porque usa un equilibrador de carga interno (ILB). Para más información sobre ASE con un ILB, consulte [Creación y uso de un ASE con un ILB][MakeILBASE].

## <a name="create-an-app-in-an-ase"></a>Creación de una aplicación en un entorno ASE ##

Para crear una aplicación en un entorno ASE, se usa el mismo proceso que al crearla normalmente, pero con pequeñas diferencias. Cuando cree un nuevo plan de App Service (ASP):

- En lugar de elegir una ubicación geográfica en la que implementar la aplicación, elija un entorno ASE como su ubicación.
- Todos los planes de App Service que se creen en una instancia de ASE solamente deberán tener un plan de tarifa Aislado.

Si no tiene un entorno ASE, puede crear uno según las instrucciones que aparecen en [Creación de un entorno en App Service Environment][MakeExternalASE].

Para crear una aplicación en un entorno ASE:

1. Seleccione **Crear un recurso** > **Web y móvil** > **Aplicación web**.

2. Escriba un nombre para la aplicación. Si ya seleccionó un plan de App Service en ASE, el nombre de dominio de la aplicación refleja el nombre de dominio del entorno ASE.

    ![Selección del nombre de la aplicación][1]

1. Seleccione una suscripción.

1. Especifique un nombre para un grupo de recursos nuevo o seleccione **Usar existente** y seleccione uno en la lista desplegable.

1. Seleccione el sistema operativo. 

1. Seleccione un plan de App Service existente en ASE o cree uno nuevo siguiendo estos pasos:

    a. En el menú izquierdo de Azure Portal, seleccione **Crear un recurso > Aplicación web**.

    b. Seleccione la suscripción.
    
    c. Seleccione o cree el grupo de recursos.
    
    d. Especifique el nombre de la aplicación web.
    
    e. Seleccione Código o DockerContainer.
    
    f. Seleccione una pila en tiempo de ejecución.
    
    g. Seleccione Linux o Windows. 
    
    h. Seleccione la instancia de ASE en la lista desplegable **Región**. 
    
    i. Seleccione o cree un nuevo plan de App Service. Si va a crear un nuevo plan de App Service, seleccione el tamaño del plan de tarifa **Aislado** que corresponda.
    
    ![Planes de tarifa aislados][2]

    > [!NOTE]
    > Las aplicaciones Windows y Linux no pueden estar en el mismo plan de App Service, pero sí que pueden estar en el mismo entorno de App Service Environment. 
    >

2. Seleccione **Revisar y crear** y, si la información es correcta, haga clic en **Crear**.

## <a name="how-scale-works"></a>Cómo funciona escalar ##

Cada aplicación de App Service se ejecuta en un plan de App Service. Las instancias de App Service Environment contienen planes de App Service, y estos incluyen aplicaciones. Cuando escala una aplicación, escala el plan de App Service y, por tanto, escala todas las aplicaciones en el mismo plan.

Cuando se escala horizontalmente un plan de App Service, de forma automática se agrega la infraestructura necesaria. Hay un retraso para escalar las operaciones mientras se agrega la infraestructura. Si realiza varias operaciones de escalado una detrás de otra, se atenderá a la primera solicitud de escalado de la infraestructura y las demás se podrán en cola. Cuando se complete la primera operación de escalado, el resto de las solicitudes de infraestructura se realizarán a la vez. Cuando se agregue la infraestructura, los planes de App Service se asignarán según corresponda. La propia acción de crear un nuevo plan de App Service es ya una operación de escalado, puesto que requiere hardware adicional. 

En las instancias multiinquilino de App Service, el escalado es inmediato, ya que hay un grupo de destino disponible al instante para realizarlo. En un entorno ASE, no hay ningún búfer de este tipo y los recursos se asignan según sea necesario.

En ASE, un plan de App Service se puede escalar hasta en 100 instancias. Un entorno de ASE puede tener como máximo un total de 201 instancias entre todos los planes de App Service que se encuentran en dicho entorno. 

## <a name="ip-addresses"></a>Direcciones IP ##

App Service tiene la capacidad de asignar una dirección IP dedicada a una aplicación. Esta funcionalidad para asignar una IP dedicada a una aplicación está disponible cuando se configura un protocolo SSL basado en IP, tal y como se describe en el artículo sobre el [enlace de un certificado SSL personalizado con Azure App Service][ConfigureSSL]. En las instancias de ASE con ILB, no se pueden agregar direcciones IP adicionales para usarlas con un protocolo SSL basado en IP.

Con las instancias de ASE externas, se puede configurar un protocolo SSL basado en IP para la aplicación de la misma manera que en una instancia de App Service con varios inquilinos. En ASE; siempre hay una dirección de reserva, hasta un máximo de 30 direcciones IP. Cada vez que usa una, se agrega otra, para que siempre haya una dirección disponible lista para su uso. Un retraso es necesario para asignar otra dirección IP, que evita agregar direcciones IP en una sucesión rápida.

## <a name="front-end-scaling"></a>Escalado de front-end ##

Cuando se escala un plan de App Service, se agregan automáticamente trabajos que contribuyen a la operación de escalado. Cada instancia de ASE se crea con dos servidores front-end. Los servidores front-end se escalan horizontalmente a un ritmo de un servidor front-end por cada 15 instancias de planes de App Service. Si tiene tres planes de App Service con cinco instancias cada uno, tendrá un total de 15 instancias y tres servidores front-end. Si escala a un total 30 instancias, tendrá cuatro servidores front-end, y así sucesivamente. 

El número de servidores front-end que se asignan de forma predeterminada es el adecuado para una carga moderada. Puede reducir el ratio hasta un front-end por cada cinco instancias. También puede cambiar el tamaño de los servidores front-end. De forma predeterminada, tienen un solo núcleo. Puede cambiar el tamaño de los servidores front-end en el portal por dos o cuatro núcleos. No se aplican cargos por cambiar el ratio o el tamaño de los servidores front-end. Para más información, consulte [Precios de Azure App Service][Pricing]. Si quiere mejorar la capacidad de carga de la instancia de ASE, disfrutará de un mejor rendimiento si escala primero a servidores front-end de dos núcleos antes de ajustar el ratio de escalado. Al cambiar el tamaño de núcleo de los servidores front-end, se actualizará la instancia de ASE, lo que debe hacerse fuera del horario comercial normal.

Los recursos de los servidores front-end son el punto de conexión HTTP/HTTPS de la instancia de ASE. Con la configuración predeterminada de los servidores front-end, el uso de memoria de cada front-end se sitúa sistemáticamente en torno al 60 %. La razón principal para escalar los servidores front-end es la CPU, lo que normalmente viene determinado por el tráfico HTTPS.

## <a name="app-access"></a>Acceso a las aplicaciones ##

En las instancias externas de ASE, el sufijo del dominio que se utiliza para crear aplicaciones es *.&lt;nombreASE&gt;.p.azurewebsites.net*. Por ejemplo, si la instancia de ASE se llama _external-ase_ y la aplicación _contoso_ se hospeda en ese ASE, puede obtener acceso utilizando las siguientes direcciones URL:

- contoso.external-ase.p.azurewebsites.net
- contoso.scm.external-ase.p.azurewebsites.net

Para más información acerca de cómo crear un entorno ASE externo, consulte [Creación de una instancia externa de App Service Environment][MakeExternalASE].

En una instancia de ASE con ILB, el sufijo del dominio que se utiliza para crear aplicaciones es *.&lt;nombreASE&gt;.appserviceenvironment.net*. Si la instancia de ASE se llama _ilb-ase_ y la aplicación _contoso_ se hospeda en ese ASE, puede obtener acceso utilizando las siguientes direcciones URL:

- contoso.ilb-ase.appserviceenvironment.net
- contoso.scm.ilb-ase.appserviceenvironment.net

Para más información sobre cómo crear un entorno ASE con un ILB, consulte [Creación y uso de un ASE con un ILB][MakeILBASE]. 

La dirección URL del SCM se utiliza para obtener acceso a la consola de Kudu o para publicar la aplicación mediante una implementación web. Para información sobre la consola Kudu, consulte el artículo sobre la [consola Kudu para Azure App Service][Kudu]. La consola Kudu ofrece una UI web para depurar, cargar archivos, editar archivos y mucho más.

## <a name="publishing"></a>Publicación ##

Al igual que lo que ocurre con App Service multiinquilino, en un entorno ASE puede publicar con:

- Implementación web.
- FTP.
- Integración continua.
- Operación de arrastrar y soltar en la consola Kudu.
- Un IDE como Visual Studio, Eclipse o IntelliJ IDEA.

Con un entorno ASE externo, todas estas opciones de publicación se comportan del mismo modo. Para más información, consulte [Implementación en Azure App Service][AppDeploy]. 

La gran diferencia con la publicación tiene que ver con un ASE con un ILB. Con un ASE con un ILB, los puntos de conexión de publicación solo están disponibles a través de ILB. El ILB está en una dirección IP privada en la subred de ASE en la red virtual. Si no tiene acceso de red al ILB, no puede publicar ninguna aplicación en ese entorno ASE. Tal como se indica en [Creación y uso de un ASE con un ILB][MakeILBASE], debe configurar DNS para las aplicaciones del sistema. Eso incluye el punto de conexión de SCM. Si no se han definido correctamente, no puede llevar a cabo la publicación. Los IDE también deben tener acceso de red al ILB para publicar directamente en él.

Los sistemas de CI basados en Internet listos para usar, como GitHub y Azure DevOps, no funcionan con un ASE con un ILB, ya que el punto de conexión de publicación no es accesible desde Internet. Puede habilitar la publicación en una instancia de ASE con ILB desde Azure DevOps instalando un agente de versiones autohospedado en la red virtual que contiene la instancia de ASE con ILB. También puede utilizar un sistema de CI que use un modelo de extracción, como Dropbox.

Los puntos de conexión de publicación para las aplicaciones en un ASE con un ILB usan el dominio con el que se creó el ASE con un ILB. Puede verlo en el perfil de publicación de la aplicación y en la hoja del portal de la aplicación (en **Información general** > **Información esencial** y también en **Propiedades**). 

## <a name="storage"></a>Storage

Una instancia de ASE dispone de 1 TB de almacenamiento para todas las aplicaciones que contiene. El plan de tarifa Aislado de App Service tiene un límite predeterminado de 250 GB. Si tiene cinco o más planes de App Service, debe tener cuidado para no superar el límite de ASE de 1 TB. Si necesita un límite superior a 250 GB en un plan de App Service, póngase en contacto con el servicio de soporte técnico para ajustar el límite del plan de App Service a un máximo de 1 TB. Cuando se ajuste el límite del plan, seguirá habiendo un límite de 1 TB en todos los planes de App Service de la instancia de ASE. 

## <a name="logging"></a>Registro ##

Puede integrar la instancia de ASE con Azure Monitor para enviar registros sobre esta instancia de ASE a Storage, Event Hubs o Log Analytics. En la actualidad, los elementos que se registran son:

| Situación | Message |
|---------|----------|
| ASE no tiene un estado correcto | La instancia de ASE especificada no tiene un estado correcto porque la configuración de la red virtual no es válida. La instancia de ASE se suspenderá si se mantiene un estado que no es correcto. Asegúrese de que se siguen las directrices indicadas aquí: https://docs.microsoft.com/azure/app-service/environment/network-info |
| La subred de ASE casi no tiene espacio | La instancia de ASE especificada está en una subred que casi no tiene espacio. Quedan {0} direcciones. Una vez que se agoten estas direcciones, la instancia de ASE no se podrá escalar.  |
| ASE está a punto de alcanzar el límite total de instancias | La instancia de ASE especificada está a punto de alcanzar al límite total de instancias. Actualmente contiene {0} instancias de planes de App Service, de un máximo de 201 instancias. |
| ASE no puede obtener acceso a una dependencia | La instancia de ASE especificada no puede obtener acceso a {0}.  Asegúrese de que se siguen las directrices indicadas aquí: https://docs.microsoft.com/azure/app-service/environment/network-info |
| La instancia de ASE está suspendida | La instancia de ASE especificada está suspendida. Esta suspensión puede deberse a que hay pocas cuentas o a que una configuración de red virtual no es válida. Resuelva la causa principal del problema y reanude la instancia de ASE para que siga atendiendo el tráfico. |
| Se ha iniciado una actualización de ASE | Se ha iniciado una actualización de la plataforma a la instancia de ASE especificada. Es posible que se produzcan retrasos en las operaciones de escalado. |
| Se ha completado una actualización de ASE | Ha finalizado una actualización de plataforma de la instancia de ASE especificada. |
| Se han iniciado operaciones de escalado | Se ha comenzado a escalar un plan de App Service ({0}). Estado deseado: {1} I{2} trabajos 
| Se han completado las operaciones de escalado | Ha finalizado el escalado de un plan de App Service ({0}). Estado actual: {1} I{2} trabajos |
| Se han producido errores en las operaciones de escalado | No se ha podido escalar un plan de App Service ({0}). Estado actual: {1} I{2} trabajos |

Puede habilitar el registro en la instancia de ASE: 

1. Vaya a Configuración de diagnóstico en el portal.  
1. Seleccione Agregar configuración de diagnóstico.
1. Especifique un nombre para la integración de registros.
1. Compruebe y configure los destinos de registro que desee. 
1. Compruebe AppServiceEnvironmentPlatformLogs.

![Configuración de registros de diagnóstico de ASE][4]

Si integra esta solución con Log Analytics, podrá ver los registros seleccionando Registros en el portal de ASE y creando una consulta en AppServiceEnvironmentPlatformLogs. 

## <a name="upgrade-preference"></a>Preferencia de actualización ##

Si tiene varias instancias de ASE, es posible que prefiera que unas se actualicen antes que otras. En el objeto HostingEnvironment del ASE de Resource Manager, puede establecer un valor en upgradePreference. El valor de upgradePreference se puede configurar utilizando una plantilla, ARMClient o https://resources.azure.com.  Los tres valores posibles son:

* None (Ninguno): es el valor predeterminado y significa que Azure actualizará la instancia de ASE de cualquiera de los lotes, sin que haya ninguno en particular.
* Early (Pronto): significa que la instancia de ASE se actualizará durante la primera mitad de las actualizaciones de App Service.
* Late (Tarde): significa que la instancia de ASE se actualizará durante la segunda mitad de las actualizaciones de App Service.

Si usa https://resources.azure.com, puede establecer el valor de upgradePreferences del modo siguiente:

1. Vaya a resources.azure.com e inicie sesión con su cuenta de Azure.
1. Acceda a subscriptions\/\[nombre de suscripción\]\/resourceGroups\/\[nombre del grupo de recursos\]\/providers\/Microsoft.Web\/hostingEnvironments\/\[nombre de ASE\]
1. Seleccione Lectura y escritura en la parte superior.
1. Seleccione Editar.
1. Cambie el valor de upgradePreference por la opción que desee.
1. Seleccione Revisión.

![Pantalla de recursos de azure.com][5]

En realidad, tiene más sentido utilizar la característica upgradePreferences cuando hay varias instancias de ASE, ya que las que estén configuradas con el valor "Early" (Pronto) se actualizarán antes de las que tienen el valor "Late" (Tarde). Si tiene varias instancias de ASE, los entornos de ASE de desarrollo y pruebas deben estar establecidos en "Early" (Pronto), mientras que los entornos de ASE de producción deben estar establecidos en "Late" (Tarde).

## <a name="pricing"></a>Precios ##

El plan de tarifa **Aislado** solo puede utilizarse con ASE. Todos los planes de App Service que se hospedan en la instancia de ASE están en el plan de tarifa Aislado. Las tarifas del plan Isolated de App Service pueden variar por región. 

Además del precio de los planes de App Service, hay una tarifa fija para ASE mismo. La tarifa plana no cambia con el tamaño de la instancia de ASE y se aplica a la infraestructura de ASE con una velocidad de escalado predeterminada de un front-end adicional por cada 15 instancias del plan de App Service.  

Si la velocidad de escalado predeterminada de un front-end por cada 15 instancias del plan de App Service no es lo suficientemente rápida, puede ajustar el ratio con el que se agregan servidores front-end o el tamaño de estos.  Si ajusta el ratio o el tamaño, pagará por los núcleos de front-end adicionales que no se agregarían de forma predeterminada.  

Por ejemplo, si ajusta el ratio de escalado a 10, se agregará un front-end por cada 10 instancias de los planes de App Service. La tarifa plana cubrirá una velocidad de escalado de un front-end por cada 15 instancias. Con un ratio de escalado de 10, se aplicará una tarifa por el tercer servidor front-end que se agregue para las 10 instancias de planes de App Service. No es necesario que pague por él si llega a 15 instancias, ya que se agregó automáticamente.

Si ajusta el tamaño de los front-end a dos núcleos pero no se ajusta el ratio, pagará por los núcleos adicionales.  Las instancias de ASE se crean con dos servidores front-end, por lo que incluso estando por debajo del umbral de escalado automático, pagaría por dos núcleos adicionales si aumentara el tamaño a servidores front-end con dos núcleos.

Para más información, consulte [Precios de Azure App Service][Pricing].

## <a name="delete-an-ase"></a>Eliminar un entorno ASE ##

Para eliminar un entorno ASE: 

1. Use **Eliminar** en la parte superior de la hoja **App Service Environment**. 

1. Escriba el nombre de su ASE para confirmar que desea eliminarlo. Cuando se elimina un entorno ASE, se elimina también todo su contenido. 

    ![Eliminación de ASE][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png
[4]: ./media/using_an_app_service_environment/usingase-logsetup.png
[4]: ./media/using_an_app_service_environment/usingase-logs.png
[5]: ./media/using_an_app_service_environment/usingase-upgradepref.png


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/management/overview.md
[ConfigureSSL]: ../configure-ssl-certificate.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../deploy-local-git.md
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
