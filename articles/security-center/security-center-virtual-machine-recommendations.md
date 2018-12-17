---
title: Recomendaciones sobre máquinas virtuales en Azure Security Center | Microsoft Docs
description: En este documento se describen las recomendaciones de Azure Security Center que le ayudan a proteger las máquinas virtuales, equipos, aplicaciones web y entornos de App Service.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: f5ce7f62-7b12-4bc0-b418-4a2f9ec49ca1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2018
ms.author: rkarlin
ms.openlocfilehash: dd7858b9723d8d3ee9eb9299b2cee11596563a33
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53340715"
---
# <a name="understand-azure-security-center-resource-recommendations"></a>Descripción de las recomendaciones sobre recursos de Azure Security Center


## <a name="recommendations"></a>Recomendaciones
Use las tablas siguientes para entender mejor las recomendaciones disponibles sobre Compute Service y App Service y lo que harán cada una de ellas si se aplican.

### <a name="computers"></a>Equipos
| Recomendación | DESCRIPCIÓN |
| --- | --- |
| [Habilitar la colección de datos de las suscripciones](security-center-enable-data-collection.md) |Recomienda activar la recopilación de datos en la directiva de seguridad para cada una de las suscripciones y para todas las máquinas virtuales de la suscripción. |
| [Habilitar el cifrado para la cuenta de Azure Storage](security-center-enable-encryption-for-storage-account.md) | Es recomendable que habilite el cifrado del servicio de Azure Storage para datos en reposo. El Cifrado de servicio de almacenamiento (SSE) funciona mediante el cifrado de los datos cuando se escriben en Azure Storage y el descifrado antes de la recuperación. SSE actualmente solo está disponible para Azure Blob service y puede usarse para blobs en bloques, blobs de página y blobs en anexos. Para obtener más información, consulte [Cifrado del servicio Azure Storage para datos en reposo (versión preliminar)](../storage/common/storage-service-encryption.md).</br>SSE solo es compatible con las cuentas de almacenamiento de Resource Manager. Las cuentas de almacenamiento clásico no se admiten en este momento. Para entender los modelos de implementación clásico y de Resource Manager, consulte [Modelos de implementación de Azure](../azure-classic-rm.md). |
| [Corregir configuraciones de seguridad](security-center-remediate-os-vulnerabilities.md) |Recomienda armonizar las configuraciones del SO con las reglas de configuración recomendadas; por ejemplo, no permitir guardar las contraseñas. |
| [Aplicar actualizaciones del sistema](security-center-apply-system-updates.md) |Recomienda implementar las actualizaciones críticas y de seguridad del sistema en las máquinas virtuales. |
| [Aplicación del control de acceso a redes Just-In-Time](security-center-just-in-time.md) | Se recomienda que aplique acceso a la máquina virtual Just-In-Time. La característica Just-In-Time se encuentra en versión preliminar y está disponible en el nivel estándar de Security Center. Para más información sobre los planes de tarifa de Security Center, consulte [Precios](security-center-pricing.md). |
| [Reiniciar tras actualizar el sistema](security-center-apply-system-updates.md#reboot-after-system-updates) |Se recomienda que reinicie una máquina virtual para completar el proceso de aplicación de actualizaciones del sistema. |
| [Instalación de Endpoint Protection](security-center-install-endpoint-protection.md) |Recomienda aprovisionar programas antimalware a las máquinas virtuales (solo máquinas virtuales de Windows). |
| [Habilitar el Agente de máquina virtual](security-center-enable-vm-agent.md) |Permite ver las VM que requieren el Agente de VM. El agente de máquina virtual debe estar instalado en las máquinas virtuales para aprovisionar la detección de revisiones, la detección de línea de base y los programas antimalware. De manera predeterminada, el agente de máquina virtual está instalado en las máquinas virtuales que se implementan desde Azure Marketplace. El artículo [VM Agent and Extensions – Part 2](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) (Agente de VM y extensiones, parte 2) proporciona información sobre cómo instalar el Agente de VM. |
| [Aplicar cifrado de discos](security-center-apply-disk-encryption.md) |Se recomienda cifrar los discos de la máquina virtual mediante Azure Disk Encryption (máquinas virtuales Linux y Windows). Se recomienda cifrar tanto los volúmenes de datos como los del sistema operativo en la máquina virtual. |
| [Actualizar versión del sistema operativo](security-center-update-os-version.md) |Recomienda actualizar la versión del sistema operativo para el servicio en la nube a la versión más reciente disponible para la familia del sistema operativo.  Para obtener más información sobre Cloud Services, consulte la [información general sobre Cloud Services](../cloud-services/cloud-services-choose-me.md). |
| [Evaluación de vulnerabilidades no instalada](security-center-vulnerability-assessment-recommendations.md) |Se recomienda instalar una solución de evaluación de vulnerabilidades en la máquina virtual. |
| [Corrección de vulnerabilidades](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Permite ver las vulnerabilidades tanto del sistema como de las aplicaciones que ha detectado la solución de evaluación de vulnerabilidades instalada en la máquina virtual. |

### <a name="app-services"></a>App Services
| Recomendación | DESCRIPCIÓN |
| --- | --- |
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


## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre las recomendaciones que se aplican a otros tipos de recursos de Azure, consulte los siguientes artículos:

* [Supervisión de identidad y acceso en Azure Security Center](security-center-identity-access.md)
* [Protección de las redes en Azure Security Center](security-center-network-recommendations.md)
* [Protección del servicio SQL de Azure en Azure Security Center](security-center-sql-service-recommendations.md)

Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Protección de las aplicaciones y las máquinas en Azure Security Center](security-center-virtual-machine-protection.md)
* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : encuentre las preguntas más frecuentes sobre el uso del servicio.

<!--Image references-->
[1]: ./media/security-center-virtual-machine-recommendations/overview.png
[2]: ./media/security-center-virtual-machine-recommendations/compute.png
[3]: ./media/security-center-virtual-machine-recommendations/monitoring-agent-health-issues.png
[4]: ./media/security-center-virtual-machine-recommendations/compute-recommendations.png
[5]: ./media/security-center-virtual-machine-recommendations/apply-system-updates.png
[6]: ./media/security-center-virtual-machine-recommendations/missing-update-details.png
[7]: ./media/security-center-virtual-machine-recommendations/vm-computers.png
[8]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon1.png
[9]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon2.png
[10]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon3.png
[11]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-icon4.png
[12]: ./media/security-center-virtual-machine-recommendations/filter.png
[13]: ./media/security-center-virtual-machine-recommendations/vm-detail.png
[14]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig1-new006-2017.png
[15]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new3.png
[16]: ./media/security-center-virtual-machine-recommendations/security-center-monitoring-fig8-new4.png
[17]: ./media/security-center-virtual-machine-recommendations/app-services.png
[18]: ./media/security-center-virtual-machine-recommendations/ase.png
[19]: ./media/security-center-virtual-machine-recommendations/web-app.png
[20]: ./media/security-center-virtual-machine-recommendations/recommendation.png
[21]: ./media/security-center-virtual-machine-recommendations/recommendation-desc.png
[22]: ./media/security-center-virtual-machine-recommendations/passed-assessment.png
[23]: ./media/security-center-virtual-machine-recommendations/healthy-resources.png
[24]: ./media/security-center-virtual-machine-recommendations/function-app.png
