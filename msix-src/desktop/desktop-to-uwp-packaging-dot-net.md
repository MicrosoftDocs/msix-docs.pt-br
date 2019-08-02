---
Description: Este guia explica como configurar sua solução do Visual Studio para editar, depurar e empacotar o aplicativo de área de trabalho.
title: Empacotar um aplicativo da área de trabalho no código-fonte usando o Visual Studio
ms.date: 07/18/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: f97fc474aa9c25a381362a55120797d2a3c5131c
ms.sourcegitcommit: 6c28c590cd563ba69b2350e556dbd2ae55d9d7f4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68730400"
---
# <a name="package-a-desktop-app-from-source-code-using-visual-studio"></a>Empacotar um aplicativo da área de trabalho no código-fonte usando o Visual Studio

Você pode usar o projeto de projeto de empacotamento de **aplicativos do Windows** no Visual Studio para gerar um pacote para seu aplicativo de área de trabalho. Em seguida, você pode publicar esse pacote no Microsoft Store ou Sideload-lo em um ou mais computadores.

O projeto de projeto de empacotamento de **aplicativos do Windows** está disponível nas seguintes versões do Visual Studio. Para obter a melhor experiência, recomendamos que você use a versão mais recente.

* Visual Studio 2019
* Visual Studio 2017 15,5 e posterior

> [!IMPORTANT]
> O projeto de projeto de empacotamento de **aplicativos do Windows** no Visual Studio tem suporte no Windows 10, versão 1607 e posterior. Ele só pode ser usado em projetos que visam a atualização de aniversário do Windows 10 (10,0; Build 14393) ou uma versão posterior.

## <a name="prepare-your-application"></a>Preparar seu aplicativo

Examine este guia antes de começar a criar um pacote para seu aplicativo: [Prepare-se para empacotar um aplicativo de área de trabalho](desktop-to-uwp-prepare.md).

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>Crie um pacote

1. No Visual Studio, abra a solução que contém seu projeto de aplicativo da área de trabalho.

2. Adicione um **Projeto de empacotamento de aplicativo do Windows** à sua solução.

   Você não terá que adicionar nenhum código a ele. Ele existe apenas para gerar um pacote para você. Vamos nos referir a esse projeto como o "projeto de empacotamento".

   ![Projeto de empacotamento](images/packaging-project.png)

3. Defina a **Versão de destino** desse projeto como qualquer versão que deseja, mas certifique-se de definir a **Versão mínima** como **Atualização de Aniversário do Windows 10**.

   ![Caixa de diálogo do seletor de versão de empacotamento](images/packaging-version.png)

4. Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta **aplicativos** no projeto empacotamento e escolha **Adicionar referência**.

   ![Adicionar referência de projeto](images/add-project-reference.png)

5. Escolha seu projeto de aplicativo da área de trabalho e, em seguida, escolha o botão **OK**.

   ![Projeto de desktop](images/reference-project.png)

   Você pode incluir vários aplicativos da área de trabalho em seu pacote, mas apenas um deles é iniciado quando os usuários escolhem o bloco do seu aplicativo. No nó **Aplicativos**, clique com botão direito no aplicativo que você quer que os usuários iniciem ao escolherem o bloco do aplicativo e escolha **Definir como ponto de entrada**.

   ![Definir ponto de entrada](images/entry-point-set.png)

6. Se o aplicativo que você está empacotando for o .NET Core 3, siga estas etapas para adicionar um novo destino de compilação ao arquivo de projeto. Isso só é necessário para aplicativos direcionados ao .NET Core 3.  

    1. No Gerenciador de Soluções, clique com o botão direito do mouse no nó do projeto de empacotamento e selecione **Editar arquivo de projeto**.

    2. Localize o elemento `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` no arquivo.

    3. Substitua este elemento pelo XML a seguir.

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

    4. Salve o arquivo de projeto e feche-o.

7. Crie o projeto de empacotamento para garantir que nenhum erro apareça. Se você receber erros, abra **Configuration Manager** e verifique se seus projetos se destinam à mesma plataforma.

   ![Gerenciador de configuração](images/config-manager.png)

8. Use o assistente para [criar pacotes de aplicativos](../package/packaging-uwp-apps.md) para gerar um arquivo. msixupload/. appxupload.

   Você pode carregar esse arquivo diretamente no repositório.

**Vídeo**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fornecer comentários ou fazer sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Executar, depurar ou testar seu aplicativo de desktop**

Consulte [Executar, depurar e testar um aplicativo de área de trabalho empacotado](desktop-to-uwp-debug.md)

**Aprimore seu aplicativo de desktop adicionando APIs UWP**

Consulte [Aprimorar seu aplicativo da área de trabalho para Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)

**Estenda seu aplicativo de desktop adicionando projetos UWP e componentes Windows Runtime**

Consulte [Estender seu aplicativo da área de trabalho com componentes UWP modernos](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend).

**Distribua seu aplicativo**

Consulte [distribuir um aplicativo de área de trabalho empacotado](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute)
