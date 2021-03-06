---
title: "教程：Azure Active Directory 与 SilkRoad Life Suite 集成 | Microsoft 文档"
description: "了解如何在 Azure Active Directory 和 SilkRoad Life Suite 之间配置单一登录。"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: ecf4e31ecea00d003fc47ea4cebb781ca58957f7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>教程：Azure Active Directory 与 SilkRoad Life Suite 集成
本教程的目的是说明如何将 SilkRoad Life Suite 与 Azure Active Directory (Azure AD) 集成。 

将 SilkRoad Life Suite 与 Azure AD 集成提供以下优势： 

* 可在 Azure AD 中控制谁有权访问 SilkRoad Life Suite 
* 可以让用户使用其 Azure AD 帐户自动登录到 SilkRoad Life Suite 单一登录 (SSO)

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>先决条件
若要配置 Azure AD 与 SilkRoad Life Suite 的集成，需备齐以下项目：

* 一个 Azure AD 订阅
* 已启用 SilkRoad Life Suite SSO 的订阅

>[!NOTE]
>为了测试本教程中的步骤，我们不建议使用生产环境。 
> 

测试本教程中的步骤应遵循以下建议：

* 不应使用生产环境，除非有此必要。
* 如果没有 Azure AD 试用环境，可以获取[一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。 

## <a name="scenario-description"></a>方案描述
本教程旨在介绍如何在测试环境中测试 Azure AD SSO。

本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 SilkRoad Life Suite 
2. 配置和测试 Azure AD SSO

## <a name="add-silkroad-life-suite-from-the-gallery"></a>从库中添加 SilkRoad Life Suite
要配置 SilkRoad Life Suite 与 Azure AD 的集成，需要从库中将 SilkRoad Life Suite 添加到托管 SaaS 应用列表。

**若要从库中添加 SilkRoad Life Suite，请执行以下步骤：**

1. 在 **Azure 经典门户**中，在左侧导航窗格上，单击“Active Directory”。 
   
    ![Active Directory][1]

2. 从“目录”列表中，选择要为其启用目录集成的目录。

3. 若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。
   
    ![应用程序][2]

4. 在页面底部单击“添加”。
   
    ![应用程序][3]

5. 在“要执行什么操作”对话框中，单击“从库中添加应用程序”。
   
    ![应用程序][4]

6. 在搜索框中，键入“SilkRoad Life Suite”。
   
    ![应用程序][5]

7. 在结果窗格中，选择“SilkRoad Life Suite”，并单击“完成”以添加该应用程序。
   
    ![应用程序][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
本部分的目的是说明如何基于名为“Britta Simon”的测试用户配置和测试 SilkRoad Life Suite 的 Azure AD SSO。

若要运行 SSO，Azure AD 需要知道与 Azure AD 用户相对应的 SilkRoad Life Suite 用户。 换句话说，需要在 Azure AD 用户与 SilkRoad Life Suite 中相关用户之间建立链接关系。

通过将 Azure AD 中“用户名”的值分配为 SilkRoad Life Suite 中“用户名”的值来建立此链接关系。

若要配置和测试 SilkRoad Life Suite 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-single-sign-on)** - 让用户能够使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 SilkRoad Life Suite 测试用户](#creating-a-silkroad-life-suite-test-user)** - 在 SilkRoad Life Suite 中创建 Britta Simon 的对应用户，并将其链接到她的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录
本部分旨在介绍如何在 Azure 经典门户中启用 Azure AD SSO 并在 SilkRoad Life Suite 应用程序中配置 SSO。

**若要配置 SilkRoad Life Suite 的 Azure AD 单一登录，需要以下项：**

1. 以管理员身份登录 SilkRoad 公司站点。 

  >[!NOTE] 
  > 若要获取对 SilkRoad Life Suite 身份验证应用程序的访问权限以配置 Microsoft Azure AD 的联合身份验证，请联系 SilkRoad 支持或 SilkRoad 服务代表。
  > 

2. 转到“服务提供商”，并单击“联合身份验证详细信息”。 
   
    ![Azure AD 单一登录][10] 

3. 单击“下载联合元数据”，并在计算机上保存该元数据文件。
   
    ![Azure AD 单一登录][11] 

4. 在 Azure 经典门户的“SilkRoad Life Suite”应用程序集成页上，单击“配置单一登录”，打开“配置单一登录”对话框。
   
    ![配置单一登录][6] 

5. 在“你希望用户如何登录 SilkRoad Life Suite”页上，选择“Azure AD 单一登录”，并单击“下一步”。
   
    ![Azure AD 单一登录][7] 

6. 在“配置应用设置”对话框页上，执行以下步骤：
   
    ![Azure AD 单一登录][8]   
 1. 在“登录 URL”文本框中，键入用户用于登录 SilkRoad Life Suite 站点的 URL（例如：*https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*）。  
 2. 打开下载的 **Silkroad** 元数据文件。 
 3. 找到 **AssertionConsumerService** 标记，并复制 **Location** 属性。         
   
    ![Azure AD 单一登录][21] 
 4. 将值粘贴到“回复 URL”文本框中。  
 5. 单击“下一步”。

6. 在“配置 SilkRoad Life Suite 的单一登录”页上，执行以下步骤：
   
    ![Azure AD 单一登录][9]  
 1. 单击“下载证书”，然后将文件保存在计算机上。  
 2. 单击“下一步”。

7. 在 **SilkRoad** 应用程序中，单击“身份验证源”。
   
    ![Azure AD 单一登录][12] 

8. 单击“添加身份验证源”。 
   
    ![Azure AD 单一登录][13] 

9. 在“添加身份验证源”部分中，执行以下步骤： 
   
    ![Azure AD 单一登录][14]  
 1. 在“选项 2 - 元数据文件”下，单击“浏览”以上传已下载的元数据文件。  
 2. 单击“使用文件数据创建标识提供者”。

10. 在“身份验证源”部分中，单击“编辑”。 
    
     ![Azure AD 单一登录][15] 

11. 在“编辑身份验证源”对话框中，执行以下步骤： 
    
     ![Azure AD 单一登录][16] 
 1. 对于“启用”，请选择“是”。   
 2. 在“IdP 说明”文本框中，键入配置说明（例如：*Azure AD SSO*）。  
 3. 在“IdP 名称”文本框中，键入特定于配置的名称（例如：*Azure SP*）。  
 4. 单击“保存” 。

12. 禁用所有其他身份验证源。 
    
     ![Azure AD 单一登录][17]

13. 在 Azure 经典门户中的“单一登录确认”页上，单击“下一步”。  
    
     ![Azure AD 单一登录][18]

14. 在“单一登录确认”页上，单击“完成”。
    
     ![Azure AD 单一登录][19]

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 经典门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][20]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 经典门户**中，在左侧导航窗格上，单击“Active Directory”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. 在“目录”列表中，选择要启用目录集成的目录。

