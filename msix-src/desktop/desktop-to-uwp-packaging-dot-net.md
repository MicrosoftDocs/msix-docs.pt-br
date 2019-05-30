---
Description: Este guia explica como configurar sua solução do Visual Studio para editar, depurar e empacotar o aplicativo da área de trabalho.
title: Empacotar um aplicativo de desktop usando o Visual Studio
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: beb5096263aaae2b2bd8a32b9417c8af6891a5e3
ms.sourcegitcommit: 958d9e8177036c8485fb79b49f35cb2deb8d553c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308445"
---
# <a name="package-a-desktop-application-by-using-visual-studio"></a>Empacotar um aplicativo de desktop usando o Visual Studio

Você pode usar o Visual Studio para gerar um pacote para seu aplicativo da área de trabalho. Em seguida, você pode publicar esse pacote para a Microsoft Store ou fazer sideload-lo em um ou mais computadores.

A versão mais recente do Visual Studio fornece uma nova versão do projeto empacotamento que elimina todas as etapas manuais que costumavam ser necessárias para empacotar seu aplicativo. Basta adicionar um projeto de empacotamento, referenciar seu projeto de desktop e depois pressionar F5 para depurar seu aplicativo. Sem ajustes manuais. Essa nova experiência simplificada é uma grande melhoria em relação à experiência disponível na versão anterior do Visual Studio.

>[!IMPORTANT]
>A capacidade de criar um pacote de aplicativo do Windows para o seu aplicativo da área de trabalho (também conhecido como a ponte de Desktop) foi introduzida no Windows 10, versão 1607, e só pode ser usado em projetos que se destinam a atualização de aniversário do Windows 10 (10.0; Build 14393) ou uma versão posterior no Visual Studio.

## <a name="first-prepare-your-application"></a>Primeiro, prepare seu aplicativo

Examine este guia antes de começar a criar um pacote para seu aplicativo: [Preparar para empacotar um aplicativo da área de trabalho](desktop-to-uwp-prepare.md).

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>Crie um pacote

1. No Visual Studio, abra a solução que contém seu projeto de aplicativo da área de trabalho.

2. Adicione um **Projeto de empacotamento de aplicativo do Windows** à sua solução.

   Você não terá que adicionar nenhum código a ele. Ele existe apenas para gerar um pacote para você. Vamos nos referir a esse projeto como o "projeto de empacotamento".

   ![Projeto de empacotamento](images/packaging-project.png)

   >[!NOTE]
   >Este projeto aparece apenas no Visual Studio 2017 versão 15.5 ou superior.

3. Defina a **Versão de destino** desse projeto como qualquer versão que deseja, mas certifique-se de definir a **Versão mínima** como **Atualização de Aniversário do Windows 10**.

   ![Caixa de diálogo do seletor de versão de empacotamento](images/packaging-version.png)

4. No projeto de empacotamento, clique com botão direito na pasta **Aplicativos** e escolha **Adicionar referência**.

   ![Adicionar referência de projeto](images/add-project-reference.png)

5. Escolha seu projeto de aplicativo da área de trabalho e, em seguida, escolha o botão **OK**.

   ![Projeto de desktop](images/reference-project.png)

   Você pode incluir vários aplicativos da área de trabalho em seu pacote, mas apenas um deles é iniciado quando os usuários escolhem o bloco do seu aplicativo. No nó **Aplicativos**, clique com botão direito no aplicativo que você quer que os usuários iniciem ao escolherem o bloco do aplicativo e escolha **Definir como ponto de entrada**.

   ![Definir ponto de entrada](images/entry-point-set.png)

6. Crie o projeto de empacotamento para garantir que nenhum erro apareça.  Se você receber erros, abra **Configuration Manager** e certifique-se de que seus projetos direcionados a mesma plataforma.

   ![Gerenciador de configuração](images/config-manager.png)

7. Use o assistente [Criar pacotes de aplicativo](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps) para gerar um arquivo appxupload.

   Você pode carregá-lo diretamente para a Store.

**Vídeo**

&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Executar, depurar ou testar seu aplicativo da área de trabalho**

Consulte [executar, depurar e testar um aplicativo da área de trabalho de pacote](desktop-to-uwp-debug.md)

**Aprimore seu aplicativo da área de trabalho com a adição de APIs de UWP**

Consulte [Aprimorar seu aplicativo da área de trabalho para Windows 10](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)

**Estender seu aplicativo da área de trabalho adicionando projetos UWP e componentes de tempo de execução do Windows**

Consulte [Estender seu aplicativo da área de trabalho com componentes UWP modernos](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend).

**Distribuir seu aplicativo**

Consulte [distribuir um aplicativo de área de trabalho do pacote](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-distribute)
