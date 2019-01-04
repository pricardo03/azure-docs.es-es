---
title: Administración de recomendaciones de seguridad en Azure Security Center | Microsoft Docs
description: Este documento explica cómo las recomendaciones del Centro de seguridad de Azure ayudan a proteger los recursos de Azure y a cumplir con las directivas de seguridad.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2018
ms.author: rkarlin
ms.openlocfilehash: d0c61f6e905ca109f3f178996a08f353c36e7880
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53337213"
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Administración de recomendaciones de seguridad en el Centro de seguridad de Azure
En este documento se explica cómo usar las recomendaciones del Centro de seguridad de Azure para proteger los recursos de Azure.

> [!NOTE]
> En este documento se presenta el servicio mediante una implementación de ejemplo.  Este documento no es una guía paso a paso.
>
>

## <a name="what-are-security-recommendations"></a>¿Cuáles son las recomendaciones de seguridad?
El Centro de seguridad analiza periódicamente el estado de seguridad de los recursos de Azure. Cuando el Centro de seguridad identifica vulnerabilidades de seguridad potenciales, crea recomendaciones. Las recomendaciones le guían en el proceso de configuración de los controles necesarios.

## <a name="implementing-security-recommendations"></a>Implementación de recomendaciones de seguridad
### <a name="set-recommendations"></a>Obtención de recomendaciones
En [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md), aprenderá lo siguiente:

* Configurar directivas de seguridad.
* Activar la recopilación de datos.
* Elegir las recomendaciones que verá como parte de la directiva de seguridad.

Las recomendaciones de directiva actuales se centran en las actualizaciones del sistema, las reglas de línea de base, los programas antimalware, los [grupos de seguridad de red](../virtual-network/security-overview.md) en subredes y las interfaces de red, la auditoría de bases de datos de SQL, el cifrado de datos transparente de bases de datos de SQL y los firewalls de aplicaciones web.  [Establecimiento de directivas de seguridad](tutorial-security-policy.md) proporciona una descripción de cada opción de recomendación.

### <a name="monitor-recommendations"></a>Supervisión de recomendaciones
Después de establecer una directiva de seguridad, el Centro de seguridad analiza el estado de seguridad de los recursos, con el fin de identificar vulnerabilidades potenciales. El icono **Recomendaciones** de **Información general** permite conocer el número total de recomendaciones que identifica Security Center.

![Icono Recomendaciones][1]

Para ver los detalles de cada recomendación, seleccione el icono **Recomendaciones** en **Información general**. **Recomendaciones** se abre.

![Filtrar recomendaciones][2]

Puede filtrar las recomendaciones. Para ello, seleccione **Filtro** en la hoja **Recomendaciones**. Se abrirá la hoja **Filtro** , donde podrá seleccionar los valores de gravedad y de estado que quiera ver.

Las recomendaciones aparecen en un formato de tabla, donde cada línea representa una recomendación determinada. Las columnas de esta tabla son:

* **DESCRIPCIÓN**: se explica la recomendación y lo que es preciso hacer para abordarla.
* **RECURSO**: enumera los recursos a los que se aplica la recomendación.
* **ESTADO**: describe el estado actual de la recomendación:
  * **Abierta**: la recomendación aún no se ha abordado.
  * **En curso**: la recomendación se está aplicando actualmente a los recursos y no se requiere ninguna acción por su parte.
  * **Resuelta**: se ha completado la recomendación (en este caso la línea aparecerá atenuada).
* **GRAVEDAD**: describe la gravedad de una recomendación concreta:
  * **Alta**: existe una vulnerabilidad en un recurso importante (como una aplicación, una máquina virtual o un grupo de seguridad de red) y requiere atención.
  * **Media**: existe una vulnerabilidad y se requieren pasos adicionales o no críticos para eliminarla o completar un proceso.
  * **Baja**: existe una vulnerabilidad que se debe abordar, pero no requiere una atención inmediata. (De manera predeterminada no se muestran las recomendaciones bajas, pero si desea verlas, puede filtrar por ellas).

