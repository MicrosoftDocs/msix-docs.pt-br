---
Description: Este guia explica como configurar a Solução do Visual Studio para editar, depurar e empacotar o aplicativo da área de trabalho.
title: Empacotar um aplicativo da área de trabalho no código-fonte usando o Visual Studio
ms.date: 02/02/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: d8f2e1dbf6a9d40ec9e913088a6aa8e8f1c039b4
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77072756"
---
# <a name="set-up-your-desktop-application-for-msix-packaging-in-visual-studio"></a>Configurar o aplicativo da área de trabalho para empacotamento MSIX no Visual Studio

Você pode usar o projeto do **Projeto de Empacotamento de Aplicativo do Windows** no Visual Studio para gerar um pacote para o aplicativo de área de trabalho. Em seguida, é possível publicar o pacote na Microsoft Store ou fazer sideload dele em um ou mais computadores.

Um projeto do **Projeto de Empacotamento de Aplicativo do Windows** está disponível nas versões a seguir do Visual Studio. Para ter a melhor experiência, recomendamos usar a versão mais recente.

* Visual Studio 2019
* Visual Studio 2017 15.5 e posterior

> [!IMPORTANT]
> Um projeto do **Projeto de Empacotamento de Aplicativo do Windows** no Visual Studio é compatível com o Windows 10, versão 1607 e posteriores. Ele só pode ser usado em projetos que visam a Atualização de Aniversário do Windows 10 (10.0; Build 14393) ou uma versão posterior.

Estas são outras coisas que você pode fazer pelo Projeto de Empacotamento de Aplicativo do Windows:

:heavy_check_mark: Gerar ativos visuais automaticamente.

:heavy_check_mark: Faça alterações no manifesto usando um designer virtual.

:heavy_check_mark: Gere o pacote usando um assistente.

:heavy_check_mark: (Caso vá publicar na Microsoft Store) Atribua com facilidade uma identidade ao aplicativo com um nome já reservado no [Partner Center](https://partner.microsoft.com/dashboard).


## <a name="prepare-your-application"></a>Prepare seu aplicativo

Examine este guia antes de começar a criar um pacote para o aplicativo: [Preparar um pacote de um aplicativo de área de trabalho](desktop-to-uwp-prepare.md).

<a id="new-packaging-project"/>

## <a name="setup-the-windows-application-packaging-project-in-your-solution"></a>Configure o Projeto de Empacotamento de Aplicativo do Windows na solução

1. No Visual Studio, abra a solução que contém o projeto de aplicativo da área de trabalho.

2. Adicione um projeto do **Projeto de Empacotamento de Aplicativo do Windows** à solução.

   Não será necessário adicionar nenhum código. Ele está ali somente para gerar um pacote para você. Vamos nos referir a esse projeto como o "projeto de empacotamento".

   ![Projeto de empacotamento](images/packaging-project.png)

3. Defina a **Versão de Destino** desse projeto como qualquer versão desejada, mas assegure-se de definir a **Versão Mínima** para a **Atualização de Aniversário do Windows 10**.

   ![Caixa de diálogo do seletor de versão de empacotamento](images/packaging-version.png)

4. No Gerenciador de Soluções, clique com o botão direito do mouse na pasta **Aplicativos** no projeto do pacote e escolha **Adicionar Referência**.

   ![Adicionar Referência de Projeto](images/add-project-reference.png)

5. Escolha seu projeto de aplicativo da área de trabalho e o botão **OK**.

   ![Projeto de área de trabalho](images/reference-project.png)

   Você pode incluir vários aplicativos da área de trabalho em seu pacote, mas apenas um deles será iniciado quando os usuários escolhem o bloco do aplicativo. No nó **Aplicativos**, clique com botão direito do mouse no aplicativo que você quer que os usuários iniciem ao escolherem o bloco do aplicativo e escolha **Definir como Ponto de Entrada**.

   ![Definir ponto de entrada](images/entry-point-set.png)

6. Se o aplicativo sendo empacotado visa o .NET Core 3, siga estas etapas para adicionar um novo destino de build ao arquivo do projeto. Isso só é necessário em aplicativos que visam o .NET Core 3.  

    1. No Gerenciador de Soluções, clique com o botão direito do mouse no nó do projeto de empacotamento e selecione **Editar Arquivo de Projeto**.

    2. Localize o elemento `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` no arquivo.

    3. Substitua esse elemento pelo XML a seguir.

        ``` xml
        <ItemGroup>
          <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
          </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
          <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
              <SourceProject>
              </SourceProject>
            </_FilteredNonWapProjProjectOutput>
          </ItemGroup>
        </Target>
        ```

    4. Salve o arquivo do projeto e feche-o.

7. Crie o projeto de empacotamento para garantir que nenhum erro apareça. Se você receber erros, abra **Gerenciador de Configurações** e verifique se os projetos se destinam à mesma plataforma.

   ![Gerenciador de Configurações](images/config-manager.png)

8. Use o assistente de [Criar Pacotes de Aplicativos](../package/packaging-uwp-apps.md) para gerar um pacote/grupo MSIX ou um arquivo .msixupload/.appxupload (para publicação na Microsoft Store).


## <a name="next-steps"></a>Próximas etapas

**Empacote o aplicativo de área de trabalho no Visual Studio**

Veja [Empacotar um aplicativo UWP ou da área de trabalho no Visual Studio](../package/packaging-uwp-apps.md)

**Executar, depurar ou testar o aplicativo da área de trabalho**

Veja [Executar, depurar e testar um aplicativo empacotado](desktop-to-uwp-debug.md)

## <a name="additional-resources"></a>Recursos adicionais

**Vídeo**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

**Aprimorar o aplicativo de área de trabalho adicionando APIs da UWP**

Veja [Aprimorar o aplicativo de área de trabalho para o Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)

**Estenda o aplicativo da área de trabalho adicionando projetos UWP e Componentes do Windows Runtime**

Veja [Estender o aplicativo de área de trabalho com componentes UWP modernos](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend).

**Distribuir o aplicativo**

Veja [Distribuir um aplicativo de área de trabalho empacotado](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute).
