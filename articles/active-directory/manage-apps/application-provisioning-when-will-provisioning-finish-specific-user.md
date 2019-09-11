---
title: Averiguar cuándo un usuario específico podrá acceder a una aplicación | Microsoft Docs
description: Cómo averiguar cuándo un usuario sumamente importante puede acceder a una aplicación que se ha configurado para el aprovisionamiento de usuarios con Azure AD
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c6ad7e305958131c4f544dfa2022e7471e9adac
ms.sourcegitcommit: aaa82f3797d548c324f375b5aad5d54cb03c7288
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70147103"
---
# <a name="check-the-status-of-user-provisioning"></a>Comprobación del estado de aprovisionamiento de usuarios

El servicio de aprovisionamiento de Azure AD ejecuta un ciclo de aprovisionamiento inicial en el sistema de origen y el sistema de destino, seguido de los ciclos periódicos incrementales. Al configurar el aprovisionamiento de una aplicación, puede comprobar el estado actual del servicio de aprovisionamiento y ver cuando un usuario podrá tener acceso a una aplicación.

## <a name="view-the-provisioning-progress-bar"></a>Ver la barra de progreso del aprovisionamiento

 En la página **Aprovisionamiento** para una aplicación, puede ver el estado del servicio de aprovisionamiento de Azure AD. En la sección **Estado actual** de la parte inferior de la página se muestra si un ciclo de aprovisionamiento ha iniciado las cuentas de usuario de aprovisionamiento. Puede ver el progreso del ciclo, cuántos usuarios y grupos se han aprovisionado y cuántos roles se crean.

