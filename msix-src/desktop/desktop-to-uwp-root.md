---
description: Este artigo descreve o processo de ponta a ponta para criar um pacote de aplicativo do Windows 10 moderno para seu aplicativo Windows Forms, WPF ou Win32 existente ou jogo.
title: Aplicativos da área de trabalho do pacote
ms.date: 07/29/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: fc70cab0074087d7ae2d2ea123ed9b41bfdac8ea
ms.sourcegitcommit: d749fa662214bddaa6854f1ee95761c547db8dae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2019
ms.locfileid: "75008086"
---
# <a name="package-desktop-applications-msix"></a>Aplicativos de desktop de pacote (MSIX)

Pegue seu aplicativo de área de trabalho existente e adicione experiências modernas para usuários do Windows 10. Em seguida, obtenha maior alcance em todos os mercados internacionais ao distribuí-lo por meio da Microsoft Store. Você pode monetizar seu aplicativo de maneiras muito mais simples, aproveitando os recursos criados diretamente na loja. É claro que você não precisa usar a loja. Fique à vontade para usar seus canais existentes.

Quando você cria um pacote MSIX para seu aplicativo de área de trabalho, seu aplicativo receberá uma identidade e com essa identidade, seu aplicativo de área de trabalho terá acesso às APIs da UWP (plataforma universal do Windows). Você pode usá-las para facilitar experiências envolventes e modernas, como blocos dinâmicos e notificações. Use a compilação condicional simples e as verificações de tempo de execução para executar o código UWP somente quando seu aplicativo for executado no Windows 10.

Além do código que você usa para aprimorar as experiências do Windows 10, seu aplicativo permanece inalterado e você pode continuar a distribuí-lo aos usuários em versões anteriores do Windows. No Windows 10, seu aplicativo continua a ser executado no modo de usuário de confiança total, assim como está fazendo hoje.

## <a name="prepare"></a>Preparar

Primeiro, prepare seu aplicativo examinando o artigo [preparar para empacotar seu aplicativo de desktop](desktop-to-uwp-prepare.md)e, em seguida, solucionar qualquer um dos problemas que se aplicam ao seu aplicativo antes de criar um pacote de aplicativo do Windows para ele. Talvez você não precise fazer muitas alterações em seu aplicativo antes de criar o pacote. No entanto, há algumas situações que podem exigir que você ajuste seu aplicativo antes de criar um pacote para ele.

<a id="convert" />

## <a name="package"></a>Pacote

Há várias maneiras diferentes de criar um pacote MSIX para seu aplicativo de desktop.

### <a name="build-an-msix-from-an-existing-app-installer"></a>Criar um MSIX de um instalador de aplicativo existente

Se você já tiver um pacote do aplicativo (por exemplo, um MSI ou o instalador do App-V), recomendamos que você use a [ferramenta de empacotamento MSIX](../mpt-overview.md) para reempacotar seu aplicativo de área de trabalho existente no formato MSIX. Ela oferece uma interface do usuário interativa e uma linha de comando para conversões e proporciona a capacidade de converter um aplicativo sem a necessidade do código-fonte. 

Uma ferramenta anterior chamada o [conversor do aplicativo para desktop](desktop-to-uwp-run-desktop-app-converter.md) também está disponível para reempacotar um pacote de aplicativo de desktop existente. No entanto, essa ferramenta foi preterida e recomendamos que você use a ferramenta de empacotamento MSIX em vez disso.

A tabela a seguir mostra as versões com suporte do Windows 10 e os formatos de pacote para essas ferramentas.

|  Ferramenta  | Versões do sistema operacional com suporte para a criação de pacotes  | Versões do sistema operacional com suporte para pacotes instalados  |  Formatos de pacote com suporte  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  [Ferramenta de empacotamento MSIX](../mpt-overview.md)        |  Windows 10, versão 1809 e posterior           | Windows 10, versão 1709 e posterior            |  . msix somente                 |
|  [Conversor de aplicativo de desktop](desktop-to-uwp-run-desktop-app-converter.md)        |  Windows 10, versão 1607 e posteriores           | Windows 10, versão 1607 e posteriores            |  somente Appx    |

