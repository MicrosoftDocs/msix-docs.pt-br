---
Description: Crie um pacote de aplicativo do Windows moderno para seu app ou jogo Win32, WPF ou Windows Forms. Adicionar experiências modernas para usuários do Windows 10 e simplificar a implantação e a monetização.
title: Aplicativos da área de trabalho do pacote
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8fa9188ef078de6104c14cc983b8ce9549507096
ms.sourcegitcommit: c3bdc2150bba942dc95811746c7a0f14ce54fbc9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65985716"
---
# <a name="package-desktop-applications-desktop-bridge"></a>Aplicativos da área de trabalho do pacote (ponte de Desktop)

Levar seu aplicativo de área de trabalho existente e adicionar experiências modernas para usuários do Windows 10. Em seguida, obtenha maior alcance em todos os mercados internacionais ao distribuí-lo por meio da Microsoft Store. Você pode monetizar seu aplicativo de maneiras muito mais simples, aproveitando os recursos criados diretamente no repositório. Obviamente, você não precisa usar a loja. Fique à vontade para usar seus canais existentes.

![Ponte de Desktop](images/desktop-bridge-4.png)

Quando você cria um pacote para seu aplicativo da área de trabalho, o aplicativo receberá uma identidade e com essa identidade, seu aplicativo da área de trabalho tem acesso ao Windows UWP (plataforma Universal) APIs. Você pode usá-las para facilitar experiências envolventes e modernas, como blocos dinâmicos e notificações. Usar a compilação condicional simple e verificações de tempo de execução para executar o código UWP somente quando seu aplicativo é executado no Windows 10.

O código que você pode usar para acender experiências com o Windows 10, além de seu aplicativo permanece inalterado e você pode continuar para distribuí-lo para seu existente do Windows 7, Windows Vista ou base de usuários do Windows XP. No Windows 10, seu aplicativo continua em execução em confiança total modo de usuário, como ele está fazendo hoje mesmo.

>[!IMPORTANT]
>A capacidade de criar um pacote de aplicativo do Windows para o seu aplicativo da área de trabalho (também conhecido como a ponte de Desktop) foi introduzida no Windows 10, versão 1607, e só pode ser usado em projetos que se destinam a atualização de aniversário do Windows 10 (10.0; Build 14393) ou uma versão posterior no Visual Studio.

> [!NOTE]
> Confira <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">essa série</a> de vídeos de curta duração publicados na Microsoft Virtual Academy. Esses vídeos percorrer todo o processo de colocar seu aplicativo da área de trabalho para Universal Windows Platform (UWP).

## <a name="benefits"></a>Benefícios

Aqui estão alguns motivos para criar um pacote de aplicativo do Windows para seu aplicativo da área de trabalho:

:heavy_check_mark: **Implantação simplificada**. Aplicativos e jogos que usam a ponte têm uma ótima experiência de implantação. Essa experiência garante que os usuários podem instalar um aplicativo com confiança e atualizá-lo. Se um usuário optar por desinstalar o aplicativo, ele será removido por completo sem deixar rastreamento. Isso reduz o tempo para criar experiências de instalação e manter os usuários atualizados.

:heavy_check_mark: **Atualizações automáticas e licenciamento**. Seu aplicativo pode participar de licenciamento interno do a Microsoft Store e as instalações de atualização automática. A atualização automática é um mecanismo altamente confiável e eficiente, pois somente as partes alteradas dos arquivos são baixadas.

:heavy_check_mark: **Maior alcance e monetização simplificada**. Se optar por distribuir por meio da Microsoft Store, você poderá alcançar milhões de usuários do Windows 10, que podem adquirir aplicativos, jogos e compras realizadas em aplicativo com opções de pagamento locais.

:heavy_check_mark: **Adicione recursos UWP**.  Em seu próprio ritmo, você pode adicionar recursos UWP ao pacote do seu aplicativo, como uma interface de usuário XAML, atualizações de bloco dinâmico, tarefas de UWP em segundo plano, serviços de aplicativo, e muito mais.

:heavy_check_mark: **Casos de uso ampliados em dispositivos**. Usando a ponte, você pode migrar gradualmente o código para a Plataforma Universal do Windows a fim de alcançar todos os dispositivos Windows 10, inclusive telefones, Xbox One e HoloLens.

