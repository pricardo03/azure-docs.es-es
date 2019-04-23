---
title: Vista de los registros de Azure Monitor para contenedores en tiempo real | Microsoft Docs
description: En este artículo se describe la vista en tiempo real de los registros de contenedor (stdout/stderr) sin usar kubectl con Azure Monitor para contenedores.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/09/2019
ms.author: magoedte
ms.openlocfilehash: c8baa4d2355adf99ce188d632ac50901db29a758
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997706"
---
# <a name="how-to-view-container-logs-real-time-with-azure-monitor-for-containers-preview"></a>Vista de los registros de contenedor en tiempo real con Azure Monitor para contenedores (versión preliminar)
Esta característica, que se encuentra actualmente en versión preliminar, proporciona una vista en tiempo real en los registros de contenedor de Azure Kubernetes Service (AKS) (stdout/stderr) sin tener que ejecutar los comandos de kubectl. Cuando se selecciona esta opción, el nuevo panel aparece debajo de la tabla de datos de rendimiento de contenedores en la vista **Contenedores**.  Muestra el registro dinámico generado por el motor de contenedores para ayudar a solucionar los problemas en tiempo real. **Colaborador** se requiere acceso al recurso de clúster para esta característica funcione.

Los registros dinámicos admiten tres métodos diferentes para controlar el acceso a los registros:

1. AKS sin autorización RBAC de Kubernetes habilitada 
2. AKS habilitado con autorización de RBAC de Kubernetes
3. AKS habilitado con inicio de sesión único basado en SAML de Azure Active Directory (AD) 

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Clúster de Kubernetes sin RBAC habilitado
 
Si tiene un clúster de Kubernetes que no está configurado con autorización de RBAC de Kubernetes ni integrado en el inicio de sesión único de Azure AD, no es necesario seguir estos pasos. Dado que la autorización de Kubernetes usa la api de kube, se requieren permisos de solo lectura.

## <a name="kubernetes-rbac-authorization"></a>Autorización de RBAC de Kubernetes
Si ha habilitado la autorización de RBAC de Kubernetes, deberá aplicar el enlace de rol de clúster. Los pasos de ejemplo siguientes muestran cómo configurar el enlace de rol de clúster desde esta plantilla de configuración de yaml.   

1. Copie el archivo yaml, péguelo y guárdelo como LogReaderRBAC.yaml.  

   ```
   apiVersion: rbac.authorization.k8s.io/v1 
   kind: ClusterRole 
   metadata: 
      name: containerHealth-log-reader 
   rules: 
      - apiGroups: [""] 
        resources: ["pods/log"] 
        verbs: ["get"] 
   --- 
   apiVersion: rbac.authorization.k8s.io/v1 
   kind: ClusterRoleBinding 
   metadata: 
      name: containerHealth-read-logs-global 
   roleRef: 
       kind: ClusterRole 
       name: containerHealth-log-reader 
       apiGroup: rbac.authorization.k8s.io 
   subjects: 
      - kind: User 
        name: clusterUser 
        apiGroup: rbac.authorization.k8s.io 
   ```

2. Cree el enlace de regla de clúster mediante la ejecución del siguiente comando: `kubectl create -f LogReaderRBAC.yaml`. 

## <a name="configure-aks-with-azure-active-directory"></a>Configuración de AKS con Azure Active Directory
Es posible configurar AKS para que utilice Azure Active Directory (AD) para la autenticación de usuarios. Si va a configurar esto por primera vez, consulte [Integración de Azure Active Directory con Azure Kubernetes Service](../../aks/azure-ad-integration.md). Durante los pasos para crear la [aplicación cliente](../../aks/azure-ad-integration.md#create-client-application) y especificar el **URI de redirección**, deberá agregar otro URI a la lista `https://ininprodeusuxbase.microsoft.com/*`.  

>[!NOTE]
>La configuración de la autenticación con Azure Active Directory para el inicio de sesión único solo puede lograrse durante la implementación inicial de un nuevo clúster de AKS. No puede configurar el inicio de sesión único en un clúster de AKS ya implementado.  
> 

## <a name="view-live-logs"></a>Vista de registros dinámicos
Cuando esté viendo **Contenedores**, también puede **ver registros del contenedor** o **ver registros dinámicos de contenedor**.  Cuando seleccione **Ver registros dinámicos de contenedor**, el nuevo panel aparece debajo de la tabla de datos de rendimiento de contenedores, y muestra el registro dinámico generado por el motor de contenedores para ayudar a solucionar los problemas en tiempo real.  
1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 
2. Desde el menú **Microsoft Azure**, seleccione **Supervisar** y, luego, seleccione **Contenedores**.  
3. Seleccione un contenedor de la lista bajo la vista **Monitored containers** (Contenedores supervisados).  
4. Seleccione la vista **Contenedores** y, en el panel de propiedades para un contenedor seleccionado, aparece el vínculo **Ver registros dinámicos de contenedor**.  
5. Si el clúster de AKS se configura con inicio de sesión único mediante AAD, deberá autenticarse en el primer uso durante esa sesión del explorador. Seleccione la cuenta y complete la autenticación con Azure.  

Después de autenticarse correctamente, el panel de registros dinámicos aparecerá en la sección inferior del panel central. Si el indicador de estado de captura muestra una marca de verificación verde, que se encuentra en el extremo derecho del panel, significa que puede recuperar los datos.
    
  ![Datos recuperados del panel de registros dinámicos](./media/container-insights-live-logs/live-logs-pane-01.png)  

En la barra de búsqueda, puede filtrar por palabra clave para resaltar texto en el registro.   

  ![Ejemplo de filtro del panel de registros dinámicos](./media/container-insights-live-logs/live-logs-pane-filter-01.png)

Para suspender el desplazamiento automático y controlar el comportamiento del panel y permitirle desplazarse manualmente por la lectura de los nuevos datos de registro, haga clic en la objeto visual **Desplazar**.  Para volver a habilitar el desplazamiento automático, simplemente vuelva a hacer clic en la opción **Desplazar**.  También puede pausar la recuperación de datos de registro haciendo clic en la opción **Pausar** y, cuando esté listo para reanudar, simplemente haga clic en **Reproducir**.  

![Vista activa de pausa en el panel de registros dinámicos](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

## <a name="next-steps"></a>Pasos siguientes
- Para continuar aprendiendo a usar Azure Monitor y supervisar otros aspectos de su clúster de AKS, consulte [Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores](container-insights-analyze.md).
- Vista [ejemplos de consultas de registro](container-insights-log-search.md#search-logs-to-analyze-data) para ver consultas predefinidas y ejemplos para evaluar o personalizar para las alertas, visualizar o analizar los clústeres.
