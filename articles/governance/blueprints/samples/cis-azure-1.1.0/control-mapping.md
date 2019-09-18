---
title: 'Ejemplo de plano técnico de CIS Microsoft Azure Foundations Benchmark: asignación de recomendaciones'
description: Asignación de recomendaciones del ejemplo de plano técnico de CIS Microsoft Azure Foundations Benchmark para Azure Policy.
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/09/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: f048262003a3567175c40ebf4ee744c41e11b5f9
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70918707"
---
# <a name="recommendation-mapping-of-the-cis-microsoft-azure-foundations-benchmark-blueprint-sample"></a>Asignación de recomendaciones del ejemplo de plano técnico de CIS Microsoft Azure Foundations Benchmark

En el siguiente artículo se detalla el modo en que el ejemplo de plano técnico de CIS Microsoft Azure Foundations Benchmark para Azure Blueprints sigue las recomendaciones de CIS Microsoft Azure Foundations Benchmark. Para más información sobre las recomendaciones, consulte [Banco de pruebas de fundamentos CIS para Microsoft Azure](https://www.cisecurity.org/benchmark/azure/).

Las siguientes asignaciones son para las recomendaciones de **CIS Microsoft Azure Foundations Benchmark v1.1.0**. Use el panel de navegación de la derecha para ir directamente a una asignación de recomendación específica.
Muchas de las recomendaciones asignadas se implementan con una iniciativa de [Azure Policy](../../../policy/overview.md). Para revisar la iniciativa completa, abra **Policy** en Azure Portal y seleccione la página **Definiciones**. Después, busque y seleccione la iniciativa de directiva integrada con las recomendaciones de la **\[Versión preliminar\] Audit CIS Microsoft Azure Foundations Benchmark v1.1.0 e implemente extensiones de VM específicas para admitir los requisitos de auditoría**.

> [!IMPORTANT]
> Cada control siguiente está asociado a una o varias definiciones de [Azure Policy](../../../policy/overview.md). Estas directivas pueden ayudarle a [evaluar el cumplimiento](../../../policy/how-to/get-compliance-data.md) del control; sin embargo, a menudo no hay una coincidencia 1:1 o completa entre un control y una o varias directivas. Como tal, el **cumplimiento** con Azure Policy solo se refiere a las propias directivas; esto no garantiza que sea totalmente compatible con todos los requisitos de un control. Además, el estándar de cumplimiento incluye controles que no se abordan con las definiciones de Azure Policy en este momento. Por lo tanto, el cumplimiento en Azure Policy es solo una vista parcial del estado general de cumplimiento. Las asociaciones entre los controles y las definiciones de Azure Policy para este ejemplo de plano técnico de cumplimiento pueden cambiar con el tiempo. Para ver el historial de cambios, consulte el [historial de confirmación de GitHub](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/cis-azure-1.1.0/control-mapping.md).

## <a name="11-ensure-that-multi-factor-authentication-is-enabled-for-all-privileged-users"></a>1.1 Asegúrese de que la autenticación multifactor esté habilitada para todos los usuarios con privilegios

Este plano técnico asigna las definiciones de [Azure Policy](../../../policy/overview.md) que se alinean con esta recomendación de CIS.

- MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción
- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción

## <a name="12-ensure-that-multi-factor-authentication-is-enabled-for-all-non-privileged-users"></a>1.2 Asegúrese de que la autenticación multifactor esté habilitada para todos los usuarios sin privilegios

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- MFA debe estar habilitada en las cuentas con permisos de lectura en la suscripción

## <a name="13-ensure-that-there-are-no-guest-users"></a>1.3 Asegúrese de que no hay ningún usuario invitado

Este plano técnico asigna las definiciones de [Azure Policy](../../../policy/overview.md) que se alinean con esta recomendación de CIS.

- Las cuentas externas con permisos de propietario deben quitarse de la suscripción
- Las cuentas externas con permisos de lectura deben quitarse de la suscripción
- Las cuentas externas con permisos de escritura deben quitarse de la suscripción

## <a name="23-ensure-asc-default-policy-setting-monitor-system-updates-is-not-disabled"></a>2.3 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar las actualizaciones del sistema" no es "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Se deben instalar actualizaciones del sistema en las máquinas

## <a name="24-ensure-asc-default-policy-setting-monitor-os-vulnerabilities-is-not-disabled"></a>2.4 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar las vulnerabilidades del sistema operativo" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Se deben corregir las vulnerabilidades en la configuración de seguridad en las máquinas

## <a name="25-ensure-asc-default-policy-setting-monitor-endpoint-protection-is-not-disabled"></a>2.5 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar Endpoint Protection" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Supervisión de la falta de Endpoint Protection en Azure Security Center

## <a name="26-ensure-asc-default-policy-setting-monitor-disk-encryption-is-not-disabled"></a>2.6 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el cifrado de disco" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- El cifrado de discos debe aplicarse en máquinas virtuales

## <a name="28-ensure-asc-default-policy-setting-monitor-web-application-firewall-is-not-disabled"></a>2.8 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el firewall de aplicaciones web" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Se deben proteger las reglas de NSG para las aplicaciones web en IaaS

## <a name="210-ensure-asc-default-policy-setting-monitor-vulnerability-assessment-is-not-disabled"></a>2.10 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar la evaluación de vulnerabilidades" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades

## <a name="212-ensure-asc-default-policy-setting-monitor-jit-network-access-is-not-disabled"></a>2.12 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el acceso de red JIT" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- El control de acceso de red Just-In-Time se debe aplicar en las máquinas virtuales.

## <a name="215-ensure-asc-default-policy-setting-monitor-sql-encryption-is-not-disabled"></a>2.15 Asegúrese de que la configuración de la directiva predeterminada de ASC "Supervisar el cifrado SQL" no sea "Deshabilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Se debe permitir el cifrado de datos transparente en bases de datos SQL

## <a name="31-ensure-that-secure-transfer-required-is-set-to-enabled"></a>3.1 Asegúrese de la opción "Se requiere transferencia segura" esté establecida en "Habilitada".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Se debe habilitar la transferencia segura a las cuentas de almacenamiento

## <a name="37-ensure-default-network-access-rule-for-storage-accounts-is-set-to-deny"></a>3.7 Asegúrese de que la regla de acceso de red predeterminada para las cuentas de almacenamiento esté configurada para denegar

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Auditar el acceso de red sin restricciones a cuentas de almacenamiento

## <a name="41-ensure-that-auditing-is-set-to-on"></a>4.1 Asegúrese de que la opción "Auditando" esté establecida en "Activada"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- La configuración de seguridad avanzada de datos debe estar habilitada en SQL Server

## <a name="42-ensure-that-auditactiongroups-in-auditing-policy-for-a-sql-server-is-set-properly"></a>4.2 Asegúrese de que la opción "AuditActionGroups" de la directiva de auditoría para un servidor SQL Server esté configurada correctamente

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- La opción de configuración Auditoría de SQL debería tener configurado Action-Groups para capturar las actividades críticas

## <a name="43-ensure-that-auditing-retention-is-greater-than-90-days"></a>4.3 Asegúrese de que la retención de "Auditoría" sea "más de 90 días"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Debe configurar los servidores SQL Server para que realicen una retención de la auditoría durante más de 90 días.

## <a name="44-ensure-that-advanced-data-security-on-a-sql-server-is-set-to-on"></a>4.4 Asegúrese de que "Advanced Data Security" en un servidor SQL Server esté establecido en "Activado".

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- La seguridad avanzada de datos debe estar habilitada en los servidores SQL Server

## <a name="45-ensure-that-threat-detection-types-is-set-to-all"></a>4.5 Asegúrese de que "Tipos de detección de amenazas" esté establecido en "Todo"

Este plano técnico asigna las definiciones de [Azure Policy](../../../policy/overview.md) que se alinean con esta recomendación de CIS.

- Los tipos de Advanced Threat Protection deben estar establecidos en "Todos" en la configuración de Advanced Data Security de SQL Server
- Los tipos de Advanced Threat Protection deben estar establecidos en "Todos" en la configuración de Advanced Data Security de la instancia administrada de SQL Server

## <a name="46-ensure-that-send-alerts-to-is-set"></a>4.6 Asegúrese de que "Enviar alertas a" esté establecido

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- La configuración de seguridad de datos avanzada para SQL Server debe contener una dirección de correo electrónico para recibir alertas de seguridad

## <a name="47-ensure-that-email-service-and-co-administrators-is-enabled"></a>4.7 Asegúrese de que la configuración de "Servicio de correo electrónico y coadministradores" sea "Habilitado"

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- La configuración de seguridad de datos avanzada para una instancia administrada SQL debe contener una dirección de correo electrónico para recibir alertas de seguridad

## <a name="48-ensure-that-azure-active-directory-admin-is-configured"></a>4.8 Asegúrese de que el administrador de Azure Active Directory esté configurado

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- El administrador de Azure Active Directory debe aprovisionarse para servidores SQL Server

## <a name="49-ensure-that-data-encryption-is-set-to-on-on-a-sql-database"></a>4.9 Asegúrese de que la opción "Cifrado de datos" esté establecida en "Activado" en una base de datos SQL Database

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Se debe permitir el cifrado de datos transparente en bases de datos SQL

## <a name="410-ensure-sql-servers-tde-protector-is-encrypted-with-byok-use-your-own-key"></a>4.10 Asegúrese de que el protector de TDE de SQL Server esté cifrado con BYOK (use su propia clave)

Este plano técnico asigna las definiciones de [Azure Policy](../../../policy/overview.md) que se alinean con esta recomendación de CIS.

- El protector de TDE de SQL Server debe estar cifrado con su propia clave
- El protector de TDE de la instancia administrada SQL debe estar cifrado con su propia clave

## <a name="517-ensure-that-logging-for-azure-keyvault-is-enabled"></a>5.1.7 Asegúrese de que el registro de Azure KeyVault esté habilitado.

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Los registros de diagnóstico en Key Vault deben estar habilitados

## <a name="71-ensure-that-os-disk-are-encrypted"></a>7.1 Asegúrese de que "Disco del SO" esté cifrado

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- El cifrado de discos debe aplicarse en máquinas virtuales

## <a name="72-ensure-that-data-disks-are-encrypted"></a>7.2 Asegúrese de que la opción "Discos de datos" esté cifrada

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- El cifrado de discos debe aplicarse en máquinas virtuales

## <a name="75-ensure-that-the-latest-os-patches-for-all-virtual-machines-are-applied"></a>7.5 Asegúrese de que se aplican las revisiones del sistema operativo más recientes para todas las máquinas virtuales

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Se deben instalar actualizaciones del sistema en las máquinas

## <a name="76-ensure-that-the-endpoint-protection-for-all-virtual-machines-is-installed"></a>7.6 Asegúrese de que Endpoint Protection esté instalado para todas las máquinas virtuales

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Supervisión de la falta de Endpoint Protection en Azure Security Center

## <a name="85-enable-role-based-access-control-rbac-within-azure-kubernetes-services"></a>8.5 Habilite el control de acceso basado en rol (RBAC) en Azure Kubernetes Services

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- \[Versión preliminar\]: En Kubernetes Services se debe usar el control de acceso basado en rol (RBAC)

## <a name="92-ensure-web-app-redirects-all-http-traffic-to-https-in-azure-app-service"></a>9.2 Asegúrese de que la aplicación web redirige todo el tráfico HTTP a HTTPS en Azure App Service

Este plano técnico asigna una definición de [Azure Policy](../../../policy/overview.md) que se alinea con esta recomendación de CIS.

- Acceso a la aplicación web solo a través de HTTPS

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha revisado la asignación de control del plano técnico de CIS Microsoft Azure Foundation Benchmark, visite el siguiente artículo para obtener información sobre el plano técnico o visite Azure Policy en Azure Portal para asignar la iniciativa:

> [!div class="nextstepaction"]
> [Información general del plano técnico de CIS Microsoft Azure Foundations Benchmark](./index.md)
> [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyMenuBlade/Definitions)

Artículos adicionales sobre planos técnicos y cómo utilizarlos:

- Información acerca del [ciclo de vida del plano técnico](../../concepts/lifecycle.md).
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).