Use la tabla siguiente como referencia para ayudarle a entender las recomendaciones disponibles y lo que hará cada uno si se aplica.

> [!NOTE]
> Deberá comprender los [modelos de implementación clásico y de Resource Manager](../azure-classic-rm.md) para los recursos de Azure.
>
>

| Recomendación | DESCRIPCIÓN |
| --- | --- |
| [Habilitar la colección de datos de las suscripciones](security-center-enable-data-collection.md) |Recomienda activar la recopilación de datos en la directiva de seguridad para cada una de las suscripciones y para todas las máquinas virtuales de Azure y equipos que no son Azure. |
| [Corregir configuraciones de seguridad](security-center-remediate-os-vulnerabilities.md) |Recomienda armonizar las configuraciones del SO con las reglas de configuración de seguridad recomendadas; por ejemplo, no permitir guardar las contraseñas. |
| [Aplicar actualizaciones del sistema](security-center-apply-system-updates.md) |Recomienda implementar las actualizaciones críticas y de seguridad del sistema que faltan en los equipos y máquinas virtuales de Windows y Linux. |
| [Aplicación del control de acceso a redes Just-In-Time](security-center-just-in-time.md) | Se recomienda que aplique acceso a la máquina virtual Just-In-Time. La característica Just-In-Time está disponible en el nivel estándar de Security Center. Para obtener más información sobre los planes de tarifa de Security Center, vea [Precios](security-center-pricing.md). |
| [Reiniciar tras actualizar el sistema](security-center-apply-system-updates.md#reboot-after-system-updates) |Se recomienda que reinicie una máquina virtual para completar el proceso de aplicación de actualizaciones del sistema. |
| [Add a web application firewall](security-center-add-web-application-firewall.md) |Recomienda implementar un Firewall de aplicaciones web (WAF) para los puntos de conexión web. Se muestra una recomendación WAFS para cualquier IP pública (dirección IP de nivel de instancia o con equilibrio de carga) que tiene un grupo de seguridad de red asociado con puertos abiertos web entrantes (80 y 443). </br>Security Center le recomendará que aprovisione un WAF para defenderse de ataques dirigidos a las aplicaciones web que se encuentran tanto en las máquinas virtuales como en App Service Environment. Un entorno de App Service es una opción de plan de servicio [Premium](https://azure.microsoft.com/pricing/details/app-service/) de Azure App Service que proporciona un entorno plenamente aislado y dedicado para ejecutar de forma segura las aplicaciones de Azure App Service. Para más información acerca de ASE, consulte [Documentación de App Service Environment](../app-service/environment/intro.md).</br>Puede proteger varias aplicaciones web del Centro de seguridad si agrega estas aplicaciones a las implementaciones de WAF existentes. |
| [Finalización de la protección de la aplicación](security-center-add-web-application-firewall.md#finalize-application-protection) |Para completar la configuración de un WAF, el tráfico se debe redirigir a la aplicación del WAF. Si se sigue esta recomendación, se completarán los cambios necesarios en la configuración. |
| [Add a Next Generation Firewall](security-center-add-next-generation-firewall.md) |Recomienda agregar un firewall de próxima generación (NGFW) de un asociado de Microsoft para aumentar la protección. |
| [Enrutar el tráfico solo a través de NGFW](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Recomienda configurar reglas de grupos de seguridad de red (NSG) que fuercen que el tráfico entrante pase a su máquina virtual mediante el NGFW. |
| [Instalación de Endpoint Protection](security-center-install-endpoint-protection.md) |Recomienda aprovisionar programas antimalware a las máquinas virtuales (solo máquinas virtuales de Windows). |
| [Habilitar grupos de seguridad de red en subredes o máquinas virtuales](security-center-enable-network-security-groups.md) |Recomienda habilitar NSG en subredes o máquinas virtuales. |
| [Restringir el acceso a través de puntos de conexión accesibles desde Internet](security-center-restrict-access-through-internet-facing-endpoints.md) |Recomienda configurar reglas de tráfico de entrada para los NSG. |
| [Habilitar la auditoría y la detección de amenazas en los servidores SQL Server](security-center-enable-auditing-on-sql-servers.md) |Recomienda activar la detección de amenazas y la auditoría para los servidores de Azure SQL. (solo el servicio SQL de Azure. No incluye los servidores SQL que se ejecutan en las máquinas virtuales). |
| [Habilitación de la auditoría y la detección de amenazas en bases de datos SQL](security-center-enable-auditing-on-sql-databases.md) |Recomienda activar la detección de amenazas y la auditoría en las bases de datos de Azure SQL. (solo el servicio SQL de Azure. No incluye los servidores SQL que se ejecutan en las máquinas virtuales). |
| [Habilitar Cifrado de datos transparente en bases de datos SQL](security-center-enable-transparent-data-encryption.md) |Recomienda habilitar el cifrado en las bases de datos SQL (Solo el servicio SQL de Azure). |
| [Habilitar el Agente de máquina virtual](security-center-enable-vm-agent.md) |Permite ver las VM que requieren el Agente de VM. El agente de máquina virtual debe estar instalado en las máquinas virtuales para aprovisionar la detección de revisiones, la detección de línea de base y los programas antimalware. De manera predeterminada, el agente de máquina virtual está instalado en las máquinas virtuales que se implementan desde Azure Marketplace. El artículo [VM Agent and Extensions – Part 2](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) (Agente de VM y extensiones, parte 2) proporciona información sobre cómo instalar el Agente de VM. |
| [Aplicar cifrado de discos](security-center-apply-disk-encryption.md) |Se recomienda cifrar los discos de la máquina virtual mediante Azure Disk Encryption (máquinas virtuales Linux y Windows). Se recomienda cifrar tanto los volúmenes de datos como los del sistema operativo en la máquina virtual. |
| [Proporcionar datos de los contactos de seguridad](security-center-provide-security-contact-details.md) |Recomienda que proporcione información de los contactos de seguridad para cada una de sus suscripciones. La información de los contactos es una dirección de correo electrónico y un número de teléfono. La información se usará para establecer contacto con usted si nuestro equipo de seguridad descubre que sus recursos están en peligro. |
| [Actualizar versión del sistema operativo](security-center-update-os-version.md) |Recomienda actualizar la versión del sistema operativo para el servicio en la nube a la versión más reciente disponible para la familia del sistema operativo.  Para obtener más información sobre Cloud Services, consulte la [información general sobre Cloud Services](../cloud-services/cloud-services-choose-me.md). |
| [Evaluación de vulnerabilidades no instalada](security-center-vulnerability-assessment-recommendations.md) |Se recomienda instalar una solución de evaluación de vulnerabilidades en la máquina virtual. |
| [Corrección de vulnerabilidades](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Permite ver las vulnerabilidades tanto del sistema como de las aplicaciones que ha detectado la solución de evaluación de vulnerabilidades instalada en la máquina virtual. |
| [Habilitar el cifrado para la cuenta de Azure Storage](security-center-enable-encryption-for-storage-account.md) | Es recomendable que habilite el cifrado del servicio de Azure Storage para datos en reposo. El Cifrado de servicio de almacenamiento (SSE) funciona mediante el cifrado de los datos cuando se escriben en Azure Storage y el descifrado antes de la recuperación. SSE actualmente solo está disponible para Azure Blob service y puede usarse para blobs en bloques, blobs de página y blobs en anexos. Para obtener más información, consulte [Cifrado del servicio Azure Storage para datos en reposo (versión preliminar)](../storage/common/storage-service-encryption.md).</br>SSE solo es compatible con las cuentas de almacenamiento de Resource Manager. |
| [Habilitar controles de aplicaciones adaptables](security-center-adaptive-application.md) | Se recomienda que aplique controles de aplicación adaptables en las máquinas virtuales de Windows. La característica está disponible en el nivel estándar de Security Center. Para obtener más información sobre los planes de tarifa de Security Center, vea [Precios](security-center-pricing.md). |
| Solo se debería poder acceder a App Service a través de HTTPS | Se recomienda limitar el acceso de App Service a través de HTTPS solamente. |
| Recomendación de desactivación de Web Sockets para aplicaciones web| Se recomienda que revise cuidadosamente el uso de Web Sockets en las aplicaciones web.  El protocolo Web Sockets es vulnerable a distintos tipos de amenazas de seguridad. |
| Uso de dominios personalizados para su aplicación web | Se recomienda el uso de dominios personalizados para proteger una aplicación web frente a ataques comunes, como la suplantación de identidad y otros ataques relacionados con el DNS. |
| Configuración de las restricciones de IP de las aplicaciones web | Se recomienda definir una lista de direcciones IP que tienen permiso para acceder a su aplicación.  El uso de restricciones de IP protege una aplicación web frente a ataques comunes. |
| No permita que todos los recursos ("*") tengan acceso a la aplicación | Se recomienda no configurar el parámetro WEBSITE_LOAD_CERTIFICATES en "*". Si establece el parámetro en "*" , significa que se van a cargar todos los certificados en el almacén de certificados personales de su aplicación web.  Esto puede dar lugar a un abuso del principio de privilegio mínimo, ya que es poco probable que el sitio necesite tener acceso a todos los certificados en tiempo de ejecución. |
| CORS no debería permitir que todos los recursos accedan a la aplicación | Se recomienda que solo permita que los dominios requeridos interactúen con la aplicación web. El uso compartido de recursos entre orígenes (CORS) no debe permitir que todos los dominios accedan a la aplicación web. |
| Uso de la última versión compatible de .NET Framework para aplicaciones web | Se recomienda que utilice la versión más reciente de .NET Framework para las clases de seguridad más recientes. El uso de tipos y clases anteriores puede hacer que la aplicación quede expuesta. |
| Uso de la última versión compatible de Java para aplicaciones web | Se recomienda que utilice la versión más reciente de Java para las clases de seguridad más recientes. El uso de tipos y clases anteriores puede hacer que la aplicación quede expuesta. |
| Uso de la última versión compatible de Java para aplicaciones web | Se recomienda que utilice la versión más reciente de PHP para las clases de seguridad más recientes. El uso de tipos y clases anteriores puede hacer que la aplicación quede expuesta. |
| [Add a web application firewall](security-center-add-web-application-firewall.md) |Recomienda implementar un Firewall de aplicaciones web (WAF) para los puntos de conexión web. Se muestra una recomendación WAFS para cualquier IP pública (dirección IP de nivel de instancia o con equilibrio de carga) que tiene un grupo de seguridad de red asociado con puertos abiertos web entrantes (80 y 443).</br></br>Security Center le recomendará que aprovisione un WAF para defenderse de ataques dirigidos a las aplicaciones web que se encuentran tanto en las máquinas virtuales como en App Service Environment. Un entorno de App Service es una opción de plan de servicio [Premium](https://azure.microsoft.com/pricing/details/app-service/) de Azure App Service que proporciona un entorno plenamente aislado y dedicado para ejecutar de forma segura las aplicaciones de Azure App Service. Para más información acerca de ASE, consulte [Documentación de App Service Environment](../app-service/environment/intro.md).</br></br>Puede proteger varias aplicaciones web del Centro de seguridad si agrega estas aplicaciones a las implementaciones de WAF existentes. |
| [Finalización de la protección de la aplicación](security-center-add-web-application-firewall.md#finalize-application-protection) |Para completar la configuración de un WAF, el tráfico se debe redirigir a la aplicación del WAF. Si se sigue esta recomendación, se completarán los cambios necesarios en la configuración. |
| Uso de la última versión compatible de Node.js para aplicaciones web | Se recomienda que utilice la versión más reciente de Node.js para las clases de seguridad más recientes. El uso de tipos y clases anteriores puede hacer que la aplicación quede expuesta. |
| CORS no debe permitir que todos los recursos accedan a Function App | Se recomienda que solo permita que los dominios requeridos interactúen con la aplicación web. El uso compartido de recursos entre orígenes (CORS) no debe permitir que todos los dominios accedan a la aplicación de función. |
| Uso de dominios personalizados para Function App | Se recomienda el uso de dominios personalizados para proteger una aplicación de función frente a ataques comunes, como la suplantación de identidad y otros ataques relacionados con el DNS. |
| Configuración de las restricciones de IP para Function App | Se recomienda definir una lista de direcciones IP que tienen permiso para acceder a su aplicación. El uso de restricciones de IP protege una aplicación de función frente a ataques comunes. |
| Acceso a Function App solo a través de HTTPS | Se recomienda limitar el acceso de instancias de Function App a través de HTTPS solamente. |
| Recomendación de desactivación de la depuración remota para Function App | Se recomienda que desactive la depuración para Function App si ya no necesita usarla. La depuración remota requiere puertos de entrada que se abran en una instancia de Function App. |
| Recomendación de desactivación de Web Sockets para Function App | Se recomienda que revise cuidadosamente el uso de Web Sockets en las instancias de Function App. El protocolo Web Sockets es vulnerable a distintos tipos de amenazas de seguridad. |
| Designar a más de un propietario en la suscripción | Se recomienda que designe a más de un propietario de la suscripción para tener redundancia de acceso de administrador. |
| Designar a un máximo de 3 propietarios en la suscripción | Se recomienda que designe a un máximo de 3 propietarios de suscripción para reducir el riesgo de vulneración por parte de un propietario en peligro. |
| Habilitar MFA para las cuentas con permisos de propietario en la suscripción | Se recomienda que habilite Multi-Factor Authentication (MFA) para todas las cuentas de suscripción con privilegios de administrador para evitar una vulneración de las cuentas o recursos. |
| Habilitar MFA para las cuentas con permisos de escritura en la suscripción | Se recomienda que habilite Multi-Factor Authentication (MFA) para todas las cuentas de suscripción con privilegios de escritura para evitar una vulneración de las cuentas o recursos. |
| Habilitar MFA para las cuentas con permisos de lectura en la suscripción | Se recomienda que habilite Multi-Factor Authentication (MFA) para todas las cuentas de suscripción con privilegios de lectura para evitar una vulneración de las cuentas o recursos. |
| Quitar las cuentas externas con permisos de lectura de la suscripción | Se recomienda eliminar las cuentas externas con privilegios de lectura de la suscripción a fin de evitar el acceso no supervisado. |
| Quitar las cuentas externas con permisos de escritura de la suscripción | Se recomienda eliminar las cuentas externas con privilegios de escritura de la suscripción a fin de evitar el acceso no supervisado. |
| Quitar las cuentas externas con permisos de propietario de la suscripción | Se recomienda eliminar las cuentas externas con permisos de propietario de la suscripción a fin de evitar el acceso no supervisado. |
| Quitar las cuentas en desuso de la suscripción | Se recomienda quitar las cuentas en desuso de la suscripción. |
| Quitar de la suscripción las cuentas en desuso con permisos de propietario | Se recomienda quitar las cuentas en desuso con permisos de propietario de la suscripción. |

### <a name="apply-recommendations"></a>Aplicación de recomendaciones
Después de revisar todas las recomendaciones, decida cuáles son las primeras que debe aplicar. Se recomienda usar la clasificación de gravedad como parámetro principal para evaluar las recomendaciones que se deben aplicar primero.

En la tabla de las recomendaciones anteriores, seleccione una recomendación y tómela como ejemplo de aplicación de una recomendación.

## <a name="next-steps"></a>Pasos siguientes
En este documento, se han presentando las recomendaciones de seguridad del Centro de seguridad. Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md) : aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md) : obtenga más información sobre cómo supervisar el estado de sus recursos en Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md) : aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/) : encuentre entradas de blog sobre el cumplimiento y la seguridad de Azure.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
