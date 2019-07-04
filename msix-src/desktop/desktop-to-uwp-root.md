---
Description: Crie um pacote de aplicativo do Windows moderno para seu app ou jogo Win32, WPF ou Windows Forms. Adicionar experiências modernas para usuários do Windows 10 e simplificar a implantação e a monetização.
title: Aplicativos da área de trabalho do pacote
ms.date: 09/05/2018
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: Windows 10, UWP, MSIX
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 388de972a2ac97418e2f684c57c7aff550292b9e
ms.sourcegitcommit: 52010495873758d9bfe7a9fb0b240108b25b3d3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555503"
---
# <a name="package-desktop-applications-desktop-bridge"></a>Aplicativos da área de trabalho do pacote (ponte de Desktop)

Levar seu aplicativo de área de trabalho existente e adicionar experiências modernas para usuários do Windows 10. Em seguida, obtenha maior alcance em todos os mercados internacionais ao distribuí-lo por meio da Microsoft Store. Você pode monetizar seu aplicativo de maneiras muito mais simples, aproveitando os recursos incorporados à direita de Store. Obviamente, você não precisa usar o Store. Fique à vontade para usar seus canais existentes.

Quando você cria um pacote para seu aplicativo da área de trabalho, o aplicativo receberá uma identidade e com essa identidade, seu aplicativo da área de trabalho tem acesso ao Windows UWP (plataforma Universal) APIs. Você pode usá-las para facilitar experiências envolventes e modernas, como blocos dinâmicos e notificações. Usar a compilação condicional simple e verificações de tempo de execução para executar o código UWP somente quando seu aplicativo é executado no Windows 10.

O código que você pode usar para acender experiências com o Windows 10, além de seu aplicativo permanece inalterado e você pode continuar para distribuí-lo aos usuários nas versões anteriores do Windows. No Windows 10, seu aplicativo continua em execução em confiança total modo de usuário, como ele está fazendo hoje mesmo.

## <a name="prepare"></a>Preparar

Primeiro, prepare seu aplicativo examinando o artigo [preparar para empacotar seu aplicativo de desktop](desktop-to-uwp-prepare.md)e, em seguida, endereçamento de qualquer um dos problemas que se aplicam ao seu aplicativo antes de criar um pacote de aplicativo do Windows para ele. Talvez você não precise fazer muitas alterações ao seu aplicativo antes de criar o pacote. No entanto, há algumas situações que podem exigir que você ajustar seu aplicativo antes de criar um pacote para ele.

<a id="convert" />

## <a name="package"></a>Pacote

Há várias maneiras diferentes de criar um pacote MSIX para seu aplicativo de desktop.

### <a name="build-an-msix-from-an-existing-app-installer"></a>Criar um MSIX de um instalador de aplicativo existente

Se você já tiver um pacote do aplicativo (por exemplo, um MSI ou o instalador do App-V), é recomendável que você use o [ferramenta de empacotamento MSIX](../mpt-overview.md) reempacotar seu aplicativo de área de trabalho existente para o formato MSIX. Ela oferece uma interface do usuário interativa e uma linha de comando para conversões e proporciona a capacidade de converter um aplicativo sem a necessidade do código-fonte. 

Uma ferramenta anterior chamada a [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) também ainda está disponível para reempacotamento de um pacote de aplicativo da área de trabalho existente. No entanto, essa ferramenta foi preterida e é recomendável que você use a ferramenta de empacotamento MSIX.

A tabela a seguir mostra as versões compatíveis do Windows 10 e o pacote de formatos para essas ferramentas.

|  Ferramenta  | Versões de sistema operacional com suporte para a criação de pacotes  | Versões de sistema operacional com suporte para pacotes instalados  |  Pacote de formatos com suporte  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  [Ferramenta de empacotamento MSIX](../mpt-overview.md)        |  Windows 10, versão 1809 e posterior           | Windows 10, versão 1709 e posterior            |  .msix apenas                 |
|  [Desktop App Converter de](desktop-to-uwp-run-desktop-app-converter.md)        |  Windows 10, versão 1607 e posteriores           | Windows 10, versão 1607 e posteriores            |  somente. AppX    |