La primera vez que configure el aprovisionamiento automático, en la sección **Estado actual** de la parte inferior de la página se muestra el estado del ciclo de aprovisionamiento inicial. Esta sección se actualiza cada vez que se ejecute un ciclo incremental. Se muestran los detalles siguientes:
- El tipo de ciclo de aprovisionamiento (inicial o incremental) que se ejecuta actualmente o que se completó por última vez.
- Una **barra de progreso** en la que se muestra el porcentaje del ciclo de aprovisionamiento que se ha completado. El porcentaje refleja el número de páginas aprovisionadas. Tenga en cuenta que cada página podría contener varios usuarios o grupos, por lo que el porcentaje no correlaciona directamente con el número de usuarios, grupos o roles aprovisionados.
- Un botón **Actualizar** que puede usar para mantener la vista actualizada.
- El número de **usuarios** y **grupos** aprovisionados, así como el número de roles que se crearon. Durante el ciclo inicial, el número de **usuarios** incrementa en 1 cuando se crea o actualiza un usuario y disminuye en 1 cuando se elimina un usuario. Durante un ciclo de incremental, las actualizaciones de usuario no afectan al número de **usuarios**; el número cambia solo cuando se crean o eliminan usuarios.
- Un vínculo **Ver registros de auditoría**, que abre los registros de auditoría de Azure AD con detalles sobre todas las operaciones que haya ejecutado el servicio de aprovisionamiento de usuarios, como el estado de aprovisionamiento de los usuarios individuales (consulte la sección [Uso de registros de auditoría](#use-audit-logs-to-check-a-users-provisioning-status) más adelante).

Una vez completado el ciclo de aprovisionamiento, en la sección **Estadísticas hasta la fecha** se muestran los números acumulativos de los usuarios y grupos que se hayan aprovisionado hasta la fecha, junto con la fecha de finalización y la duración del último ciclo. La opción **Id. de actividad** identifica de forma única el ciclo de aprovisionamiento más reciente. La opción **Id. de trabajo** es un identificador único para el trabajo de aprovisionamiento y es específico de la aplicación en el inquilino.

El progreso de aprovisionamiento se puede ver en Azure Portal, en la pestaña **Azure Active Directory &gt; Aplicaciones empresariales &gt; \[nombre de la aplicación\] &gt; Aprovisionamiento**.

![Barra de progreso de la página de aprovisionamiento](media/application-provisioning-when-will-provisioning-finish-specific-user/provisioning-progress-bar-section.png)

## <a name="use-audit-logs-to-check-a-users-provisioning-status"></a>Uso de registros de auditoría para comprobar el estado de aprovisionamiento de un usuario

Para ver el estado de aprovisionamiento de un usuario seleccionado, consulte los registros de auditoría de Azure AD. Todas las operaciones que ejecute el servicio de aprovisionamiento de usuarios se registran en los registros de auditoría de Azure AD. Esto incluye todas las operaciones de lectura y escritura realizadas en los sistemas de origen y de destino, así como los datos del usuario que se leyeron o escribieron durante cada operación.

Puede acceder a los registros de auditoría de aprovisionamiento en Azure Portal, en la pestaña **Azure Active Directory &gt; Aplicaciones empresariales &gt; \[nombre de la aplicación\] &gt; Registros de auditoría**. Filtre los registros en la categoría **Aprovisionamiento de cuentas** para ver solo los eventos de aprovisionamiento para esa aplicación. Puede buscar usuarios por el "identificador de coincidencia" que se configuró para ellos en las asignaciones de atributos. 

Por ejemplo, si configuró el "nombre principal de usuario" o la "dirección de correo electrónico" como atributo de coincidencia en el lado de Azure AD y el usuario que no se aprovisiona tiene el valor "audrey@contoso.com", busque "audrey@contoso.com" en los registros de auditoría y revise las entradas devueltas.

Los registros de auditoría de aprovisionamiento registran todas las operaciones realizadas por el servicio de aprovisionamiento, lo que incluye:

* Consultar en Azure AD los usuarios asignados que están en el ámbito de aprovisionamiento
* Consultar en la aplicación de destino la existencia de esos usuarios
* Comparar los objetos de usuario entre el sistema
* Agregar, actualizar o deshabilitar la cuenta de usuario en el sistema de destino en función de la comparación

Para más información sobre cómo leer los registros de auditoría en Azure Portal, consulte la [guía de informes de aprovisionamiento](check-status-user-account-provisioning.md).

## <a name="how-long-will-it-take-to-provision-users"></a>¿Cuánto tiempo se tarda en aprovisionar usuarios?
Cuando se usa el aprovisionamiento automático de usuarios con una aplicación, Azure AD aprovisiona y actualiza automáticamente las cuentas de usuario de una aplicación en función de aspectos tales como la [asignación de usuarios y grupos](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) con un intervalo programado periódicamente, normalmente cada 40 minutos.

El tiempo necesario para que un usuario determinado se aprovisione depende principalmente de si el trabajo de aprovisionamiento se ejecuta en una sincronización inicial o una sincronización incremental.

- Para las **sincronizaciones iniciales**, el tiempo de trabajo depende de muchos factores, entre los que se incluye el número de usuarios y grupos en el ámbito para realizar el aprovisionamiento, y el número total de usuarios y grupos en el sistema de origen. La sincronización inicial entre Azure AD y una aplicación puede tardar desde 20 minutos hasta varias horas, según el tamaño del directorio de Azure AD y el número de usuarios en el ámbito de aprovisionamiento. En esta sección se indica más adelante una lista completa de factores que afectan al rendimiento inicial de la sincronización.

- Para las **sincronizaciones incrementales** después de la sincronización inicial, los tiempos de trabajo suelen ser más rápidos (por ejemplo, unos 10 minutos), ya que el servicio de aprovisionamiento almacena marcas de agua que representan el estado de ambos sistemas tras la sincronización inicial, lo cual mejora el rendimiento de las posteriores. El tiempo del trabajo depende del número de cambios detectados en ese ciclo de aprovisionamiento. Si hay menos de 5000 cambios de usuario o pertenencia a un grupo, el trabajo puede finalizar dentro de un solo ciclo de sincronización incremental. 

En la siguiente tabla se resumen los tiempos de sincronización de escenarios comunes de aprovisionamiento. En estos escenarios, el sistema de origen es Azure AD y el sistema de destino es una aplicación SaaS. Los tiempos de sincronización se derivan de un análisis estadístico de trabajos de sincronización para las aplicaciones ServiceNow, Workplace, Salesforce y G Suite.


| Configuración de ámbito | Usuarios, grupos y miembros del ámbito | Hora de sincronización inicial | Hora de sincronización incremental |
| -------- | -------- | -------- | -------- |
| Sincronizar solo los usuarios y grupos asignados |  < 1000 |  < 30 minutos | < 30 minutos |
| Sincronizar solo los usuarios y grupos asignados |  1000 - 10.000 | 142 - 708 minutos | < 30 minutos |
| Sincronizar solo los usuarios y grupos asignados |   10.000 - 100.000 | 1170 - 2340 minutos | < 30 minutos |
| Sincronizar todos los usuarios y grupos en Azure AD |  < 1000 | < 30 minutos  | < 30 minutos |
| Sincronizar todos los usuarios y grupos en Azure AD |  1000 - 10.000 | < 30 - 120 minutos | < 30 minutos |
| Sincronizar todos los usuarios y grupos en Azure AD |  10.000 - 100.000  | 713 - 1425 minutos | < 30 minutos |
| Sincronizar todos usuarios en Azure AD|  < 1000  | < 30 minutos | < 30 minutos |
| Sincronizar todos usuarios en Azure AD | 1000 - 10.000  | 43 - 86 minutos | < 30 minutos |


Para la configuración dedicada a **sincronizar únicamente los usuarios y grupos asignados**, puede usar las siguientes fórmulas para determinar aproximadamente las veces que se realiza la **sincronización inicial** mínima y máxima esperada:

    Minimum minutes =  0.01 x [Number of assigned users, groups, and group members]
    Maximum minutes = 0.08 x [Number of assigned users, groups, and group members] 
    
Resumen de los factores que influyen en el tiempo que lleva completar una **sincronización inicial**:

- El número total de usuarios y grupos del ámbito para el aprovisionamiento.

- La cantidad total de usuarios, grupos y miembros del grupo presentes en el sistema de origen (Azure AD).

- Si los usuarios del ámbito de aprovisionamiento se corresponden o no con los usuarios existentes en la aplicación de destino, o si deben crearse por primera vez. Las tareas de sincronización para las cuales se crean todos los usuarios por primera vez llevan aproximadamente el *doble de tiempo* que los trabajos de sincronización para los cuales todos los usuarios se corresponden con usuarios existentes.

- Número de errores en los [registros de auditoría](check-status-user-account-provisioning.md). El rendimiento es también menor si hay muchos errores y si el servicio de aprovisionamiento ha quedado en un estado de "cuarentena".    

- Solicitar los límites de velocidad y otras limitaciones que haya implementado el sistema de destino. Algunos sistemas de destino implementan límites de velocidad y otras limitaciones en las solicitudes, que pueden afectar al rendimiento durante operaciones de sincronización grandes. En estos casos, una aplicación que recibe demasiadas solicitudes demasiado rápido puede ralentizar su velocidad de respuesta o cerrar la conexión. Para mejorar el rendimiento, el conector debe ajustarse; para ello, no debe enviar las solicitudes de la aplicación más rápido de lo que la aplicación puede procesarlas. Los conectores de aprovisionamiento que ha compilado Microsoft hacen este ajuste. 

- La cantidad y el tamaño de los grupos asignados. Sincronizar grupos asignados lleva más tiempo que sincronizar usuarios. Tanto el número como los tamaños de los grupos asignados afectan al rendimiento. Si una aplicación tiene [asignaciones habilitadas para la sincronización de objetos de grupo](customize-application-attributes.md#editing-group-attribute-mappings), las propiedades del grupo, como los nombres y la pertenencia, se sincronizan además de los usuarios. Estas sincronizaciones adicionales tomarán más tiempo que solo la sincronización de objetos de usuario.

## <a name="next-steps"></a>Pasos siguientes
[Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)
