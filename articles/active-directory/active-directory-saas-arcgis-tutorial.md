---
title: "Tutorial: Integración de Azure Active Directory con ArcGIS Online | Microsoft Docs"
description: "Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y ArcGIS Online."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/13/2017
ms.author: jeedes
ms.openlocfilehash: b09dd977cbf5c4273667167217e86bb79ac2a9d8
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a>Tutorial: Integración de Azure Active Directory con ArcGIS Online

En este tutorial, obtendrá información sobre cómo integrar ArcGIS Online con Azure Active Directory (Azure AD).

La integración de ArcGIS Online con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a ArcGIS Online.
- Puede permitir que los usuarios inicien sesión automáticamente en ArcGIS Online (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ArcGIS Online, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en ArcGIS Online

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. La situación descrita en este tutorial consta de dos bloques de creación principales:

1. Incorporación de ArcGIS Online desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-arcgis-online-from-the-gallery"></a>Incorporación de ArcGIS Online desde la galería
Para configurar la integración de ArcGIS Online en Azure AD, deberá agregar ArcGIS Online desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ArcGIS Online desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **ArcGIS Online**, seleccione **ArcGIS Online** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![ArcGIS Online en la lista de resultados](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con ArcGIS Online con un usuario de prueba denominado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de ArcGIS Online para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ArcGIS Online.

Para establecer la relación de vínculo, en ArcGIS Online, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con ArcGIS Online, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para permitir que los usuarios utilicen esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**: para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de ArcGIS Online](#create-a-arcgis-online-test-user)**: para tener un homólogo de Britta Simon en ArcGIS Online que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**: para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación ArcGIS Online.

**Para configurar el inicio de sesión único de Azure AD con ArcGIS Online, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **ArcGIS Online**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. En la sección **Dominio y direcciones URL de ArcGIS Online**, lleve a cabo el paso siguiente:

    ![Información sobre el dominio y las direcciones URL de inicio de sesión único de ArcGIS Online](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.maps.arcgis.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `<companyname>.maps.arcgis.com`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico para clientes de ArcGIS Online](http://support.esri.com/en/) para obtener estos valores. 
 


4. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. Haga clic en el botón **Guardar** .

    ![Botón Guardar de Configuración de inicio de sesión único](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. En otra ventana del explorador web, inicie sesión en como administrador en el sitio de la compañía de ArcGIS.

7. Haga clic en **EDITAR CONFIGURACIÓN**.

    ![Editar configuración](./media/active-directory-saas-arcgis-tutorial/ic784742.png "Editar configuración")

8. Haga clic en **Seguridad**.

    ![Seguridad](./media/active-directory-saas-arcgis-tutorial/ic784743.png "Seguridad")

9. En **Inicios de sesión de la empresa**, haga clic en **ESTABLECER PROVEEDOR DE IDENTIDADES**.

    ![Enterprise Logins (Inicios de sesión de la empresa)](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise Logins (Inicios de sesión de la empresa)")

10. En la sección **Configurar proveedor de identidades** , realice los pasos siguientes:
   
    ![Set Identity Provider (Establecer proveedor de identidades)](./media/active-directory-saas-arcgis-tutorial/ic784745.png "Set Identity Provider (Establecer proveedor de identidades)")
   
    a. En el cuadro de texto **Nombre**, escriba el nombre de la organización.

    b. En **Metadata for the Enterprise Identity Provider will be supplied using** (Los metadatos para el proveedor de identidades de la empresa se proporcionarán con), seleccione **A File** (Un archivo).

    c. Haga clic en **Choose file**(Elegir archivo) para cargar el archivo de metadatos descargado.

    d. Haga clic en **ESTABLECER PROVEEDOR DE IDENTIDADES**.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Crear**.
 
### <a name="create-a-arcgis-online-test-user"></a>Crear un usuario de prueba de ArcGIS Online

Para permitir que los usuarios de Azure AD inicien sesión en ArcGIS Online, tienen que aprovisionarse en ArcGIS Online.  
En el caso de ArcGIS Online, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **ArcGIS** .

2. Haga clic en **INVITAR A MIEMBROS**.
   
    ![Invitar a miembros](./media/active-directory-saas-arcgis-tutorial/ic784747.png "Invitar a miembros")

3. Seleccione **Agregar miembros automáticamente sin enviar un correo electrónico** y luego haga clic en **SIGUIENTE**.
   
    ![Agregar miembros automáticamente](./media/active-directory-saas-arcgis-tutorial/ic784748.png "Agregar miembros automáticamente")

4. En la página de diálogo **Miembros** , realice los pasos siguientes:
   
     ![Agregar y revisar](./media/active-directory-saas-arcgis-tutorial/ic784749.png "Agregar y revisar")
    
     a. Escriba los valores de **correo electrónico**, **nombre** y **apellido** de una cuenta de AAD válida que desea aprovisionar.
  
     b. Haga clic en **AGREGAR Y REVISAR**.
5. Revise los datos que ha escrito y luego haga clic en **AGREGAR MIEMBROS**.
   
    ![Agregar miembros](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Agregar miembros")
        
    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo y seguirá un vínculo para confirmar su cuenta antes de que se active.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a ArcGIS Online.

![Asignación del rol de usuario][200] 

**Para asignar a Britta Simon a ArcGIS Online, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, vaya a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego, haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones. seleccione **ArcGIS Online**.

    ![Vínculo a ArcGIS Online en la lista de aplicaciones](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ArcGIS Online en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación ArcGIS Online.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

