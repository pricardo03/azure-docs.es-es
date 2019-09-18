---
title: Administración de laboratorios de clase en Azure Lab Services | Microsoft Docs
description: Obtenga información sobre cómo crear y configurar un laboratorio de clase, ver todos los laboratorios de clase, compartir el vínculo de registro con un usuario del laboratorio o eliminar un laboratorio.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2019
ms.author: spelluru
ms.openlocfilehash: 1f9cb82abd5bc0823f5e7bc23fe437007bccc8e0
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70873579"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Administración de laboratorios de clase en Azure Lab Services 
En este artículo se describe cómo crear y eliminar un laboratorio de clase. También se muestra cómo ver todos los laboratorios de clase de una cuenta de laboratorio. 

## <a name="prerequisites"></a>Requisitos previos
Para configurar un laboratorio de clase en una cuenta de laboratorio, debe ser miembro del rol **Creador de laboratorio** en la cuenta de laboratorio. La cuenta que usó para crear una cuenta de laboratorio se agrega automáticamente a este rol. Un propietario de laboratorio puede agregar otros usuarios al rol Creador de laboratorio mediante los pasos que se indican en el siguiente artículo: [Incorporación de un usuario al rol Creador de laboratorio](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Creación de un laboratorio educativo

1. Vaya al [sitio web de Azure Lab Services](https://labs.azure.com). Tenga en cuenta que Internet Explorer 11 aún no se admite. 
2. Seleccione **Iniciar sesión**. Seleccione o escriba un **Id. de usuario** que sea miembro del rol **Creador de laboratorio** en la cuenta del laboratorio y escriba la contraseña. Azure Lab Services es compatible con cuentas profesionales y cuentas Microsoft. 
3. En la ventana **Nuevo laboratorio**, lleve a cabo las siguientes acciones: 
    1. Especifique un **nombre** para el laboratorio. 
    2. Especifique el máximo **número de máquinas virtuales** del laboratorio. Puede aumentar o reducir el número de máquinas virtuales del laboratorio más adelante. 
    6. Seleccione **Guardar**.

        ![Creación de un laboratorio educativo](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. En la página **Seleccionar especificaciones de máquina virtual**, realice los pasos siguientes:
    1. Seleccione un **tamaño** para las máquinas virtuales (VM) creadas en el laboratorio. Actualmente, se permiten los tamaños **pequeño**, **medio**, **medio (virtualización)** , **grande** y **GPU**. Para obtener información detallada, vea la sección sobre los [tamaños de las máquinas virtuales](#vm-sizes).
    1. Seleccione la **región** en la que quiere que se creen las máquinas virtuales. 
    1. Seleccione la **imagen de máquina virtual** que se usará para crear máquinas virtuales en el laboratorio. Si selecciona una imagen de Linux, verá una opción para habilitar la conexión a Escritorio remoto para ella. Para más información, consulte [Habilitar conexión de escritorio remoto para Linux](how-to-enable-remote-desktop-linux.md).
    1. Seleccione **Next** (Siguiente).

        ![Especificaciones de máquina virtual](../media/tutorial-setup-classroom-lab/select-vm-specifications.png)    
5. En la página **Establecer credenciales**, especifique las credenciales predeterminadas de todas las máquinas virtuales del laboratorio. 
    1. Especifique el **nombre del usuario** para todas las máquinas virtuales del laboratorio.
    2. Especifique la **contraseña** del usuario. 

        > [!IMPORTANT]
        > Tome nota de ambos. No se volverán a mostrar.
    3. Deshabilite la opción **Usar la misma contraseña para todas las máquinas virtuales** si quiere que los alumnos establezcan sus propias contraseñas. Este paso es **opcional**. 

        Un profesor puede elegir usar la misma contraseña para todas las máquinas virtuales en el laboratorio o permitir a los alumnos establecer contraseñas para sus máquinas virtuales. De forma predeterminada, esta configuración está habilitada para todas las imágenes Windows y Linux, excepto para Ubuntu. Al seleccionar la máquina virtual **Ubuntu**, esta opción está deshabilitada, por lo que se pedirá a los alumnos que establezcan una contraseña cuando inicien sesión por primera vez.
    1. Seleccione **Crear**. 

        ![Establecer credenciales](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. En la página **Configurar plantilla**, puede ver el estado del proceso de creación del laboratorio. La creación de la plantilla en el laboratorio tarda un máximo de 20 minutos. En un laboratorio, una plantilla es una imagen de máquina virtual base a partir de la que se crean las máquinas virtuales de todos los usuarios. Configure la máquina virtual de la plantilla de modo que esté configurada exactamente con lo que desea proporcionar a los usuarios del laboratorio.  

    ![Configurar plantilla](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Una vez completada la configuración de la plantilla, verá la siguiente página: 

    ![Página Configurar plantilla una vez terminada](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Los pasos siguientes son opcionales para este tutorial: 
    2. Seleccione **Conectar** para conectarse a la plantilla de máquina virtual. Si es una plantilla de máquina virtual Linux, decida si desea conectarse mediante SSH o RDP (si RDP está habilitado).
    1. Seleccione **Restablecer contraseña** para restablecer la contraseña de la máquina virtual. 
    1. Instale y configure el software en la plantilla de máquina virtual. 
    1. **Pare** la máquina virtual.  
    1. Escriba una **descripción** para la plantilla.
9. Seleccione **Siguiente** en la página de plantilla. 
10. En la página **Publicar la plantilla**, realice las acciones siguientes. 
    1. Para publicar la plantilla inmediatamente, seleccione la casilla *I understand I can't modify the template after publishing. This process can only be done once and can take up to an hour* (Comprendo que no puedo modificar la plantilla después de la publicación. Este proceso solo se puede realizar una vez y puede tardar hasta una hora), y seleccione **Publicar**.  Publique la plantilla si desea que las instancias de la máquina virtual de la plantilla estén disponibles para los usuarios del laboratorio.

        > [!WARNING]
        > Una vez que publique, no se puede cancelar la publicación. 
    2. Para publicar más adelante, seleccione **Guardar para más adelante**. Puede publicar la plantilla de máquina virtual una vez finalizado el asistente. Para más información sobre cómo configurar y publicar una vez finalizado el asistente, consulte la sección Publicación de la plantilla del artículo [Administración de laboratorios educativos](how-to-manage-classroom-labs.md).

        ![Publicar plantilla](../media/tutorial-setup-classroom-lab/publish-template.png)
11. Puede ver el **progreso de la publicación** de la plantilla. Este proceso puede tardar hasta una hora. 

    ![Publicar plantilla: progreso](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Podrá ver la página siguiente cuando la plantilla se haya publicado correctamente. Seleccione **Listo**.

    ![Publicar plantilla: correctamente](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Verá el **panel** del laboratorio. 
    
    ![Panel del laboratorio de clase](../media/tutorial-setup-classroom-lab/classroom-lab-home-page.png)
4. Cambie a la página **Máquinas virtuales** y confirme que ve las máquinas virtuales que tienen el estado **Sin asignar**. Estas máquinas virtuales no están asignadas a los alumnos todavía. Deberían estar en estado **Detenida**. En esta pagina, puede iniciar una máquina virtual de un alumno, conectarse a la máquina virtual, detener la máquina virtual y eliminar la máquina virtual. Puede iniciarlas en esta página o dejar que los alumnos inicien las máquinas virtuales. 

    ![Máquinas virtuales en estado detenido](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)

### <a name="vm-sizes"></a>Tamaños de VM  

| Size | Núcleos | RAM | DESCRIPCIÓN | 
| ---- | ----- | --- | ----------- | 
| Pequeña | 2 | 3,5 GB | Este tamaño es el más adecuado para la línea de comandos, apertura de navegador web, servidores web de poco tráfico, bases de datos pequeñas o medianas. |
| Mediano | 4 | 7 GB | Este tamaño es el más adecuado para bases de datos relacionales, análisis y almacenamiento en caché en memoria | 
| Mediano (virtualización anidada) | 4 | 16 GB | Este tamaño es el más adecuado para bases de datos relacionales, análisis y almacenamiento en caché en memoria. Este tamaño admite la virtualización anidada. <p>Este tamaño puede usarse en escenarios donde cada alumno necesita varias máquinas virtuales. Los profesores pueden usar la virtualización anidada para configurar varias máquinas virtuales anidadas de tamaño pequeño dentro de la máquina virtual. </p> |
| grande | 8 | 32 GB | Este tamaño es ideal para aplicaciones que necesitan CPU más rápidas, un mejor rendimiento de los discos locales, bases de datos grandes y memorias caché grandes. Este tamaño admite la virtualización anidada |  
| GPU pequeña (visualización) | 6 | 56 GB | Este tamaño es más adecuado para visualización remota, streaming, juegos y codificación mediante plataformas como OpenGL y DirectX. | 
| GPU pequeña (proceso) | 6 | 56 GB | Este tamaño es más adecuado para aplicaciones de proceso intensivo y uso intensivo de la red; por ejemplo, aplicaciones de inteligencia artificial y aprendizaje profundo. | 
| GPU mediana (visualización) | 12 | 112 GB | Este tamaño es más adecuado para visualización remota, streaming, juegos y codificación mediante plataformas como OpenGL y DirectX. | 

## <a name="view-all-classroom-labs"></a>Visualización de todos los laboratorios de clase
1. Vaya al [portal de Azure Lab Services](https://labs.azure.com).
2. Seleccione **Iniciar sesión**. Seleccione o escriba un **Id. de usuario** que sea miembro del rol **Creador de laboratorio** en la cuenta del laboratorio y escriba la contraseña. Azure Lab Services es compatible con cuentas profesionales y cuentas Microsoft. 
3. Confirme que ve todos los laboratorios en la cuenta de laboratorio seleccionada. 

    ![Todos los laboratorios](../media/how-to-manage-classroom-labs/all-labs.png)
3. Use la lista desplegable de la parte superior para seleccionar una cuenta de laboratorio distinta. Verá los laboratorios en la cuenta de laboratorio seleccionada. 

## <a name="delete-a-classroom-lab"></a>Eliminación de un laboratorio de clase
1. En el icono del laboratorio, seleccione los tres puntos (...) en la esquina. 

    ![Selección del laboratorio](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Seleccione **Eliminar**. 

    ![Botón Eliminar](../media/how-to-manage-classroom-labs/delete-button.png)
3. En el cuadro de diálogo **Delete lab** (Eliminar laboratorio), seleccione **Delete** (Eliminar). 

    ![Cuadro de diálogo Eliminar](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)

## <a name="switch-to-another-classroom-lab"></a>Cambiar a otro laboratorio de clase
Para cambiar a otro laboratorio de clase desde el actual, seleccione la lista desplegable de laboratorios en la cuenta de laboratorio en la parte superior.

![Seleccione el laboratorio de lista desplegable en la parte superior](../media/how-to-manage-classroom-labs/switch-lab.png)


## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como propietario del laboratorio, configurar y publicar plantillas](how-to-create-manage-template.md)
- [Como propietario del laboratorio, configurar y controlar el uso de un laboratorio](how-to-configure-student-usage.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)