3. 若要显示用户列表，请在顶部菜单中，单击“用户”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. 若要打开“添加用户”对话框，请在底部工具栏中单击“添加用户”。 
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. 在“告诉我们有关此用户的信息”对话框页上，执行以下步骤： 
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. 对于“用户类型”，选择“组织中的新用户”。  
 2. 在“用户名”文本框中，键入“BrittaSimon”。 
 3. 单击“下一步”。

6. 在“用户配置文件”对话框页上，执行以下步骤： 
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. 在“名字”文本框中，键入“Britta”。    
 2. 在“姓氏”文本框中，键入“Simon”。 
 3. 在“显示名称”文本框中，键入“Britta Simon”。 
 4. 在“角色”列表中，选择“用户”。
 5. 单击“下一步”。

7. 在“获取临时密码”对话框页上，单击“创建”。
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. 在“获取临时密码”对话框页上，执行以下步骤：
   
    ![创建 Azure AD 测试用户](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. 写下“新密码”的值。 
 2. 单击“完成”。   

### <a name="create-a-silkroad-life-suite-test-user"></a>创建 SilkRoad Life Suite 测试用户
本部分的目的是在 SilkRoad Life Suite 中创建名为“Britta Simon”的用户。 Britta 的 SSO ID（有时称为 *AuthParam*）必须与 Azure AD 中 Britta 的 **emailaddress** 匹配。

**若要在 SilkRoad Life Suite 中创建名为“Britta Simon”的用户，请执行以下步骤：**

- 要求 SilkRoad Life Suite 支持团队创建一个用户，该用户的 **SSO ID** 属性值与 Azure AD 中 Britta Simon 的 **emailaddress** 相同。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户
本部分旨在通过授予 Britta Simon 访问 SilkRoad Life Suite 的权限，允许她使用 Azure SSO。

![分配用户][200] 

**要将 Britta Simon 分配到 SilkRoad Life Suite，请执行以下步骤：**

1. 在 Azure 经典门户中，若要打开应用程序视图，请在目录视图的顶部菜单中，单击“应用程序”。
   
    ![分配用户][201] 

2. 在应用程序列表中，选择“SilkRoad Life Suite”。
   
    ![分配用户][202] 

3. 在顶部菜单中，单击“用户”。
   
    ![分配用户][203] 

4. 在“用户”列表中，选择“Britta Simon”。

5. 在底部工具栏中，单击“分配”。
   
    ![分配用户][205]

### <a name="test-single-sign-on"></a>测试单一登录
本部分旨在使用“访问面板”测试 Azure AD SSO 配置。  

单击访问面板中的“SilkRoad Life Suite”磁贴时，用户应自动登录到 SilkRoad Life Suite 应用程序。

## <a name="additional-resources"></a>其他资源
* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](active-directory-saas-tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





