---
title: Solución de problemas de la extensión del panel de acceso de Azure para Internet Explorer | Microsoft Docs
description: Cómo usar la directiva de grupo para implementar el complemento de Internet Explorer para el portal de Mis aplicaciones.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/11/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9ebd949460f826c9529b9f392bc4a7f4918ee170
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "44715047"
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Solución de problemas de la extensión del Panel de acceso para Internet Explorer
Este artículo le ayudará a solucionar los siguientes problemas:

* No puede tener acceso a sus aplicaciones a través del portal de Mis aplicaciones con Internet Explorer.
* Verá el mensaje "Instalar software" aunque ya haya instalado el software.

Si es administrador, vea también: [Cómo implementar la extensión del Panel de acceso para Internet Explorer con la Directiva de grupo](deploy-access-panel-browser-extension.md)

## <a name="run-the-diagnostic-tool"></a>Ejecutar la herramienta de diagnóstico
Puede diagnosticar problemas de instalación con la extensión del Panel de acceso descargando y ejecutando la herramienta de diagnóstico del Panel de acceso:

1. [Haga clic aquí para descargar la herramienta de diagnóstico.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Abra el archivo y presione el botón **Extraer todo** .
   
    ![Presionar Extraer todo](./media/manage-access-panel-browser-extension/extract1.png)
3. A continuación, presione el botón **Extraer** para continuar.
   
    ![Presionar Extraer](./media/manage-access-panel-browser-extension/extract2.png)
4. Para ejecutar la herramienta, haga clic con el botón derecho en el archivo denominado **AccessPanelExtensionDiagnosticTool** y, después, seleccione **Abrir con > Host de script basado en Microsoft Windows**.
   
    ![Abrir con > Host de script basado en Microsoft Windows](./media/manage-access-panel-browser-extension/open_tool.png)
5. A continuación verá la siguiente ventana de diagnóstico, en la que se describe qué puede haber incorrecto con la instalación.
   
    ![Un ejemplo de la ventana de diagnóstico](./media/manage-access-panel-browser-extension/tool_preview.png)
6. Haga clic en "**SÍ**" para permitir que el programa corrija los problemas que se han encontrado.
7. Para guardar estos cambios, cierre cada ventana de Internet Explorer y luego vuelva a abrir Internet Explorer.<br />Si todavía no puede tener acceso a sus aplicaciones, pruebe los pasos siguientes.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Comprobar que la extensión del Panel de acceso está activada
Para comprobar que la extensión del Panel de acceso está habilitada en Internet Explorer:

1. En Internet Explorer, haga clic en el **icono de engranaje** de la esquina superior derecha de la ventana. Después, seleccione **Opciones de Internet**.<br />(En versiones anteriores de Internet Explorer, encontrará esta opción en **Herramientas > Opciones de Internet**.
   
    ![Ir a Herramientas > Opciones de Internet](./media/manage-access-panel-browser-extension/internetoptions.png)
2. Haga clic en la pestaña **Programas** y, después, en el botón **Administrar complementos**.
   
    ![Hacer clic en Administrar complementos](./media/manage-access-panel-browser-extension/internetoptions_programs.png)
3. En este diálogo, seleccione **Extensión del Panel de acceso** y, después, haga clic en el botón **Habilitar**.
   
    ![Hacer clic en Habilitar](./media/manage-access-panel-browser-extension/enableaddon.png)
4. Para guardar estos cambios, cierre cada ventana de Internet Explorer y luego vuelva a abrir Internet Explorer.

## <a name="enable-extensions-for-inprivate-browsing"></a>Habilitar extensiones para la exploración de InPrivate
Si usa el modo de exploración de InPrivate:

1. En Internet Explorer, haga clic en el **icono de engranaje** de la esquina superior derecha de la ventana. Después, seleccione **Opciones de Internet**.<br />(En versiones anteriores de Internet Explorer, encontrará esta opción en **Herramientas > Opciones de Internet**.
   
    ![Un ejemplo de la ventana de diagnóstico](./media/manage-access-panel-browser-extension/inprivateoptions.png)
2. Vaya a la pestaña **Privacidad** y **desactive** la casilla **Deshabilitar barras de herramientas y extensiones cuando se inicie la exploración de InPrivate**.</p>
   
    ![Desactivar Deshabilitar barras de herramientas y extensiones cuando se inicie la exploración de InPrivate](./media/manage-access-panel-browser-extension/enabletoolbars.png)
3. Para guardar estos cambios, cierre cada ventana de Internet Explorer y luego vuelva a abrir Internet Explorer.

## <a name="uninstall-the-access-panel-extension"></a>Desinstalar la extensión del Panel de acceso
Para desinstalar la extensión del Panel de acceso desde el equipo:

1. En el teclado, presione la **clave de Windows** para abrir el menú Inicio. Cuando se abre el menú, puede escribir cualquier cosa para realizar una búsqueda. Escriba "Panel de Control" y luego abra el **Panel de Control** cuando aparezca en los resultados de la búsqueda.
   
    ![Buscar el Panel de control](./media/manage-access-panel-browser-extension/search_sm.png)
2. En la esquina superior derecha del Panel de control, cambie la opción **Ver** a **Iconos grandes**. Luego busque y haga clic en el botón **Programas y características**.
   
    ![Cambiar la vista para mostrar iconos grandes](./media/manage-access-panel-browser-extension/control_panel.png)
3. En la lista, seleccione **Access Panel Extension** (Extensión del Panel de acceso) y, después, haga clic en el botón **Desinstalar**.
   
    ![Hacer clic en Desinstalar](./media/manage-access-panel-browser-extension/uninstall.png)
4. Después, puede intentar instalar la extensión de nuevo para ver si se ha resuelto el problema.

Si encuentra problemas al desinstalar la extensión, también puede quitarla usando la herramienta [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) .

## <a name="related-articles"></a>Artículos relacionados
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](what-is-single-sign-on.md)
* [Implementación de la extensión del Panel de acceso para Internet Explorer mediante la directiva de grupo](deploy-access-panel-browser-extension.md)

