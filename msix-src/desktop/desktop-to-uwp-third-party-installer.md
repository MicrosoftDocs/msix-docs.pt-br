---
Description: Este guia fornece uma lista de produtos de terceiros e instaladores de aplicativos da área de trabalho do pacote.
title: Empacotar um aplicativo da área de trabalho usando os instaladores de terceiros
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e07d44d2a809a183d57b59f3121683bf0e2ab971
ms.sourcegitcommit: 0378a8897e0691bee4d9a957982961a377974856
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2019
ms.locfileid: "67253481"
---
# <a name="package-a-desktop-app-using-third-party-installers"></a>Empacotar um aplicativo da área de trabalho usando os instaladores de terceiros

Abaixo está uma lista de produtos de terceiros populares e instaladores que dão suporte a capacidade de empacotar um aplicativo da área de trabalho. Você pode usá-los para gerar instaladores MSI ou pacotes do aplicativo com apenas alguns cliques. Embora não produza documentação sobre como usar essas ferramentas, visite seus sites para saber mais.

## <a name="advanced-installer"></a>Instalador avançado

A Caphyon fornece uma ferramenta de empacotamento de aplicativo da área de trabalho baseada em GUI gratuita que ajuda você a gerar um pacote de aplicativo do Windows para seu app com apenas alguns cliques. Ele pode usar qualquer instalador; até mesmo aquelas que são executadas no modo silencioso e executa uma validação verificar para determinar se o aplicativo é adequado para empacotamento. O Desktop App Converter também se integra ao Hyper-V e ao [VMware](https://www.vmware.com/). Isso significa que você pode usar suas próprias máquinas virtuais, sem precisar baixar uma imagem [Docker](https://docs.docker.com/) correspondente que pode ter mais de 3GB de tamanho.

<img width="20%" src="images/Advanced_Installer_Vertical.png">

Você pode usar o [Instalador Avançado](https://www.advancedinstaller.com/) para gerar MSI e [pacotes de aplicativos Windows](https://www.advancedinstaller.com/uwp-app-package.html) a partir dos projetos existentes. Você também pode usar o instalador avançado para importar pacotes de aplicativos do Windows que você gera usando o Microsoft Desktop App Converter. Uma vez importado, você pode mantê-los usando ferramentas visuais especificamente projetadas para aplicativos UWP.

O Advanced Installer também fornece uma extensão para o Visual Studio 2017 e 2015 que pode ser usada para [compilar e depurar aplicativos de ponte de Desktop](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

Veja este [vídeo](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) para uma rápida visão geral.

> [!TIP]
> Não se esqueça de conferir o recém-lançado [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html).

## <a name="cloudhouse-compatibility-containers"></a>Contêineres de Compatibilidade Cloudhouse

Para os clientes corporativos com aplicativos de linha de negócios incompatíveis com o Windows 10 e o Windows 10 S, os Contêineres de Compatibilidade da Cloudhouse permitem que os aplicativos do Windows XP e do Windows 7 sejam executados no Windows 10 e, em seguida, convertidos para execução na Plataforma Universal do Windows (UWP) para disponibilização pela Microsoft Store para Empresas ou do Microsoft Intune, sem alteração no código-fonte. Inscreva-se em uma [Avaliação gratuita](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/cloudhouse-container-logo.png">

Cloudhouse fornece um gerenciador automático de linha de empacotamento de aplicativos comerciais em [contêineres compatibilidade](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) nos sistemas operacionais que os aplicativos é executado em hoje em dia (por exemplo: Windows XP) e, em seguida [prepará-lo para conversão](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) para UWP. Em seguida, o Contêiner é convertido para o novo formato de pacote de aplicativos do Windows ao integrá-lo à ferramenta Desktop App Converter da Microsoft.

O Empacotador automático usa a análise de instalação/captura e de tempo de execução a fim de criar um Contêiner para o aplicativo, o que inclui os arquivos do aplicativo, o registro, os tempos de execução, as dependências, além do mecanismo de compatibilidade e redirecionamento necessários para que o aplicativo seja executado no Windows 10. O Contêiner fornece isolamento do aplicativo e seus tempos de execução para que não afetem ou entrem em conflito com outros aplicativos executados no dispositivo do usuário.

Saiba mais sobre como você pode fornecer aplicativos de negócios pela Microsoft Store para Empresas. Leia tudo em nosso [Blog de lançamento](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp).

## <a name="firegiant"></a>FireGiant

O [extensão FireGiant MSIX](https://www.firegiant.com/products/wix-expansion-pack/msix) permite que você crie pacotes de aplicativos do Windows e pacotes do MSI simultaneamente com o mesmo código de origem do WiX. Sempre que você criar, você pode direcionar o Windows 10 com um pacote de aplicativo do Windows e versões anteriores do Windows com o MSI.

<img width="20%" src="images/FG3rdPartyLogo.png">

A extensão de FireGiant MSIX usa análise estática e intelligent emulação de seus projetos WiX para criar pacotes de aplicativos do Windows sem a sobrecarga de espaço e tempo de execução do disco de contêineres ou máquinas virtuais.

Porque a extensão FireGiant MSIX não converte seu instalador ao executá-lo, você pode manter seu instalador do WiX sem precisar convertê-la repetidamente para pacotes de aplicativos do Windows. Todos os usuários em diferentes versões do Windows obtêm seus últimos aprimoramentos e você não precisa se preocupar com pacotes de aplicativos MSI e Windows fora de sincronia.

Confira esta [vídeo](https://www.youtube.com/watch?v=AFBpdBiAYQE) e veja como em algumas linhas de código FireGiant CEO Rob Mensching cria uma versão de Appx (pacote de aplicativo do Windows) da ferramenta de compactação de 7-Zip do código-fonte aberto popular e como ele melhora a ambos os aplicativos do Windows e Pacotes do MSI com alterações no mesmo código de origem do WiX.

## <a name="installaware"></a>InstallAware

O InstallAware, com um [registro de acompanhamento](https://www.installaware.com/press-room.htm) de suportar rapidamente inovações da Microsoft, compila [pacotes de aplicativos do Windows (ponte de Desktop)](https://www.installaware.com/appx-builder.htm), App-V (Application Virtualization), o MSI (Windows Installer), e Pacotes EXE (código nativo) de uma única fonte.

<img width="20%" src="images/installaware.png">

O InstallAware fornece extensões gratuitas do InstallAware para versões do Visual Studio 2012 a 2017. Você pode usá-las para criar pacotes de aplicativo do Windows com um único clique diretamente da [barra de ferramentas do Visual Studio](https://www.installaware.com/visual-studio-installer-2015.htm).

Você também pode importar qualquer configuração, mesmo se você não tiver o código-fonte para que a instalação, usando PackageAware (programa de instalação sem instantâneo captura), ou o Assistente de importação de banco de dados (para todos os instaladores MSI e módulos de mesclagem do MSM). Você pode usar as [ferramentas de GUI](https://www.installaware.com/scripting-two-way-integrated-ide.htm) para manter e aprimorar suas importações, visualmente ou por meio de script.

As [opções avançadas de criação de APPX](https://www.installaware.com/mhtml5/desktop/appx.htm) ajudam a direcionar envios da Microsoft Store ou a produzir binários de pacote de aplicativo do Windows para distribuição de sideload para os usuários finais. Você pode até mesmo criar pacotes de instalador do WSA (aplicativos de servidor do Windows) que se destinam a implantações em **Nano Server** tudo a partir de uma fonte única e com suporte total para [automação de linha de comando](https://www.installaware.com/scripting-automation-interface.htm), além disso para uma interface gráfica do usuário.

O InstallAware também [livre](https://www.installaware.com/gnu.asp) um **biblioteca de construtor APPX**, juntamente com um miniaplicativo de linha de comando de exemplo, sob a licença da GNU Affero GPL. Tudo isso foi projetado para uso com plataformas de software livre, como a WiX.

## <a name="installshield"></a>InstallShield

A InstallShield fornece uma única solução para desenvolver instaladores MSI e EXE, criar pacotes UWP (Plataforma Universal do Windows) e WSA (Aplicativo de Windows Server) e para virtualizar aplicativos com um mínimo de scripts, codificação e reformulação.

<img width="20%" src="images/InstallShield-logo.jpg">

Examine seu projeto InstallShield em segundos para economizar horas de trabalho de investigação ao identificar automaticamente potenciais problemas de compatibilidade entre seu aplicativo e pacotes UWP e WSA.

Prepare-se para a Microsoft Store e simplifique a experiência de instalação do software no Windows 10 com a criação de pacotes de aplicativo UWP de seus projetos existentes do InstallShield. Crie pacotes do Windows Installer e de aplicativo UWP para dar suporte a todos os cenários de implantação desejados por seus clientes. Dê suporte a implantações do Nano Servidor e do Windows Server 2016 ao criar pacotes WSA de seus projetos existentes do InstallShield.

Desenvolva sua instalação em módulos para facilitar a implantação e a manutenção e então mescle os componentes e as dependências em tempo de compilação em um único pacote de aplicativo UWP para a Microsoft Store. Para distribuição diretos fora a Store, agrupar seus pacotes de aplicativo UWP e outras dependências junto com um instalador de pacote/avançado da interface do usuário.

Saiba mais neste [livro eletrônico](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

## <a name="pace-suite"></a>PACE Suite

O [PACE Suite](https://pacesuite.com/) é uma ferramenta de empacotamento de aplicativo que pode ser usada para levar seus aplicativos da área de trabalho para a Plataforma Universal do Windows.

<img width="20%" src="images/PACE.png">

Com o PACE Suite, você não precisa preparar ambientes de empacotamento especiais ou instalar componentes adicionais do SDK do Windows. O PACE Suite pode criar pacotes de aplicativo do Windows de maneira independente em seu ambiente de empacotamento padrão no Windows 10 ou Windows Server 2016. Confira este [exemplo ilustrado](https://pacesuite.com/convert-exe-to-appx/) para saber como o PACE Suite trata o empacotamento de um instalador em um pacote de aplicativo do Windows.

Além de criar pacotes de aplicativo do Windows, você também pode usar o PACE Suite para criar pacotes do Windows Installer (MSI), patches (MSP), transformações (MST) e pacotes do App-V. Quando se trada de criação de MSI, o PACE Suite ajuda no gerenciamento de upgrades, configurações de permissão, ações personalizadas, scripts e outros. Você também pode publicar seus aplicativos diretamente no System Center Configuration Manager.

Para revisar todos os recursos de empacotamento de aplicativo, consulte [Recursos do PACE Suite](https://pacesuite.com/features/).

## <a name="rad-studio"></a>RAD Studio

Consulte [RAD Studio da Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

## <a name="raypack-studio"></a>RayPack Studio

Solução, embalagem da Raynet [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio), suporta a criação de pacotes para aplicativos da área de trabalho como um dos vários possíveis resultados da conversão eficiente e fácil de configurar e reempacotamento do framework.

<img width="20%" src="images/RaynetLogo_v3.png">

Os ambientes virtuais existentes (Estação de Trabalho VMware, Hyper-V) podem ser usados para realizar a conversão automatizada/em massa sem uma configuração demorada do ambiente. Um componente do Studio ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)) é capaz de fazer testes de compatibilidade e triagem de pré-conversão para verificar se o software está qualificado para a conversão. Além disso, os usuários podem realizar agora verificações abrangentes de colisão e compatibilidade com diversas edições do Windows 10, incluindo as atualizações de Aniversário e para Criadores.

Ao lado de criação de pacotes de software para o formato APPX/UWP do Windows 10, o RayPack Studio também pode ser usado para criar pacotes clássicos do Windows Installer (MSI), patches (MSP), transformações (MST) e pacotes App-V. Além disso, essa solução vem com um conjunto de produtos de software e componentes para empacotamento de software empresarial profissional. Além de empacotamento de software e virtualização, o RayPack Studio considera todas as tarefas relacionadas ao empacotamento: verificações de compatibilidade e conflitos de pacotes e aplicativos de software ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), avaliação de software ([RayEval](https://raynet.de/Raynet-Products/RayEval)) e controle de qualidade ([RayQC](https://raynet.de/Raynet-Products/RayQC)).

Combinado ao [RayFlow](https://raynet.de/Raynet-Products/RayFlow), Sistema de Fluxo de Trabalho Empresarial da Raynet, os usuários podem trabalhar com eficiência no software por todo o ciclo de vida do aplicativo empresarial, desde a solicitação do pacote, passando pela avaliação, análise, empacotamento, garantia de qualidade, testes de aceitação do usuário e implantação. Todos os pacotes e formatos podem ser armazenados e implantados diretamente no SCCM ou em outras soluções. Todo o processo de ciclo de vida do aplicativo é controlado e gerenciado pelo RayFlow. Além disso, quaisquer sistemas de pedidos, como o ServiceNow, podem ser integrados. A Raynet cria fábricas de empacotamento de software no mundo inteiro com suas ferramentas para provedores de serviço.

Confirme por conta própria e obtenha a [licença de avaliação gratuita](https://raynet.de/contact?init=license) do RayPack Studio e do RayFlow, da Raynet. Para obter mais informações, visite [www.raynet.de](https://raynet.de/home).

Links relacionados:

* Raynet: [https://raynet.de/home](https://raynet.de/home)
* RayPack Studio: [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow: [https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval: [https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC: [https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced: [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* Licença de avaliação gratuita: [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)
