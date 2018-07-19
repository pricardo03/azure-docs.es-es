---
title: 'Tutorial: Integración de Azure Active Directory con Sansan | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Sansan.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: cc070f7c4cb201e68c93b0b1337982325df74663
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39051268"
---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Tutorial: Integración de Azure Active Directory con Sansan

En este tutorial, obtendrá información sobre cómo integrar Sansan con Azure Active Directory (Azure AD).

Integrar Sansan con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Sansan.
- Puede permitir que los usuarios inicien sesión automáticamente en Sansan (Inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Sansan, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Sansan

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Sansan desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-sansan-from-the-gallery"></a>Incorporación de Sansan desde la galería
Para configurar la integración de Sansan en Azure AD, deberá agregar Sansan desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Sansan desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

4. En el cuadro de búsqueda, escriba **Sansan**.

    ![Creación de un usuario de prueba de Azure AD](./media/sansan-tutorial/tutorial_sansan_search.png)

5. En el panel de resultados, seleccione **Sansan** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Sansan con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Sansan para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Sansan.

Para establecer la relación de vínculo, en Sansan, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Sansan, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Sansan](#creating-a-sansan-test-user)**: para tener un homólogo de Britta Simon en Sansan que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en su aplicación Sansan.

**Para configurar el inicio de sesión único de Azure AD con Sansan, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Sansan**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/sansan-tutorial/tutorial_sansan_samlbase.png)

3. En la sección **Dominio y direcciones URL de Sansan**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/sansan-tutorial/tutorial_sansan_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: 
    
    | Environment | URL |
    |:--- |:--- |
    | Web de PC |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | Aplicación nativa móvil |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | Configuración del explorador móvil |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección de inicio de sesión real. Póngase en contacto con el [equipo de soporte al cliente de Sansan](https://www.sansan.com/form/contact) para obtener estos valores. 
     
4. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/sansan-tutorial/tutorial_sansan_certificate.png) 

5. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/sansan-tutorial/tutorial_general_400.png)

6. La aplicación Sansan espera varios **identificadores** y **direcciones URL de respuesta** para admitir diversos entornos (web PC, aplicación móvil nativa, configuración del explorador para móviles), que se pueden configurar con el script de PowerShell. A continuación se explican los pasos detallados.

7. Para configurar varios **identificadores** y **direcciones URL de respuesta** para la aplicación Sansan mediante el script de PowerShell, siga los pasos siguientes:

    ![Configurar el objeto de inicio de sesión único](./media/sansan-tutorial/tutorial_sansan_objid.png)  

    a. Vaya a la página **Propiedades** de la aplicación **Sansan**, copie el **Identificador del objeto** con el botón **Copiar** y péguelo en el Bloc de notas.

    b. El **Identificador del objeto** que ha copiado de Azure Portal se usará como **ServicePrincipalObjectId** en el script de PowerShell que se usará más adelante en el tutorial. 

    c. Ahora, abra un símbolo del sistema de Windows PowerShell con privilegios elevados.
    
    >[!NOTE] 
    > Debe instalar el módulo de AzureAD (use el comando `Install-Module -Name AzureAD`). Si se le pide que instale un módulo de NuGet o el nuevo módulo de PowerShell para Azure Active Directory V2, escriba S y presione ENTRAR.

    d. Ejecute `Connect-AzureAD` e inicie sesión con una cuenta de usuario administrador global.

    e. Use el script siguiente para actualizar diversas direcciones URL a una aplicación:

    ```poweshell
     Param(
    [Parameter(Mandatory=$true)][guid]$ServicePrincipalObjectId,
    [Parameter(Mandatory=$false)][string[]]$ReplyUrls,
    [Parameter(Mandatory=$false)][string[]]$IdentifierUrls
    )

    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId

    if($ReplyUrls.Length)
    {
    echo "Updating Reply urls"
    Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls $ReplyUrls
    echo "updated"
    }
    if($IdentifierUrls.Length)
    {
    echo "Updating Identifier urls"
    $applications = Get-AzureADApplication -SearchString $servicePrincipal.AppDisplayName 
    echo "Found Applications =" $applications.Length
    $i = 0;
    do
    {  
    $application = $applications[$i];
    if($application.AppId -eq $servicePrincipal.AppId){
    Set-AzureADApplication -ObjectId $application.ObjectId -IdentifierUris $IdentifierUrls
    $servicePrincipal = Get-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId
    echo "Updated"
    return;
    }
    $i++;
    }while($i -lt $applications.Length);
    echo "Not able to find the matched application with this service principal"
    }
    ```

8. Tras finalizar correctamente el script de PowerShell, el resultado del script será el siguiente, tal y como se muestra a continuación, y los valores de las direcciones URL se actualizan, pero no se reflejan en Azure Portal. 

    ![Configurar el script de inicio de sesión único](./media/sansan-tutorial/tutorial_sansan_powershell.png)


9. En la sección **Configuración de Sansan**, haga clic en **Configurar Sansan** para abrir la ventana **Configurar inicio de sesión**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/sansan-tutorial/tutorial_sansan_configure.png) 

10. Para configurar el inicio de sesión único en **Sansan**, es preciso enviar el **certificado** descargado, la **dirección URL de cierre de sesión**, el **identificador de identidad de SAML** y la **dirección URL del servicio de inicio de sesión único de SAML** al [equipo de soporte técnico de Sansan](https://www.sansan.com/form/contact). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

>[!NOTE]
>La configuración del explorador de PC también funciona para aplicaciones y exploradores móviles junto con la web del PC. 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/sansan-tutorial/create_aaduser_01.png) 

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/sansan-tutorial/create_aaduser_02.png) 

3. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/sansan-tutorial/create_aaduser_03.png) 

4. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/sansan-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-sansan-test-user"></a>Creación de un usuario de prueba de Sansan

En esta sección, creará un usuario llamado Britta Simon en Sansan. La aplicación Sansan necesita que todos los usuarios estén aprovisionados en la aplicación antes de realizar el inicio de sesión único. 

>[!NOTE]
>Si necesita crear un usuario de forma manual o por lotes de usuarios, póngase en contacto con el [equipo de soporte técnico de Sansan](https://www.sansan.com/form/contact). 

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Sansan para que use el inicio de sesión único de Azure.

![Asignar usuario][200] 

**Para asignar Britta Simon a Sansan, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Sansan**.

    ![Configurar inicio de sesión único](./media/sansan-tutorial/tutorial_sansan_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Sansan en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Sansan.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sansan-tutorial/tutorial_general_01.png
[2]: ./media/sansan-tutorial/tutorial_general_02.png
[3]: ./media/sansan-tutorial/tutorial_general_03.png
[4]: ./media/sansan-tutorial/tutorial_general_04.png

[100]: ./media/sansan-tutorial/tutorial_general_100.png

[200]: ./media/sansan-tutorial/tutorial_general_200.png
[201]: ./media/sansan-tutorial/tutorial_general_201.png
[202]: ./media/sansan-tutorial/tutorial_general_202.png
[203]: ./media/sansan-tutorial/tutorial_general_203.png