Para ver uma lista mais completa dos benefícios, consulte [Ponte de Desktop ](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="prepare"></a>Preparar

Primeiro, prepare seu aplicativo examinando o artigo [preparar para empacotar seu aplicativo de desktop](desktop-to-uwp-prepare.md)e, em seguida, endereçamento de qualquer um dos problemas que se aplicam ao seu aplicativo antes de criar um pacote de aplicativo do Windows para ele. Talvez você não precise fazer muitas alterações ao seu aplicativo antes de criar o pacote. No entanto, há algumas situações que podem exigir que você ajustar seu aplicativo antes de criar um pacote para ele.

<a id="convert" />

## <a name="package"></a>Pacote

Há várias maneiras diferentes de criar um pacote MSIX para seu aplicativo de desktop.

### <a name="build-an-msix-from-an-existing-app-package"></a>Criar um MSIX de um pacote de aplicativo existente

Se você já tiver um pacote do aplicativo (por exemplo, um MSI ou o instalador do App-V), é recomendável que você use o [ferramenta de empacotamento MSIX](../mpt-overview.md) reempacotar seu aplicativo de área de trabalho existente para o formato MSIX. Ele oferece uma interface do usuário interativo e uma linha de comando para conversões e lhe dá a habilidade de converter um aplicativo sem a necessidade de código-fonte.

> [!NOTE]
> Uma ferramenta anterior chamada a [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) também ainda está disponível para reempacotamento de um pacote de aplicativo da área de trabalho existente para o formato MSIX. No entanto, essa ferramenta foi preterida e é recomendável que você use o [ferramenta de empacotamento MSIX](../mpt-overview.md) em vez disso.

### <a name="build-an-msix-from-source-code-using-visual-studio"></a>Criar um MSIX do código-fonte usando o Visual Studio

Se você mantém seu aplicativo usando o Visual Studio e esse aplicativo não tem um instalador ou seu instalador não realiza tarefas complicadas demais, considere usar o Visual Studio no lugar.

Com o Visual Studio, é muito mais fácil criar um pacote. Você vai adicionar um projeto de empacotamento, referenciar seu projeto de desktop e depois pressionar F5 para depurar seu aplicativo. Sem ajustes manuais. Essa nova experiência simplificada é uma grande melhoria em relação à experiência disponível na versão anterior do Visual Studio. Aqui estão algumas outras coisas que você pode fazer com ele.

:heavy_check_mark: Gere automaticamente os ativos visuais.

:heavy_check_mark: Fazer alterações ao seu manifesto usando um designer visual.

:heavy_check_mark: Gere seu pacote usando um assistente.

:heavy_check_mark: Atribuir facilmente uma identidade para seu aplicativo de um nome que você reservou já [Partner Center](https://partner.microsoft.com/dashboard).

Consulte [empacotar um aplicativo de desktop usando o Visual Studio](desktop-to-uwp-packaging-dot-net.md)

### <a name="third-party-installer"></a>Instalador de terceiros

 Vários produtos de terceiros populares e instaladores agora dão suporte a capacidade de empacotar um aplicativo da área de trabalho. Você pode usá-los para gerar instaladores MSI ou pacotes do aplicativo com apenas alguns cliques. Embora não produza documentação sobre como usar essas ferramentas, visite seus sites para saber mais.

#### <a name="advanced-installer"></a>Instalador avançado

A Caphyon fornece uma ferramenta de empacotamento de aplicativo da área de trabalho baseada em GUI gratuita que ajuda você a gerar um pacote de aplicativo do Windows para seu app com apenas alguns cliques. Ele pode usar qualquer instalador; até mesmo aquelas que são executadas no modo silencioso e executa uma validação verificar para determinar se o aplicativo é adequado para empacotamento.
O Desktop App Converter também se integra ao Hyper-V e ao [VMware](https://www.vmware.com/). Isso significa que você pode usar suas próprias máquinas virtuais, sem precisar baixar uma imagem [Docker](https://docs.docker.com/) correspondente que pode ter mais de 3GB de tamanho.

<img width="20%" src="images/Advanced_Installer_Vertical.png">

Você pode usar o [Instalador Avançado](https://www.advancedinstaller.com/) para gerar MSI e [pacotes de aplicativos Windows](https://www.advancedinstaller.com/uwp-app-package.html) a partir dos projetos existentes. Você também pode usar o instalador avançado para importar pacotes de aplicativos do Windows que você gera usando o Microsoft Desktop App Converter. Uma vez importado, você pode mantê-los usando ferramentas visuais especificamente projetadas para aplicativos UWP.

O Advanced Installer também fornece uma extensão para o Visual Studio 2017 e 2015 que pode ser usada para [compilar e depurar aplicativos de ponte de Desktop](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

Veja este [vídeo](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) para uma rápida visão geral.

> [!TIP]
> Não se esqueça de conferir o recém-lançado [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html).

#### <a name="cloudhouse-compatibility-containers"></a>Contêineres de Compatibilidade Cloudhouse

Para os clientes corporativos com aplicativos de linha de negócios incompatíveis com o Windows 10 e o Windows 10 S, os Contêineres de Compatibilidade da Cloudhouse permitem que os aplicativos do Windows XP e do Windows 7 sejam executados no Windows 10 e, em seguida, convertidos para execução na Plataforma Universal do Windows (UWP) para disponibilização pela Microsoft Store para Empresas ou do Microsoft Intune, sem alteração no código-fonte. Inscreva-se em uma [Avaliação gratuita](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/cloudhouse-container-logo.png">

Cloudhouse fornece um gerenciador automático de linha de empacotamento de aplicativos comerciais em [contêineres compatibilidade](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) nos sistemas operacionais que os aplicativos é executado em hoje em dia (por exemplo: Windows XP) e, em seguida [prepará-lo para conversão](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) para UWP. Em seguida, o Contêiner é convertido para o novo formato de pacote de aplicativos do Windows ao integrá-lo à ferramenta Desktop App Converter da Microsoft.

O Empacotador automático usa a análise de instalação/captura e de tempo de execução a fim de criar um Contêiner para o aplicativo, o que inclui os arquivos do aplicativo, o registro, os tempos de execução, as dependências, além do mecanismo de compatibilidade e redirecionamento necessários para que o aplicativo seja executado no Windows 10. O Contêiner fornece isolamento do aplicativo e seus tempos de execução para que não afetem ou entrem em conflito com outros aplicativos executados no dispositivo do usuário.

Saiba mais sobre como você pode fornecer aplicativos de negócios pela Microsoft Store para Empresas. Leia tudo em nosso [Blog de lançamento](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp).

#### <a name="firegiant"></a>FireGiant

O [extensão FireGiant MSIX](https://www.firegiant.com/products/wix-expansion-pack/msix) permite que você crie pacotes de aplicativos do Windows e pacotes do MSI simultaneamente com o mesmo código de origem do WiX. Sempre que você criar, você pode direcionar o Windows 10 com um pacote de aplicativo do Windows e versões anteriores do Windows com o MSI.

<img width="20%" src="images/FG3rdPartyLogo.png">

A extensão de FireGiant MSIX usa análise estática e intelligent emulação de seus projetos WiX para criar pacotes de aplicativos do Windows sem a sobrecarga de espaço e tempo de execução do disco de contêineres ou máquinas virtuais.

Porque a extensão FireGiant MSIX não converte seu instalador ao executá-lo, você pode manter seu instalador do WiX sem precisar convertê-la repetidamente para pacotes de aplicativos do Windows. Todos os usuários em diferentes versões do Windows obtêm seus últimos aprimoramentos e você não precisa se preocupar com pacotes de aplicativos MSI e Windows fora de sincronia.

Confira esta [vídeo](https://www.youtube.com/watch?v=AFBpdBiAYQE) e veja como em algumas linhas de código FireGiant CEO Rob Mensching cria uma versão de Appx (pacote de aplicativo do Windows) da ferramenta de compactação de 7-Zip do código-fonte aberto popular e como ele melhora a ambos os aplicativos do Windows e Pacotes do MSI com alterações no mesmo código de origem do WiX.

#### <a name="installaware"></a>InstallAware

Install**Aware**, com um [registro](https://www.installaware.com/press-room.htm) de suporte rápido para inovações da Microsoft, compilações de [pacotes de aplicativos do Windows (Ponte de Desktop)](https://www.installaware.com/appx-builder.htm), App-V (Virtualização de aplicativo), MSI (Windows Installer) e Pacotes EXE (Código nativo) de uma única origem.

<img width="20%" src="images/installaware.png">

A Install**Aware** fornece extensões gratuitas do Install**Aware** para as versões 2012 a 2017 do . Você pode usá-las para criar pacotes de aplicativo do Windows com um único clique diretamente da [barra de ferramentas do Visual Studio](https://www.installaware.com/visual-studio-installer-2015.htm).

Você também pode importar qualquer instalação, mesmo se não tiver o código-fonte dessa instalação, usando o Package**Aware** (capturas de instalação sem instantâneos) ou o Assistente de Importação de Banco de Dados (para todos os instaladores MSI e módulos de mesclagem MSM). Você pode usar as [ferramentas de GUI](https://www.installaware.com/scripting-two-way-integrated-ide.htm) para manter e aprimorar suas importações, visualmente ou por meio de script.

As [opções avançadas de criação de APPX](https://www.installaware.com/mhtml5/desktop/appx.htm) ajudam a direcionar envios da Microsoft Store ou a produzir binários de pacote de aplicativo do Windows para distribuição de sideload para os usuários finais. Você pode até mesmo compilar pacotes do Instalador **WSA** (Aplicativos do Windows Server) destinados a implantações para **Nano Servidor** desde uma única origem e com suporte total para [automação de linha de comando](https://www.installaware.com/scripting-automation-interface.htm), além de uma GUI.

A Install**Aware** também criou como [software livre](https://www.installaware.com/gnu.asp) uma **biblioteca de compilador APPX**, além de um applet de linha de comando de exemplo, sob a licença da GNU Affero GPL. Tudo isso foi projetado para uso com plataformas de software livre, como a WiX.

#### <a name="installshield"></a>InstallShield

A InstallShield fornece uma única solução para desenvolver instaladores MSI e EXE, criar pacotes UWP (Plataforma Universal do Windows) e WSA (Aplicativo de Windows Server) e para virtualizar aplicativos com um mínimo de scripts, codificação e reformulação.

<img width="20%" src="images/InstallShield-logo.jpg">

Examine seu projeto InstallShield em segundos para economizar horas de trabalho de investigação ao identificar automaticamente potenciais problemas de compatibilidade entre seu aplicativo e pacotes UWP e WSA.

Prepare-se para a Microsoft Store e simplifique a experiência de instalação do software no Windows 10 com a criação de pacotes de aplicativo UWP de seus projetos existentes do InstallShield. Crie pacotes do Windows Installer e de aplicativo UWP para dar suporte a todos os cenários de implantação desejados por seus clientes. Dê suporte a implantações do Nano Servidor e do Windows Server 2016 ao criar pacotes WSA de seus projetos existentes do InstallShield.

Desenvolva sua instalação em módulos para facilitar a implantação e a manutenção e então mescle os componentes e as dependências em tempo de compilação em um único pacote de aplicativo UWP para a Microsoft Store. Para a distribuição direta fora da loja, empacote seus Pacotes de Aplicativo UWP e outras dependências com um instalador de IU de pacote/avançado.

Saiba mais neste [livro eletrônico](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

#### <a name="pace-suite"></a>PACE Suite

O [PACE Suite](https://pacesuite.com/) é uma ferramenta de empacotamento de aplicativo que pode ser usada para levar seus aplicativos da área de trabalho para a Plataforma Universal do Windows.

<img width="20%" src="images/PACE.png">

Com o PACE Suite, você não precisa preparar ambientes de empacotamento especiais ou instalar componentes adicionais do SDK do Windows. O PACE Suite pode criar pacotes de aplicativo do Windows de maneira independente em seu ambiente de empacotamento padrão no Windows 10 ou Windows Server 2016. Confira este [exemplo ilustrado](https://pacesuite.com/convert-exe-to-appx/) para saber como o PACE Suite trata o empacotamento de um instalador em um pacote de aplicativo do Windows.

Além de criar pacotes de aplicativo do Windows, você também pode usar o PACE Suite para criar pacotes do Windows Installer (MSI), patches (MSP), transformações (MST) e pacotes do App-V. Quando se trada de criação de MSI, o PACE Suite ajuda no gerenciamento de upgrades, configurações de permissão, ações personalizadas, scripts e outros. Você também pode publicar seus aplicativos diretamente no System Center Configuration Manager.

Para revisar todos os recursos de empacotamento de aplicativo, consulte [Recursos do PACE Suite](https://pacesuite.com/features/).

#### <a name="rad-studio"></a>RAD Studio

Consulte [RAD Studio da Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

#### <a name="raypack-studio"></a>RayPack Studio

Solução, embalagem da Raynet [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio), suporta a criação de pacotes para aplicativos da área de trabalho como um dos vários possíveis resultados da conversão eficiente e fácil de configurar e reempacotamento do framework.

<img width="20%" src="images/RaynetLogo_v3.png">

Os ambientes virtuais existentes (Estação de Trabalho VMware, Hyper-V) podem ser usados para realizar a conversão automatizada/em massa sem uma configuração demorada do ambiente. Um componente do Studio ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)) é capaz de fazer testes de compatibilidade e triagem de pré-conversão para verificar se o software está qualificado para a conversão. Além disso, os usuários podem realizar agora verificações abrangentes de colisão e compatibilidade com diversas edições do Windows 10, incluindo as atualizações de Aniversário e para Criadores.

Ao lado de criação de pacotes de software para o formato APPX/UWP do Windows 10, o RayPack Studio também pode ser usado para criar pacotes clássicos do Windows Installer (MSI), patches (MSP), transformações (MST) e pacotes App-V. Além disso, essa solução vem com um conjunto de produtos de software e componentes para empacotamento de software empresarial profissional. Além de empacotamento de software e virtualização, o RayPack Studio considera todas as tarefas relacionadas ao empacotamento: verificações de compatibilidade e conflitos de pacotes e aplicativos de software ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), avaliação de software ([RayEval](https://raynet.de/Raynet-Products/RayEval)) e controle de qualidade ([RayQC](https://raynet.de/Raynet-Products/RayQC)).

Combinado ao [RayFlow](https://raynet.de/Raynet-Products/RayFlow), Sistema de Fluxo de Trabalho Empresarial da Raynet, os usuários podem trabalhar com eficiência no software por todo o ciclo de vida do aplicativo empresarial, desde a solicitação do pacote, passando pela avaliação, análise, empacotamento, garantia de qualidade, testes de aceitação do usuário e implantação. Todos os pacotes e formatos podem ser armazenados e implantados diretamente no SCCM ou em outras soluções. Todo o processo de ciclo de vida do aplicativo é controlado e gerenciado pelo RayFlow. Além disso, quaisquer sistemas de pedidos, como o ServiceNow, podem ser integrados. A Raynet cria fábricas de empacotamento de software no mundo inteiro com suas ferramentas para provedores de serviço.

Confirme por conta própria e obtenha a [licença de avaliação gratuita](https://raynet.de/contact?init=license) do RayPack Studio e do RayFlow, da Raynet. Para obter mais informações, visite [www.raynet.de](https://raynet.de/home).

**Links relacionados**:

* Raynet: [https://raynet.de/home](https://raynet.de/home)
* RayPack Studio: [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow: [https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval: [https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC: [https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced: [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* Licença de avaliação gratuita: [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)

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
> Se você planeja publicar seu aplicativo para a Microsoft Store, certifique-se de que seu aplicativo funcione corretamente em dispositivos que executam o Windows 10 no modo S. Isso não é um requisito da Store. Veja [Testar seu aplicativo do Windows para o Windows 10 no modo S](desktop-to-uwp-test-windows-s.md).

## <a name="validate"></a>Validar

Para fornecer a melhor chance de ser publicado em que a Microsoft Store ou torna-se ao seu aplicativo [Windows Certified](https://go.microsoft.com/fwlink/p/?LinkID=309666), validar e testá-lo localmente antes de enviá-lo para certificação.

Se você estiver usando o DAC para empacotar seu aplicativo, você pode usar o novo ``-Verify`` sinalizador para validar seu pacote contra os requisitos de Store e o aplicativo de área de trabalho de pacote. Consulte [Empacotar um app, assinar o app e prepará-lo para envio à loja](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters).

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
