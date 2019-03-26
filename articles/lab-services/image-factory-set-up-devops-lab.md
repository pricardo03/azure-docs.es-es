---
title: Ejecute un generador de imágenes de Azure DevOps en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo crear un generador de imágenes personalizadas en Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 62cb8222b592660f2e7ab32d438fd4073246ee50
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58440039"
---
# <a name="run-an-image-factory-from-azure-devops"></a>Ejecute un generador de imágenes de Azure DevOps
En este artículo se trata todos los preparativos necesarios para ejecutar el generador de imágenes de DevOps de Azure (anteriormente Visual Studio Team Services).

> [!NOTE]
> Funcionará cualquier motor de orquestaciones. Azure DevOps no es obligatorio. El generador de imágenes se ejecuta con secuencias de comandos de PowerShell de Azure, por lo que es posible ejecutar manualmente, mediante el programador de tareas de Windows, otros sistemas de CI/CD y así sucesivamente.

## <a name="create-a-lab-for-the-image-factory"></a>Crear un laboratorio para el generador de imágenes
Es el primer paso para configurar el generador de imágenes crear un laboratorio en Azure DevTest Labs. Este laboratorio es la práctica de fábrica imagen donde creamos las máquinas virtuales y guardar las imágenes personalizadas. Esta práctica se considera como parte del proceso general de fábrica de imagen. Una vez creado un laboratorio, asegúrese de guardar el nombre, ya que la necesitará más adelante.