Para começar a usar a ferramenta de empacotamento MSIX, consulte estes artigos:

* [Criar um pacote MSIX de um instalador da área de trabalho (MSI, EXE ou App-V) em uma VM](../packaging-tool/create-app-package-msi-vm.md)
* [Criar um pacote MSIX com outro instalador todos os tipos](../packaging-tool/create-other-installer.md)
* [Criar um pacote MSIX usando a linha de comando](../packaging-tool/package-conversion-cli.md)

### <a name="build-an-msix-from-source-code-using-visual-studio"></a>Criar um MSIX do código-fonte usando o Visual Studio

Se você mantém seu aplicativo usando o Visual Studio e esse aplicativo não tem um instalador ou seu instalador não realiza tarefas complicadas demais, considere usar o Visual Studio no lugar.

Visual Studio torna fácil criar um pacote. Você adicionará uma **projeto de pacote de aplicativo do Windows** à sua solução, fazer referência a seu projeto da área de trabalho e, em seguida, pressione F5 para depurar seu aplicativo. Aqui estão algumas outras coisas que você pode fazer com ele.

:heavy_check_mark: Gere automaticamente os ativos visuais.

:heavy_check_mark: Fazer alterações ao seu manifesto usando um designer visual.

:heavy_check_mark: Gere seu pacote usando um assistente.