Para começar a usar a ferramenta de empacotamento MSIX, consulte estes artigos:

* [Criar um pacote MSIX de um instalador de desktop (MSI, EXE ou App-V) em uma VM](../packaging-tool/create-app-package-msi-vm.md)
* [Criar um pacote MSIX com todos os outros tipos de instalador](../packaging-tool/create-other-installer.md)
* [Criar um pacote MSIX usando a linha de comando](../packaging-tool/package-conversion-cli.md)

### <a name="build-an-msix-from-source-code-using-visual-studio"></a>Criar um MSIX a partir do código-fonte usando o Visual Studio

Se você mantém seu aplicativo usando o Visual Studio e esse aplicativo não tem um instalador ou seu instalador não realiza tarefas complicadas demais, considere usar o Visual Studio no lugar.

O Visual Studio facilita a criação de um pacote. Você adicionará um **projeto de pacote de aplicativos do Windows** à sua solução, fará referência ao seu projeto de desktop e, em seguida, pressionará F5 para depurar seu aplicativo. Aqui estão algumas outras coisas que você pode fazer com ele.

:heavy_check_mark: Gerar automaticamente ativos visuais.

:heavy_check_mark: Fazer alterações no seu manifesto usando um designer visual.

:heavy_check_mark: Gerar seu pacote usando um assistente.