## <a name="scripts-and-templates"></a>Secuencias de comandos y plantillas
El siguiente paso para adoptar el generador de imágenes para su equipo es comprender lo que está disponible. Los scripts de fábrica de imagen y las plantillas están disponibles públicamente en el [repositorio de GitHub de DevTest Labs](https://github.com/Azure/azure-devtestlab/tree/master/Scripts/ImageFactory). Este es un resumen de las piezas:

- Generador de imágenes. Es la carpeta raíz. 
    - Configuración. Las entradas en el generador de imágenes
        - GoldenImages. Esta carpeta contiene archivos JSON que representan las definiciones de imágenes personalizadas.
        - Labs.JSON. Archivo donde los equipos de registrarse para recibir imágenes personalizadas específicas.
- Secuencias de comandos. El motor para el generador de imágenes.

Los artículos de esta sección proporcionan más detalles acerca de estos scripts y plantillas. 

## <a name="create-an-azure-devops-team-project"></a>Crear un proyecto de equipo de DevOps de Azure
Azure DevOps le permiten almacenar el código fuente, ejecute Azure PowerShell en un solo lugar. Puede programar ejecuciones periódicas para mantener actualizadas las imágenes. Hay buenas instalaciones para registrar los resultados a diagnosticar cualquier problema.  Uso de Azure DevOps no es un requisito sin embargo, puede usar cualquier agente/motor que puede conectarse a Azure y puede ejecutar Azure PowerShell.

Si tiene una cuenta existente de DevOps o un proyecto que le gustaría usar en su lugar, omita este paso.

Para empezar, cree una cuenta gratuita de DevOps de Azure. Visite https://www.visualstudio.com/ y seleccione **empezar a trabajar de forma gratuita** justo debajo **Azure DevOps** (anteriormente VSTS). Deberá elegir un nombre de cuenta único y asegúrese de elegir si desea administrar código con Git. Una vez creado, guarde la dirección URL para el proyecto de equipo. Esta es una dirección URL de ejemplo: https://<accountname>.visualstudio.com/MyFirstProject.

## <a name="check-in-the-image-factory-to-git"></a>Compruebe en el generador de imágenes a Git
Todos los PowerShell, plantillas y configuración para el generador de imágenes se encuentran en el [repositorio público de GitHub de DevTest Labs](https://github.com/Azure/azure-devtestlab/tree/master/Scripts/ImageFactory). La manera más rápida de obtener el código en el nuevo proyecto de equipo consiste en Importar un repositorio. Esto extrae en el repositorio de DevTest Labs completo (por lo que obtendrá docs adicionales y ejemplos). 

1. Visite el proyecto de DevOps de Azure que creó en el paso anterior (dirección URL es similar a **https://<accountname>.visualstudio.com/MyFirstProject**).
2. Seleccione **importar un repositorio**.
3. Escriba el **dirección URL de clonación** para el repositorio de DevTest Labs: `https://github.com/Azure/azure-devtestlab`.
4. Seleccione **Import** (Importar).

    ![Importación del repositorio de Git](./media/set-up-devops-lab/import-git-repo.png)

Si decide proteger solo exactamente lo que necesita (los archivos de imagen factory), siga los pasos [aquí](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs) para clonar el repositorio de Git e insertar sólo los archivos ubicados en el **secuencias de comandos/ImageFactory** directory.

## <a name="create-a-build-and-connect-to-azure"></a>Crear una compilación y conectarse a Azure
En este punto, tiene los archivos de origen almacenados en un repositorio de Git en DevOps de Azure. Ahora, deberá configurar una canalización para ejecutar Azure PowerShell. Hay muchas opciones en realizar estos pasos. En este artículo, usar la definición de compilación por motivos de simplicidad, pero funciona con la compilación de DevOps, versión de DevOps (uno o varios entornos), otros motores de ejecución, como programador de tareas de Windows o cualquier otro instrumento que puede ejecutar Azure PowerShell.

> [!NOTE]
> Un punto importante a tener en cuenta que algunos de los archivos de PowerShell tardan mucho tiempo en ejecutarse cuando hay mucha (10 +) imágenes personalizadas para crear. Agentes de compilación o versión de DevOps hospedado libre tienen un tiempo de espera de 30 minutos, por lo que no puede usar al agente hospedado gratuita cuando empiece a crear muchas imágenes. Este desafío de tiempo de espera se aplica a cualquier agente que decida utilizar, es conveniente comprobar por adelantado que puede ampliar los tiempos de espera típico de larga ejecución de los scripts de PowerShell de Azure. En el caso de DevOps de Azure, puede usar agentes hospedados pago o utilizar a su propio agente de compilación.

1. Para empezar, seleccione **Configurar compilación** en la página principal de un proyecto de DevOps:

    ![Botón de compilación del programa de instalación](./media/set-up-devops-lab/setup-build-button.png)
2. Especifique un **nombre** para la compilación (por ejemplo: Compilar y entregar imágenes en DevTest Labs). 
3. Seleccione un **vacía** definición de compilación y seleccione **aplicar** para crear la compilación. 
4. En esta fase, puede elegir **hospedado** para el agente de compilación. 
5. **Guardar** la definición de compilación.

    ![Definición de compilación](./media/set-up-devops-lab/build-definition.png)

## <a name="configure-the-build-variables"></a>Configurar las variables de compilación
Para simplificar los parámetros de línea de comandos, encapsulan los valores de clave que impulsan el generador de imágenes a un conjunto de variables de compilación. Seleccione el **Variables** pestaña y verá una lista de varias variables de forma predeterminada. Esta es la lista de variables que escriba en a DevOps de Azure:


| Nombre de la variable | Valor | Notas |
| ------------- | ----- | ----- |
| ConfigurationLocation | /Scripts/ImageFactory/Configuration | Se trata de la ruta de acceso completa del repositorio para la **configuración** carpeta. Si ha importado el repositorio completo anterior, el valor a la izquierda es correcto. En caso contrario, se actualice para que apunte a la ubicación de configuración. |
| DevTestLabName | MyImageFactory | El nombre del laboratorio en Azure DevTest Labs usa como el generador para producir imágenes. Si no tiene uno, cree uno. Asegúrese de que el laboratorio está en la misma suscripción que el punto de conexión de servicio tiene acceso. |
| ImageRetention | 1 | El número de imágenes que desea guardar de cada tipo. Valor predeterminado se establece en 1. |
| MachinePassword | ******* | La contraseña de cuenta de administrador integrada para las máquinas virtuales. Esta es una cuenta transitoria, así que asegúrese de que es segura. Seleccione el pequeño icono de bloqueo de la derecha para asegurarse de que es una cadena segura. |
| MachineUserName | ImageFactoryUser | El nombre de usuario de cuenta de administrador integrada para las máquinas virtuales. Se trata de una cuenta transitoria. |
| StandardTimeoutMinutes | 30 | El tiempo de espera que debemos esperar las operaciones normales de Azure. |
| SubscriptionId |  0000000000-0000-0000-0000-0000000000000 | El identificador de la suscripción donde existe el laboratorio y que el punto de conexión de servicio tiene acceso. |
| VMSize | Standard_A3 | El tamaño de la máquina virtual que se usará para la **crear** paso. Las máquinas virtuales creadas son transitorias. El tamaño debe ser el único que [habilitada para el laboratorio](devtest-lab-set-lab-policy.md). Confirme que hay suficiente [cuota de núcleos de suscripción](../azure-subscription-service-limits.md). 

![Variables de compilación](./media/set-up-devops-lab/configure-build-variables.png)

## <a name="connect-to-azure"></a>Conexión a Azure
El siguiente paso es configurar el servicio principal. Se trata de una identidad en Azure Active Directory que permite que el agente de compilación de DevOps para que funcione en Azure en el nombre de usuario. Para configurarlo, comience con Agregar paso de compilación de primera Azure PowerShell.

1. Seleccione **agregar tarea**.
2. Busque **Azure PowerShell**. 
3. Una vez que lo encuentre, seleccione **agregar** para agregar la tarea a la compilación. Al hacerlo, aparecerá la tarea en el lado izquierdo como agregado.

![Configurar el paso de PowerShell](./media/set-up-devops-lab/set-up-powershell-step.png)

Es la manera más rápida de configurar un servicio principal permitir que Azure DevOps lo haga por nosotros. 

1. Seleccione el **tarea** que acaba de agregar.
2. Para **tipo de conexión de Azure**, elija **Azure Resource Manager**. 
3. Seleccione el **administrar** vínculo para configurar la entidad de servicio. 

Para obtener más información, consulte este [entrada de blog](https://devblogs.microsoft.com/devops/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/). Cuando se selecciona el **administrar** vínculo, aterrizará en el lugar correcto en DevOps (segunda captura de pantalla en la entrada de blog) para establecer la conexión a Azure. Asegúrese de elegir **punto de conexión de servicio de Azure Resource Manager** al configurar esto.

## <a name="complete-the-build-task"></a>Completar la tarea de compilación
Si selecciona la tarea de compilación, verá todos los detalles en el panel derecho que se debe rellenar. 

1. En primer lugar, nombre de la tarea de compilación: **Creación de máquinas virtuales**. 
2. Elija la **serviceprincipal** creó eligiendo **Azure Resource Manager**
3. Elija la **punto de conexión de servicio**. 
4. Para **ruta del Script**, seleccione **... (puntos suspensivos)**  a la derecha.
5. Vaya a **MakeGoldenImageVMs.ps1** secuencia de comandos. 
6. Parámetros de script deben tener este aspecto: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -DevTestLabName $(DevTestLabName) -vmSize $(VMSize) -machineUserName $(MachineUserName) -machinePassword (ConvertTo-SecureString -string '$(MachinePassword)' -AsPlainText -Force) -StandardTimeoutMinutes $(StandardTimeoutMinutes)`

    ![Completar la definición de compilación](./media/set-up-devops-lab/complete-build-definition.png)


## <a name="queue-the-build"></a>Poner en cola la compilación
Vamos a comprobar que tiene todo configurado correctamente poniendo en cola una compilación nueva. Mientras se ejecuta la compilación, cambie a la [portal Azure](https://portal.azure.com) y seleccione en **todas las máquinas virtuales** en el laboratorio de fábrica de la imagen para confirmar que todo funciona correctamente. Debería ver tres máquinas virtuales se crean en el laboratorio.

![Máquinas virtuales del laboratorio](./media/set-up-devops-lab/vms-in-lab.png)

## <a name="next-steps"></a>Pasos siguientes 
El primer paso para configurar el generador de imágenes basado en Azure DevTest Labs es completando. En el siguiente artículo de la serie, obtenga las máquinas virtuales generalizadas y se guardan en imágenes personalizadas. A continuación, ya las distribuye a todos los laboratorios. Consulte el artículo siguiente de la serie: [Guardar las imágenes personalizadas y distribuir a varios laboratorios](image-factory-save-distribute-custom-images.md).
