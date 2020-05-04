---
Description: Este guia fornece uma lista de produtos e instaladores de terceiros que podem ser usados para empacotar aplicativos da área de trabalho.
title: Formar pacote de um aplicativo da área de trabalho usando instaladores de terceiros
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8c753e40c6ccf1e20458159f863bbd17a8617f5b
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2020
ms.locfileid: "81613974"
---
# <a name="package-a-desktop-app-using-third-party-installers"></a>Formar pacote de um aplicativo da área de trabalho usando instaladores de terceiros

Veja abaixo uma lista de produtos e instaladores populares de terceiros com suporte para a capacidade de empacotar aplicativos da área de trabalho. Você pode usá-los para gerar instaladores MSI ou pacotes do aplicativo com apenas alguns cliques. Embora nós não tenhamos documentação sobre como usar essas ferramentas, visite os sites delas para saber mais.

## <a name="advanced-installer"></a>Advanced Installer

A Caphyon fornece uma ferramenta gratuita de empacotamento de aplicativos da área de trabalho baseada em GUI que ajuda você a gerar um pacote de aplicativo do Windows para seu aplicativo com apenas alguns cliques. Ele pode usar qualquer instalador, mesmo aqueles que são executados em modo sem confirmação, e executa uma verificação de validação para determinar se o aplicativo é adequado para empacotamento. O Desktop App Converter também se integra ao Hyper-V e ao [VMware](https://www.vmware.com/). Isso significa que você pode usar suas próprias máquinas virtuais, sem precisar baixar uma imagem do [Docker](https://docs.docker.com/) correspondente, que pode ter mais de 3GB de tamanho.

<img width="20%" src="images/Advanced_Installer_Vertical.png">

Você pode usar o [Advanced Installer](https://www.advancedinstaller.com/) para gerar MSI e [pacotes de aplicativos do Windows](https://www.advancedinstaller.com/uwp-app-package.html) com base em projetos existentes. Você também pode usá-lo para importar pacotes de aplicativos do Windows gerados usando o Microsoft Desktop App Converter. Após a importação, você pode mantê-los usando ferramentas visuais projetadas especificamente para aplicativos UWP.

O Advanced Installer também fornece uma extensão para o Visual Studio 2017 e 2015 que pode ser usada para [compilar e depurar aplicativos da Ponte de Desktop](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

Assista a este [vídeo](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) para ter uma visão geral rápida.

> [!TIP]
> Não se esqueça de conferir o recém-lançado [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html).

## <a name="cloudhouse-compatibility-containers"></a>Cloudhouse Compatibility Containers

Para clientes corporativos que têm aplicativos de linha de negócios incompatíveis com o Windows 10 e o Windows 10 S, os Compatibility Containers da Cloudhouse permitem que aplicativos do Windows XP e do Windows 7 sejam executados no Windows 10 e, em seguida, os converte para execução na UWP (Plataforma Universal do Windows) para fornecimento pela Microsoft Store para Empresas ou pelo Microsoft Intune, sem alteração no código-fonte. Inscreva-se para fazer uma [Avaliação gratuita](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/cloudhouse-container-logo.png">

A Cloudhouse fornece um Empacotador automático para empacotar aplicativos de linha de negócios em [Compatibility Containers](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) nos sistemas operacionais em que os aplicativos são executados atualmente (por exemplo, o Windows XP) e, em seguida, os [prepara para conversão](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) para a UWP. Em seguida, o Contêiner é convertido para o novo formato de pacote de aplicativo do Windows por meio da integração com a ferramenta Desktop App Converter da Microsoft.

O Empacotador automático usa instalação/captura e análise de runtime a fim de criar um Contêiner para o aplicativo, que inclui os arquivos, o Registro, os runtimes e as dependências do aplicativo, além do mecanismo de compatibilidade e redirecionamento necessário para que o aplicativo seja executado no Windows 10. O Contêiner fornece isolamento para o aplicativo e seus runtimes, para que eles não afetem nem entrem em conflito com outros aplicativos executados no dispositivo do usuário.

Saiba mais sobre como você pode fornecer aplicativos de negócios pela Microsoft Store para Empresas. Leia tudo em nosso [Blog de lançamento](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp).

## <a name="firegiant"></a>FireGiant

A [extensão de MSIX da FireGiant](https://www.firegiant.com/products/wix-expansion-pack/msix) permite criar pacotes de aplicativo do Windows e pacotes MSI simultaneamente usando o mesmo código-fonte do WiX. Sempre que compilar, você poderá ter como destino o Windows 10 com um pacote de aplicativo do Windows e versões anteriores do Windows com MSI.

<img width="20%" src="images/FG3rdPartyLogo.png">

A extensão de MSIX da FireGiant usa análise estática e emulação inteligente de projetos WiX para criar pacotes de aplicativo do Windows sem a sobrecarga de espaço em disco e de runtime de contêineres ou máquinas virtuais.

Como a extensão de MSIX da FireGiant não converte o instalador ao executá-lo, você pode manter seu instalador do WiX sem precisar convertê-lo repetidamente em pacotes de aplicativo do Windows. Todos os usuários em diferentes versões do Windows recebem seus aprimoramentos mais recentes e você não precisa se preocupar com pacotes de aplicativos do Windows e do MSI fora de sincronia.

Confira este [vídeo](https://www.youtube.com/watch?v=AFBpdBiAYQE) e veja como, em algumas linhas de código, o CEO da FireGiant, Rob Mensching, cria uma versão do Appx (pacote de aplicativo do Windows) da conhecida ferramenta de compactação de software livre 7-Zip e, em seguida, aprimora os pacotes de aplicativos do Windows e do MSI com alterações no mesmo código-fonte WiX. 

## <a name="installaware"></a>InstallAware

O InstallAware, com um [registro de acompanhamento](https://www.installaware.com/press-room.htm) de suporte rápido para inovações da Microsoft, builds de [pacotes de aplicativos do Windows (Ponte de Desktop)](https://www.installaware.com/appx-builder.htm), App-V (Application Virtualization), MSI (Windows Installer) e pacotes EXE (código nativo) de uma só origem.

<img width="20%" src="images/installaware.png">

O InstallAware fornece extensões gratuitas do InstallAware para as versões 2012 a 2017 do Visual Studio. Você pode usá-las para criar pacotes de aplicativos do Windows com um só clique diretamente na [barra de ferramentas do Visual Studio](https://www.installaware.com/visual-studio-installer-2015.htm).

Você também pode importar qualquer instalação, mesmo se não tiver o código-fonte da instalação em questão, usando o PackageAware (capturas de instalação sem instantâneos) ou o Assistente de Importação de Banco de Dados (para todos os instaladores MSI e módulos de mesclagem MSM). Use as [ferramentas de GUI](https://www.installaware.com/scripting-two-way-integrated-ide.htm) para manter e aprimorar suas importações, visualmente ou por meio de script.

As [opções avançadas de criação de appx](https://www.installaware.com/mhtml5/desktop/appx.htm) ajudam você a direcionar envios da Microsoft Store ou a produzir binários assinados do pacote do aplicativo do Windows para distribuição por sideload aos usuários finais. Você pode, até mesmo, compilar pacotes do Instalador WSA (Aplicativos do Windows Server) destinados a implantações no **Nano Server** em uma só origem e com suporte total para [automação de linha de comando](https://www.installaware.com/scripting-automation-interface.htm), além de uma GUI.

O InstallAware também disponibilizou como [software livre](https://www.installaware.com/gnu.asp) uma **biblioteca de compilador appx**, além de um exemplo de miniaplicativo de linha de comando, sob a licença da GNU Affero GPL. Tudo isso foi projetado para uso com plataformas de software livre, como a WiX.

## <a name="installshield"></a>InstallShield

O InstallShield fornece uma solução para desenvolver instaladores MSI e EXE, criar pacotes UWP (Plataforma Universal do Windows) e WSA (Aplicativo de Windows Server) e virtualizar aplicativos com um mínimo de scripts, codificação e reformulação.

<img width="20%" src="images/InstallShield-logo.jpg">

Examine seu projeto do InstallShield em segundos para economizar horas de trabalho de investigação ao identificar automaticamente potenciais problemas de compatibilidade entre seu aplicativo e pacotes UWP e WSA.

Prepare-se para a Microsoft Store e simplifique a experiência de instalação do software no Windows 10 com a criação de pacotes de aplicativo UWP de seus projetos existentes do InstallShield. Crie pacotes do Windows Installer e de aplicativos UWP para dar suporte a todos os cenários de implantação desejados por seus clientes. Dê suporte a implantações do Nano Servidor e do Windows Server 2016 criando pacotes WSA de seus projetos existentes do InstallShield.

Desenvolva sua instalação em módulos para facilitar a implantação e a manutenção e, então, mescle os componentes e as dependências em tempo de compilação em apenas um pacote de aplicativo UWP para a Microsoft Store. Para distribuição direta fora da Store, empacote seus Pacotes de Aplicativo UWP e outras dependências com um instalador de IU de pacote/avançado.

Saiba mais neste [livro eletrônico](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

## <a name="pace-suite"></a>PACE Suite

O [PACE Suite](https://pacesuite.com/) é uma ferramenta de empacotamento de aplicativo que pode ser usada para levar seus aplicativos da área de trabalho para a Plataforma Universal do Windows.

<img width="20%" src="images/PACE.png">

Com o PACE Suite, você não precisa preparar ambientes de empacotamento especiais nem instalar componentes adicionais do SDK do Windows. O PACE Suite pode criar pacotes de aplicativo do Windows de maneira independente em seu ambiente de empacotamento padrão no Windows 10 ou no Windows Server 2016. Confira este [exemplo ilustrado](https://pacesuite.com/convert-exe-to-appx/) para saber como o PACE Suite trata o empacotamento de um instalador em um pacote de aplicativo do Windows.

Além de criar pacotes de aplicativo do Windows, você também pode usar o PACE Suite para criar pacotes do Windows Installer (MSI), patches (MSP), transformações (MST) e pacotes do App-V. Quando se trata de criação de MSI, o PACE Suite ajuda no gerenciamento de upgrades, configurações de permissão, ações personalizadas, scripts e outros. Você também pode publicar seus aplicativos diretamente no System Center Configuration Manager.

Para revisar todos os recursos de empacotamento de aplicativo, confira [Recursos do PACE Suite](https://pacesuite.com/features/).

## <a name="rad-studio"></a>RAD Studio

Confira [RAD Studio da Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

## <a name="raypack-studio"></a>RayPack Studio

A solução de empacotamento da Raynet, o [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio), dá suporte à criação de pacotes para aplicativos da área de trabalho como um dos vários resultados possíveis da estrutura eficiente e fácil de configurar a conversão e o re-empacotamento.

<img width="20%" src="images/RaynetLogo_v3.png">

Os ambientes virtuais existentes (Estação de Trabalho VMware, Hyper-V) podem ser usados para realizar a conversão automatizada/em massa sem precisar fazer uma configuração demorada do ambiente. Um componente do Studio ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)) é capaz de fazer testes de compatibilidade e triagem pré-conversão para verificar se o software está qualificado para conversão. Além disso, agora os usuários podem realizar verificações abrangentes de colisão e compatibilidade com diversas edições do Windows 10, incluindo as atualizações de Aniversário e para Criadores.

Além da criação de pacotes de software para o formato APPX/UWP do Windows 10, o RayPack Studio também pode ser usado para criar pacotes clássicos do Windows Installer (MSI), patches (MSP), transformações (MST) e pacotes App-V. Além disso, essa solução vem com um conjunto de produtos de software e componentes para empacotamento de software empresarial profissional. Além do empacotamento de software e da virtualização, o RayPack Studio considera todas as tarefas relacionadas ao empacotamento: verificações de compatibilidade e conflitos de pacotes e aplicativos de software ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), avaliação de software ([RayEval](https://raynet.de/Raynet-Products/RayEval)) e garantia de qualidade ([RayQC](https://raynet.de/Raynet-Products/RayQC)).

Combinado ao [RayFlow](https://raynet.de/Raynet-Products/RayFlow), o Sistema de Fluxo de Trabalho Empresarial da Raynet, os usuários podem trabalhar com eficiência no software por todo o ciclo de vida do aplicativo empresarial, desde a solicitação do pacote, passando pela avaliação, análise, empacotamento, garantia de qualidade, testes de aceitação do usuário e implantação. Todos os pacotes e formatos podem ser armazenados e implantados diretamente no SCCM ou em outras soluções. Todo o processo de ciclo de vida do aplicativo é controlado e gerenciado pelo RayFlow. Além disso, qualquer sistema de pedidos, como o ServiceNow, pode ser integrado. A Raynet cria fábricas de empacotamento de software no mundo inteiro com suas ferramentas para provedores de serviço.

Veja você mesmo e obtenha a [licença de avaliação gratuita](https://raynet.de/contact?init=license) do RayPack Studio e do RayFlow da Raynet. Para obter mais informações, visite [www.raynet.de](https://raynet.de/home).

Links relacionados:

* Raynet: [https://raynet.de/home](https://raynet.de/home)
* RayPack Studio: [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow: [https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval: [https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC: [https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced: [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* Licença de avaliação gratuita: [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)