: heavy_check_mark: atribua facilmente uma identidade ao seu aplicativo a partir de um nome que você já reservou no [Partner Center](https://partner.microsoft.com/dashboard).

Para obter instruções, confira [empacotar um aplicativo de área de trabalho usando o Visual Studio](desktop-to-uwp-packaging-dot-net.md). A tabela a seguir mostra as versões com suporte do Visual Studio, do Windows 10 e dos formatos de pacote.

|  Versões com suporte do Visual Studio | Versões do sistema operacional com suporte para a criação de pacotes  | Versões do sistema operacional com suporte para pacotes instalados  |  Formatos de pacote com suporte  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  Visual Studio 2019<br/>Visual Studio 2017 15,5 e posterior       |  Windows 10, versão 1607 e posteriores           |  Windows 10, versão 1607 e posteriores            |  . msix (para Windows 10, versão 1709 e posterior)<br/>. Appx (para Windows 10, versão 1607 e posterior)                 |

### <a name="third-party-installers"></a>Instaladores de terceiros

Vários produtos e instaladores de terceiros populares agora oferecem suporte à capacidade de empacotar um aplicativo de área de trabalho. Para obter mais detalhes e uma lista desses produtos de terceiros, confira [empacotar um aplicativo de área de trabalho usando instaladores de](desktop-to-uwp-third-party-installer.md)terceiros.

### <a name="manual-packaging"></a>Empacotamento manual

Como uma opção final, você pode converter seu aplicativo sem usar nenhuma dessas ferramentas. Se você deseja um controle granular sobre sua conversão, você pode criar um arquivo de manifesto e, em seguida, executar o **MakeAppx.exe** para criar seu pacote de aplicativo do Windows.

Confira [empacotar um aplicativo de área de trabalho manualmente](desktop-to-uwp-manual-conversion.md).

## <a name="add-modern-windows-10-experiences"></a>Adicionar experiências modernas do Windows 10

Depois de criar um pacote MSIX para seu aplicativo de desktop, você pode usar APIs, extensões de pacote e componentes UWP do UWP para iluminar experiências modernas e envolventes do Windows 10, como blocos dinâmicos e notificações.

### <a name="enhance-with-uwp-apis"></a>Aprimore com as APIs UWP

Depois de ter empacotado seu app, você poderá aprimorá-lo com recursos como blocos dinâmicos e notificações por push. Alguns desses recursos podem melhorar significativamente o nível de envolvimento do seu aplicativo e eles custam muito pouco tempo para serem adicionados. Alguns aprimoramentos exigem um pouco mais de código.

Consulte [usar APIs UWP em aplicativos de área de trabalho](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

### <a name="integrate-with-package-extensions"></a>Integrar com extensões de pacote

Se seu aplicativo precisar ser integrado ao sistema (por exemplo: estabelecer regras de firewall), descreva as coisas no manifesto do pacote do seu aplicativo e o sistema fará o restante. Para a maioria dessas tarefas, você não precisará escrever qualquer código. Com um pouco de XML no manifesto, você pode fazer coisas como iniciar um processo quando o usuário faz logon, integrar seu aplicativo no explorador de arquivos e adicionar seu aplicativo a uma lista de destinos de impressão que aparecem em outros aplicativos.

Consulte [integrar seu aplicativo de área de trabalho com extensões de pacote](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions).

### <a name="extend-with-uwp-components"></a>Estender com componentes UWP

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de app moderno. Em geral, você deve primeiro determinar se pode adicionar sua experiência por [Aprimoramento](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance) do seu aplicativo da área de trabalho existente com APIs UWP. Se você precisar usar um componente UWP, para obter a experiência, poderá adicionar um projeto UWP à sua solução e usar os serviços de aplicativos para se comunicar entre o aplicativo da área de trabalho e o componente UWP.

Consulte [estender seu aplicativo de área de trabalho com componentes UWP](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend).

## <a name="test"></a>Testar

Para testar seu aplicativo em uma configuração realista enquanto você se prepara para a distribuição, é melhor assinar seu aplicativo e instalá-lo. Consulte [Testar seu app](desktop-to-uwp-debug.md#test-your-app).

>[!IMPORTANT]
> Se você planeja publicar seu aplicativo no Microsoft Store, certifique-se de que seu aplicativo opere corretamente em dispositivos que executam o Windows 10 no modo S. Esse é um requisito de armazenamento. Veja [Testar seu aplicativo do Windows para o Windows 10 no modo S](desktop-to-uwp-test-windows-s.md).

## <a name="validate"></a>Validar

Para dar ao seu aplicativo a melhor chance de ser publicado no Microsoft Store ou se tornar o [Windows certificado](https://go.microsoft.com/fwlink/p/?LinkID=309666), valide-o e teste-o localmente antes de enviá-lo para certificação.

Se você estiver usando o DAC para empacotar seu aplicativo, poderá usar o novo sinalizador ``-Verify`` para validar seu pacote em relação ao aplicativo de área de trabalho empacotado e armazenar os requisitos. Consulte [empacotar um aplicativo, assinar o aplicativo e prepará-lo para envio da loja](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters).

Se você estiver usando o Visual Studio, poderá validar seu aplicativo do assistente para **criar pacotes de aplicativos** . Consulte [Criar um arquivo de upload de pacote do aplicativo](../package/packaging-uwp-apps.md#create-an-app-package-upload-file).

Para executar a ferramenta manualmente, consulte [Kit de Certificação de Aplicativos Windows](/windows/uwp/debug-test-perf/windows-app-certification-kit).

Para revisar a lista de testes que a certificação de aplicativo do Windows usa para validar seu aplicativo, consulte [testes de aplicativo da ponte de desktop do Windows](/windows/uwp/debug-test-perf/windows-desktop-bridge-app-tests).

## <a name="distribute"></a>Distribuir

Você pode distribuir seu aplicativo publicando-o na Microsoft Store ou fazendo o Sideload para outros sistemas.

Consulte [distribuir um aplicativo de área de trabalho empacotado](/windows/apps/desktop/modernize/desktop-to-uwp-distribute).

## <a name="support-and-feedback"></a>Suporte e comentários

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fornecer comentários ou fazer sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