:heavy_check_mark: Atribuir facilmente uma identidade para seu aplicativo de um nome que você reservou já [Partner Center](https://partner.microsoft.com/dashboard).

Para obter instruções, consulte [empacotar um aplicativo de desktop usando o Visual Studio](desktop-to-uwp-packaging-dot-net.md). A tabela a seguir mostra as versões com suporte do Visual Studio, Windows 10 e formatos do pacote.

|  Versões com suporte do Visual Studio | Versões de sistema operacional com suporte para a criação de pacotes  | Versões de sistema operacional com suporte para pacotes instalados  |  Pacote de formatos com suporte  |
|-----------------------------|-----------------------------------------------|-----------------------------------------------|-----------------------------|
|  Visual Studio 2019<br/>Visual Studio 2017 15.5 e posterior       |  Windows 10, versão 1607 e posteriores           |  Windows 10, versão 1607 e posteriores            |  .msix (para Windows 10, versão 1709 e posterior)<br/>. AppX (para Windows 10, versão 1607 e posterior)                 |

### <a name="third-party-installers"></a>Instaladores de terceiros

Vários produtos de terceiros populares e instaladores agora dão suporte a capacidade de empacotar um aplicativo da área de trabalho. Para obter mais detalhes e uma lista desses produtos de terceiros, consulte [empacotar um aplicativo da área de trabalho usando os instaladores de terceiros](desktop-to-uwp-third-party-installer.md).

### <a name="manual-packaging"></a>Empacotamento manual

Como uma opção final, você pode converter seu aplicativo sem usar qualquer uma dessas ferramentas. Se você deseja um controle granular sobre sua conversão, você pode criar um arquivo de manifesto e, em seguida, executar o **MakeAppx.exe** para criar seu pacote de aplicativo do Windows.

Ver [empacotar um aplicativo de área de trabalho manualmente](desktop-to-uwp-manual-conversion.md).

## <a name="add-modern-windows-10-experiences"></a>Adicionar experiências modernas do Windows 10

Depois de criar um pacote MSIX para seu aplicativo da área de trabalho, você pode usar as APIs da UWP, extensões de pacote, e componentes UWP para acender modernas e envolventes experiências do Windows 10, como blocos dinâmicos e notificações.

### <a name="enhance-with-uwp-apis"></a>Aprimorar com APIs de UWP

Depois de ter empacotado seu app, você poderá aprimorá-lo com recursos como blocos dinâmicos e notificações por push. Alguns desses recursos podem melhorar significativamente o nível de engajamento do seu aplicativo e eles custam muito pouco tempo para adicionar. Alguns aprimoramentos exigem um pouco mais de código.

Ver [Use as APIs de UWP em aplicativos da área de trabalho](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

### <a name="integrate-with-package-extensions"></a>Integrar com extensões de pacote

Se seu aplicativo precisa integrar o sistema (por exemplo: estabelecer regras de firewall), descrevem essas coisas no manifesto do pacote do aplicativo e o sistema fará o resto. Para a maioria dessas tarefas, você não precisará escrever qualquer código. Com um pouco de XML no manifesto, você pode fazer coisas como iniciar um processo quando o usuário fizer logon, integrar seu aplicativo no Explorador de arquivos e adicionar seu aplicativo uma lista de destinos de impressão que aparecem em outros aplicativos.

Ver [integrar seu aplicativo da área de trabalho com extensões do pacote](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions).

### <a name="extend-with-uwp-components"></a>Estender com componentes UWP

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de app moderno. Em geral, você deve primeiro determinar se pode adicionar sua experiência por [Aprimoramento](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance) do seu aplicativo da área de trabalho existente com APIs UWP. Se você tiver que usar um componente UWP, obter a experiência, adicione um projeto UWP à sua solução e usar os serviços de aplicativo para se comunicar entre o aplicativo da área de trabalho e o componente UWP.

Ver [estender seu aplicativo da área de trabalho com componentes UWP](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend).

## <a name="test"></a>Testar

Para testar seu aplicativo em uma configuração realista enquanto se prepara para distribuição, é melhor assinar seu aplicativo e, em seguida, instalá-lo. Consulte [Testar seu app](desktop-to-uwp-debug.md#test-your-app).

>[!IMPORTANT]
> Se você planeja publicar seu aplicativo para a Microsoft Store, certifique-se de que seu aplicativo funcione corretamente em dispositivos que executam o Windows 10 no modo S. Este é um requisito da Store. Veja [Testar seu aplicativo do Windows para o Windows 10 no modo S](desktop-to-uwp-test-windows-s.md).

## <a name="validate"></a>Validar

Para fornecer a melhor chance de ser publicado em que a Microsoft Store ou torna-se ao seu aplicativo [Windows Certified](https://go.microsoft.com/fwlink/p/?LinkID=309666), validar e testá-lo localmente antes de enviá-lo para certificação.

Se você estiver usando o DAC para empacotar seu aplicativo, você pode usar o novo ``-Verify`` sinalizador para validar seu pacote contra os requisitos de Store e o aplicativo de área de trabalho de pacote. Ver [empacotar um aplicativo, assinar o aplicativo e prepará-lo para envio de Store](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters).

Se você estiver usando o Visual Studio, você pode validar seu aplicativo a partir de **criar pacotes de aplicativos** assistente. Consulte [Criar um arquivo de upload de pacote do aplicativo](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps#create-an-app-package-upload-file).

Para executar a ferramenta manualmente, consulte [Kit de Certificação de Aplicativos Windows](/windows/uwp/debug-test-perf/windows-app-certification-kit).

Para revisar a lista de testes que a certificação de aplicativo do Windows usa para validar seu aplicativo, consulte [testes de aplicativo da ponte de desktop do Windows](/windows/uwp/debug-test-perf/windows-desktop-bridge-app-tests).

## <a name="distribute"></a>Distribuir

Você pode distribuir seu aplicativo ao publicá-la a Microsoft Store ou fazendo o sideload-lo em outros sistemas.

Ver [distribuir um aplicativo de área de trabalho empacotado](/windows/apps/desktop/modernize/desktop-to-uwp-distribute).

## <a name="support-and-feedback"></a>Suporte e comentários

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
