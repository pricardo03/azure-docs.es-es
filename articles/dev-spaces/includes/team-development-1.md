---
title: archivo de inclusión
description: archivo de inclusión
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 2a6118bd23c6e8319ad4fa26a266948a4dad1b9f
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939177"
---
Hasta ahora, ha ejecutado el código de la aplicación como si fuera el único desarrollador que trabaja en la aplicación. En esta sección, aprenderá cómo Azure Dev Spaces simplifica el desarrollo en equipo:
* Habilite un equipo de desarrolladores para trabajar en el mismo entorno, mediante el trabajo en un espacio de desarrollo compartido o en espacios de desarrollo distintos, lo que sea necesario.
* Permitirá que cada desarrollador efectúe una iteración en su código de forma aislada y sin peligro de interrumpir a otros desarrolladores.
* Probará el código de un extremo a otro, antes de confirmarlo, sin tener que crear bocetos o dependencias simuladas.

### <a name="challenges-with-developing-microservices"></a>Dificultades con el desarrollo de microservicios
Por el momento, la aplicación de ejemplo no es muy compleja. Pero en el trabajo de desarrollo del mundo real, las dificultades pronto surgen a medida que se agregan más servicios y crece el equipo de desarrollo.

Imagínese trabajando en un servicio que interactúa con otras docenas de servicios.

- Puede ser poco realista ejecutarlo todo localmente para el desarrollo. Es posible que la máquina de desarrollo no tenga recursos suficientes para ejecutar la aplicación completa. O bien, quizás la aplicación tenga puntos de conexión que deben estar accesibles públicamente (por ejemplo, la aplicación responde a un webhook desde una aplicación SaaS).

- Puede intentar ejecutar solo los servicios de los que depende, pero esto significa que debe conocer el cierre completo de dependencias (por ejemplo, las dependencias de dependencias). O bien sea cuestión de no saber fácilmente cómo compilar y ejecutar las dependencias porque no ha trabajado con ellas.
- Algunos desarrolladores lo que hacen es simular o realizar bocetos de muchas de sus dependencias de servicios. Este sistema puede funcionar a veces, pero administrar esos bocetos puede ocupar pronto todo el trabajo de desarrollo. Además, este enfoque hace que el aspecto de su espacio de desarrollo es muy distinto en producción y que pueden aparecer errores sutiles.
- A eso se suma la dificultad de hacer cualquier tipo de prueba de un extremo a otro. Las pruebas de integración solo pueden suceder de forma realista tras una confirmación, lo que significa que verá problemas más adelante en el ciclo de desarrollo.

![](../media/common/microservices-challenges.png)


### <a name="work-in-a-shared-dev-space"></a>Trabajo en un espacio de desarrollo compartido
Con Azure Dev Spaces se puede configurar un espacio de desarrollo *compartido* en Azure. Cada desarrollador puede centrarse en su parte de la aplicación y desarrollar de forma iterativa *código de confirmación previa* en un espacio de desarrollo que ya contiene los restantes servicios y recursos en la nube de los que dependen sus escenarios. Las dependencias siempre están actualizadas y los desarrolladores trabajan de una manera que refleja el entorno de producción.

### <a name="work-in-your-own-space"></a>Trabajar en su propio espacio de trabajo
A medida que desarrolla código para su servicio, y antes de estar preparado para insertarlo en el repositorio, el código estará normalmente en buenas condiciones. Aún estará dándole forma, probándolo y experimentando con soluciones de manera iterativa. Azure Dev Spaces proporciona el concepto de un **espacio**, que le permite trabajar de forma aislada sin miedo a que le interrumpan los miembros de su equipo.

> [!Note]
> Antes de continuar, cierre todas las ventanas de VS Code de ambos servicios y, a continuación, ejecute `azds up -d` en cada una de las carpetas raíz del servicio. (Se trata de una limitación en versión preliminar).

Examinemos de forma más detallada donde se están ejecutando los servicios actualmente. Ejecute el comando`azds list` y verá una salida similar a la siguiente:

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------------------
mywebapi     default  mywebapi-0.1.0     80/TCP  2m ago     <not attached>
webfrontend  default  webfrontend-0.1.0  80/TCP  1m ago     http://webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

La columna Espacio muestra que ambos servicios se ejecutan en un espacio denominado `default`. Cualquiera que abra la dirección URL pública y vaya a la aplicación web, invocará la ruta de acceso al código que ha escrito anteriormente y que se ejecuta a través de ambos servicios. Ahora suponga que quiere seguir desarrollando `mywebapi`. ¿Cómo puede realizar cambios de código y probarlos y sin interrumpir a otros desarrolladores que usan el entorno de desarrollo? Para ello, configurará su propio espacio.

### <a name="create-a-dev-space"></a>Creación de un espacio de desarrollo
Para ejecutar su propia versión de `mywebapi` en un espacio distinto de `default`, puede crear su propio espacio mediante el comando siguiente:

``` 
azds space select --name scott
```

Cuando se le solicite, seleccione `default` como el **espacio de desarrollo primario**. Esto significa que nuestro nuevo espacio `default/scott`, derivará desde el espacio `default`. En breve veremos cómo nos ayudará esto con las pruebas. 

En el ejemplo anterior, he usado mi nombre para que mis compañeros puedan identificar el espacio nuevo como el espacio en el que estoy trabajando, pero puede denominarlo como quiera y ser flexible en relación a su significado, como "sprint4" o "demo".

Ejecute el comando `azds space list` para ver una lista de todos los espacios del entorno de desarrollo. Aparece un asterisco (*) junto al espacio seleccionado actualmente. En su caso, el espacio llamado "default/scott" se seleccionó automáticamente cuando se creó. Puede seleccionar otro espacio en cualquier momento con el comando `azds space select`.
