---
title: Habilitar la compatibilidad con Avere vFXT - Azure
description: Cómo habilitar las cargas de soporte desde Avere vFXT for Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: fe096b2e2a75cc89e3ce5ef905d8e4c347cc153a
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992471"
---
# <a name="enable-support-uploads"></a>Habilitación de cargas de soporte

Avere vFXT for Azure puede cargar automáticamente datos de soporte sobre su clúster. Estas cargas permiten que el personal de soporte técnico proporcione el mejor servicio posible al cliente.

## <a name="steps-to-enable-uploads"></a>Pasos para habilitar cargas

Siga estos pasos desde el Panel de control de Avere para activar el soporte técnico. (Lea [Access the vFXT cluster](avere-vfxt-cluster-gui.md) (Obtener acceso al clúster de vFXT) para aprender a abrir el Panel de control de Avere.)

1. Vaya a la pestaña **Configuración** en la parte superior.
1. Haga clic en el vínculo **Soporte técnico** en la izquierda y acepte la directiva de privacidad.

   ![Captura de pantalla que muestra el Panel de Control de Avere y una ventana emergente con el botón Confirmar para aceptar la directiva de privacidad](media/avere-vfxt-privacy-policy.png)

1. Haga clic en el triángulo situado a la izquierda de **Información del cliente** para expandir la sección.
1. Haga clic en el botón **Revalidate upload information** (Revalidar información de carga).
1. Establezca el nombre de soporte del clúster en **Nombre de clúster único**, asegúrese de que identifica de manera única a su clúster para el personal de apoyo.
1. Active las casillas de **Statistics Monitoring** (Supervisión de estadísticas), **General Information Upload** (Carga de información general) y **Crash Information Upload** (Carga de información de bloqueo).
1. Haga clic en **Enviar**.

   ![Captura de pantalla que contiene la sección de información del cliente completada en la página de configuración de soporte técnico](media/avere-vfxt-support-info.png)

1. Haga clic en el triángulo situado a la izquierda de **Secure Proactive Support (SPS)** (Soporte técnico proactivo seguro (SPS)) para expandir la sección.
1. Active la casilla **Enable SPS Link** (Habilitar vínculo SPS).
1. Haga clic en **Enviar**.

   ![Captura de pantalla que contiene la sección de soporte técnico proactivo completada en la página de configuración de soporte técnico](media/avere-vfxt-support-sps.png)

## <a name="next-steps"></a>Pasos siguientes

Si necesita agregar una local o existentes en la nube de sistema de almacenamiento al clúster, siga las instrucciones de [configurar almacenamiento](avere-vfxt-add-storage.md). 

Si está listo para empezar a adjuntar clientes al clúster, lea [Mount the Avere vFXT cluster](avere-vfxt-mount-clients.md) (Montar el clúster de Avere vFXT).