---
title: Administrar problemas de Endpoint Protection con Azure Security Center | Microsoft Docs
description: Obtenga información sobre la supervisión de Security Center de la protección de puntos de conexión y cómo corregir las incidencias que surjan.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 1599ad5f-d810-421d-aafc-892e831b403f
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2019
ms.author: memildin
ms.openlocfilehash: e1ed403babe66b465fb1800dc8c5a90c7a8f1a08
ms.sourcegitcommit: 0cc25b792ad6ec7a056ac3470f377edad804997a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77604117"
---
# <a name="manage-endpoint-protection-issues-with-azure-security-center"></a>Administrar problemas de Endpoint Protection con Azure Security Center
Azure Security Center supervisa el estado de la protección antimalware y lo notifica en la página de incidencias de Endpoint Protection. Security Center resalta problemas, como las amenazas detectadas y si hay una protección insuficiente, que pueden hacer vulnerables sus equipos y máquinas virtuales frente a amenazas antimalware. Si aplica la información descrita en **Problemas de Endpoint Protection**, puede identificar un plan para solucionar cualquier problema detectado.

Security Center notifica los siguientes problemas de Endpoint Protection:

- Endpoint Protection no está instalado en las máquinas virtuales de Azure: No hay ninguna solución antimalware compatible instalada en estas máquinas virtuales de Azure.
- Endpoint protection not installed on non-Azure computers (Endpoint Protection no instalado en los equipos que no son de Azure): No hay ningún antimalware compatible instalado en estos equipos que no son de Azure.
- Estado de Endpoint Protection:

  - Firma sin actualizar: Hay una solución antimalware instalada en estos equipos y máquinas virtuales, pero no cuenta con las firmas de antimalware más recientes.
  - Sin protección en tiempo real: Hay una solución antimalware instalada en estos equipos y máquinas virtuales, pero no está configurada para la protección en tiempo real. El servicio podría deshabilitarse o es posible que Security Center no pueda obtener el estado, ya que la solución no es compatible. Vea [Integración de soluciones de asociados](security-center-services.md#endpoint-supported) para ver una lista de las soluciones compatibles.
  - Sin generación de informes: Hay una solución antimalware instalada, pero no notifica datos.
  - Desconocido: Hay una solución antimalware instalada, pero su estado es desconocido o informa de un error desconocido.

    > [!NOTE]
    > Vea [Integración de soluciones de seguridad en Azure Security Center](security-center-services.md#endpoint-supported) para obtener una lista de soluciones de seguridad de protección de punto de conexión integradas con Security Center.
    >
    >

## <a name="implement-the-recommendation"></a>Implementación de la recomendación
Los problemas de Endpoint Protection se presentan como una recomendación en Security Center. Si su entorno es vulnerable a las amenazas antimalware, esta recomendación se mostrará en **Recomendaciones** y en **Proceso**. Para ver el panel de **problemas de Endpoint Protection**, debe seguir el flujo de trabajo de Proceso.

En este ejemplo usaremos **Proceso**.  Veremos cómo instalar un antimalware en máquinas virtuales de Azure y en equipos que no son de Azure.

## <a name="install-antimalware-on-azure-vms"></a>Instalar un antimalware en máquinas virtuales de Azure

1. Seleccione **Compute & apps** (Proceso y aplicaciones) en el menú principal de Security Center o en **Información general**.

   ![Seleccionar Proceso][1]

2. En **Proceso**, seleccione **Endpoint protection issues** (Problemas de Endpoint Protection). Se abrirá el panel **Endpoint protection issues** (Problemas de Endpoint Protection).

   ![Seleccionar Endpoint protection issues (Problemas de Endpoint Protection)][2]

   En la parte superior del panel se muestra lo siguiente:

   - Proveedores de Endpoint Protection instalados: Se muestran los distintos proveedores identificados por Security Center.
   - Installed endpoint protection health state (Estado de mantenimiento de Endpoint Protection instalado): Se muestra el estado de mantenimiento de las máquinas virtuales y equipos que tienen instalada una solución de Endpoint Protection. En el gráfico se muestra el número de máquinas virtuales y de equipos que están en buen estado y los que tienen una protección insuficiente.
   - Malware detectado: Se muestra el número de máquinas virtuales y de equipos en los que Security Center ha notificado que se ha detectado un malware.
   - Equipos atacados: se muestra el número de máquinas virtuales y de equipos en los que Security Center ha notificado ataques de malware.

   En la parte inferior del panel hay una lista de problemas de Endpoint Protection que incluye la información siguiente:  

   - **TOTAL**: Número de máquinas virtuales y de equipos afectados por el problema.
   - Una barra en la que se agrega el número de máquinas virtuales y de equipos afectados por el problema. Los colores de la barra identifican la prioridad:

      - Rojo: Alta prioridad; se debe solucionar de inmediato.
      - Naranja: Prioridad media; se debe solucionar lo antes posible.

3. Seleccione **Endpoint protection not installed on Azure VMs** (Endpoint Protection no instalado en las máquinas virtuales de Azure).

   ![Seleccionar Endpoint protection not installed on Azure VMs (Endpoint Protection no instalado en las máquinas virtuales de Azure)][3]

4. En **Endpoint protection not installed on Azure VMs** (Endpoint Protection no instalado en las máquinas virtuales de Azure) hay una lista de máquinas virtuales de Azure que no tienen ningún antimalware instalado.  Puede optar por instalar un antimalware en todas las máquinas virtuales de la lista o seleccionar máquinas virtuales concretas para instalar el antimalware haciendo clic en la máquina virtual en cuestión.
5. En **Seleccionar Endpoint Protection**, seleccione la solución de Endpoint Protection que quiere usar. En este ejemplo, seleccione **Microsoft Antimalware**.
6. Aparece información adicional sobre la solución Endpoint Protection. Seleccione **Crear**.

## <a name="install-antimalware-on-non-azure-computers"></a>Instalar un antimalware en equipos que no son de Azure

1. Vuelva a **Endpoint protection issues** (Problemas de Endpoint Protection) y seleccione **Endpoint protection not installed on non-Azure computers** (Endpoint Protection no instalado en los equipos que no son de Azure).

   ![Seleccionar Endpoint protection not installed on non-Azure computers (Endpoint Protection no instalado en los equipos que no son de Azure)][4]

2. En **Endpoint protection not installed on non-Azure computers** (Endpoint Protection no instalado en los equipos que no son de Azure), seleccione un área de trabajo. Se abre una consulta de búsqueda de registros de Azure Monitor filtrada por el área de trabajo, donde se muestran los equipos a los que les falta antimalware. Seleccione un equipo de la lista para más información.

   ![Búsqueda de registros de Azure Monitor][5]

Se abre otro resultado de la búsqueda con la información filtrada solo para ese equipo.

  ![Búsqueda de registros de Azure Monitor][6]

> [!NOTE]
> Se recomienda aprovisionar Endpoint Protection para todas las máquinas virtuales y equipos a fin de identificar y quitar virus, spyware y otro software malintencionado.
>
>

## <a name="next-steps"></a>Pasos siguientes
En este documento, mostramos cómo implementar la recomendación "Instalar Endpoint Protection" de Security Center. Para obtener más información sobre cómo habilitar Microsoft Antimalware en Azure, consulte el siguiente documento:

* [Microsoft Antimalware para Azure Cloud Services y Virtual Machines](../security/fundamentals/antimalware.md): aprenda a implementar Microsoft Antimalware.

Para obtener más información sobre Security Center, consulte los siguientes documentos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md) : aprenda a configurar directivas de seguridad.
* [Administración de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md): recomendaciones que le ayudan a proteger los recursos de Azure.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md) : obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.

<!--Image references-->
[1]:./media/security-center-install-endpoint-protection/compute.png
[2]:./media/security-center-install-endpoint-protection/endpoint-protection-issues.png
[3]:./media/security-center-install-endpoint-protection/install-endpoint-protection.png
[4]:./media/security-center-install-endpoint-protection/endpoint-protection-issues-computers.png
[5]:./media/security-center-install-endpoint-protection/log-search.png
[6]:./media/security-center-install-endpoint-protection/info-filtered-to-computer.png
