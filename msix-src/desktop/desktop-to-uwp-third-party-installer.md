---
description: Este guia fornece uma lista de produtos e instaladores de terceiros que podem ser usados para empacotar aplicativos da área de trabalho.
title: Formar pacote de um aplicativo da área de trabalho usando instaladores de terceiros
ms.date: 06/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 9143bcba76bd168d38b36d2c8bca2ff4fc89fb80
ms.sourcegitcommit: f0d0dd5275990c50ca4a72a9c6a689ba604aba38
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101841151"
---
# <a name="package-a-desktop-app-using-third-party-installers"></a>Formar pacote de um aplicativo da área de trabalho usando instaladores de terceiros

Veja abaixo uma lista de produtos e instaladores populares de terceiros com suporte para a capacidade de empacotar aplicativos da área de trabalho. Você pode usá-los para gerar instaladores MSI ou pacotes do aplicativo com apenas alguns cliques. Embora nós não tenhamos documentação sobre como usar essas ferramentas, visite os sites delas para saber mais.

## <a name="advanced-installer"></a>Advanced Installer

A Caphyon fornece uma ferramenta gratuita de empacotamento de aplicativos da área de trabalho baseada em GUI que ajuda você a gerar um pacote de aplicativo do Windows para seu aplicativo com apenas alguns cliques. Ele pode usar qualquer instalador, mesmo aqueles que são executados em modo sem confirmação, e executa uma verificação de validação para determinar se o aplicativo é adequado para empacotamento. O Desktop App Converter também se integra ao Hyper-V e ao [VMware](https://www.vmware.com/). Isso significa que você pode usar suas próprias máquinas virtuais, sem precisar baixar uma imagem do [Docker](https://docs.docker.com/) correspondente, que pode ter mais de 3GB de tamanho.

<img width="20%" src="images/Advanced_Installer_Vertical.png" Alt-text="Logo of Advance Installer">

Você pode usar o [Advanced Installer](https://www.advancedinstaller.com/) para gerar MSI e [pacotes de aplicativos do Windows](https://www.advancedinstaller.com/uwp-app-package.html) com base em projetos existentes. Você também pode usá-lo para importar pacotes de aplicativos do Windows gerados usando o Microsoft Desktop App Converter. Após a importação, você pode mantê-los usando ferramentas visuais projetadas especificamente para aplicativos UWP.

O Advanced Installer também fornece uma extensão para o Visual Studio 2017 e 2015 que pode ser usada para [compilar e depurar aplicativos da Ponte de Desktop](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

Assista a este [vídeo](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) para ter uma visão geral rápida.

> [!TIP]
> Não se esqueça de conferir o recém-lançado [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html).


## <a name="appcure"></a>appCURE

**appCURE** permite que a TI acelere a captura de aplicativos e simplifique o processo de transformação para o MSIX. O appCURE da SSH2 captura aplicativos em execução no ponto de extremidade sem se preocupar com a localização de mídia de instalação, instruções etc; permite a atualização e a correção para criar correções PSF prontas para serem adicionadas, criando, assim, a alteração de etapa abrangente na velocidade em que os ambientes de usuário final podem ser executados.

<img width="20%" src="images/AppCure-WB.png" alt-text="Logo of appCure">

O **appCURE** captura detalhes do aplicativo, da execução de aplicativos à garantia de que todos os pontos que possam afetar os aplicativos de usuário final sejam compreendidos. O appCURE os atualiza e os entrega como um pacote MSIX pronto para uso. Capturando todos os pontos de integração do aplicativo em seu ambiente atual, o appCURE oferece a velocidade para otimizar recursos de TI e planejar as suas migrações melhor e de modo mais rápido do que nunca, permitindo que as organizações entrem em produção com mais rapidez.

O **appCURE Studio**, criado com base no processo do appCURE, tem a capacidade de criar os ambientes mais ideais e mais bem utilizados para seu acervo de MSIX automatizando a criação de:
*   VHD/VHDX/VMDK/CIMFS
*   Alterações de certificação
*   Conversão de VHD em CIMFS
*   Anexação do aplicativo MSIX e APP Volumes 4
*   Teste e relatórios

## <a name="capture"></a>Capturar
O Capture da Access IT Automation é uma oferta de gerenciamento de aplicativos modular controlada por API que permite a criação autônoma e sem esforços de pacotes de aplicativos nos formatos de anexação de aplicativo, MSI, AppV e MSIX. 

Nós oferecemos resultados tangíveis que você pode usar diretamente por meio do Configuration Manager e da entrega do Intune ou pode utilizar ainda mais nossas amplas APIs de teste e publicação para que possamos criar os objetos de aplicativo do SCCM e do Intune para que você gerencie o Teste de Aceitação do Usuário para garantir um gerenciamento de aplicativos de TI perene. 

<img width="20%" src="images/Capture-AccessIT.png" alt-text="Logo  for Capture">

Nossas ofertas de API de anexação de aplicativo e MSIX são divididas no seguinte:  
* Criação de pacotes para MSI, AppV, MSIX e anexação de aplicativo 
  * API AppScan – podemos carregar todos os seus aplicativos MSI existentes e verificar a adequação do MSIX.  Verificação de bloqueadores como serviços de tempo de inicialização ou MSIs sem atalhos. 
  * A API de construtor do MSIX é para qualquer pipeline de CI/CD em que você precisa criar um MSIX sem a necessidade de instantâneo – os exemplos comuns são DevOps (arquivos soltos, binários). 
  * API de criação do MSIX – você fornece entradas simples de pacote de origem e arquivo de assinatura, e nós criamos e assinamos digitalmente a saída do MSIX usando a tecnologia de instantâneo. 
  * API de anexação de aplicativo – podemos criar como parte da API acima, por criação do MSIX, e com essa API podemos gerenciar o agrupamento de conjuntos de MSIX para criar CIMfs ou VHD de anexação de aplicativo. 
* Gerenciamento de testes 
  * API de teste de aceitação do usuário – usamos os pacotes concluídos do MSIX ou de anexação de aplicativo e criamos os objetos do Intune ou do Azure WVD para publicação e entrega. 
    * Capturamos os detalhes de aprovação e reprovação dos testadores UAT. 
    * Capturamos as capturas de tela e a auditoria completa do teste UAT. 
    *   Capturamos o desempenho do aplicativo no build do Windows 10. 
  * API de teste de carga e inicialização – carregue de maneira autônoma todos os seus aplicativos, um por um, em um build do Windows 10 (para comprar com outros testes de carga e inicialização em outras versões de build do Windows 10), verificando se seu pacote funciona em sistemas Windows 10 mais recentes 
    * Distribuímos o aplicativo usando o Intune para imitar a entrega de um aplicativo real. 
    * Inicializamos todos os atalhos do pacote MSIX para que não haja problemas. 
    *   O vídeo registra esse teste autônomo para aprovação ou reprovação do teste de inicialização. 
      * As aprovações significam seu investimento no empacotamento para MSIX. 
      * As reprovações fornecem detalhes para que você possa corrigir o pacote antes de mover os usuários finais para a próxima versão do Windows 10. 
  * API de teste de desempenho – dá a você a confiança de que alterações de pacote de alto risco são executadas conforme esperado em seu desktop físico e planta VDI/WVD 
    * Você pode configurar os contadores de desempenho que deseja monitorar. 
    *   Você pode definir a duração em horas do pacote entregue pelo Intune do MSIX. 
    *   Fornecemos todos os resultados do pacote de aplicativos, procurando especificamente picos de CPU ou de memória. 
    
Como conectamos nossas APIs em seus fluxos de trabalho tradicionais de gerenciamento de aplicativos de ponta a ponta existentes: [Solução de automação de empacotamento e teste de aplicativos de ponta a ponta](https://www.accessitautomation.com/access-capture-end-to-end) 

Gerenciamento de aplicativos controlado por uma API moderna: [teste e empacotamento de aplicativos controlados por uma API moderna](https://www.accessitautomation.com/api-driven-app-packaging-testing) 

## <a name="cloudhouse-compatibility-containers"></a>Contêineres de Compatibilidade Cloudhouse

Para os clientes corporativos com aplicativos de linha de negócios incompatíveis com o Windows 10 e o Windows 10 S, os Contêineres de Compatibilidade da Cloudhouse permitem que os aplicativos do Windows XP e do Windows 7 sejam executados no Windows 10 e, em seguida, convertidos para execução na Plataforma Universal do Windows (UWP) para disponibilização pela Microsoft Store para Empresas ou do Microsoft Intune, sem alteração no código-fonte. Inscreva-se em uma [Avaliação gratuita](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/cloudhouse-container-logo.png" alt-text="Logo of Cloudhouse Compatibility Container">

A Cloudhouse fornece um Empacotador automática de empacotamento de aplicativos de linha de negócios em [Contêineres de compatibilidade](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) nos sistemas operacionais que os aplicativos executam atualmente (por exemplo, o Windows XP) e [prepara-o para conversão](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) para a UWP. Em seguida, o Contêiner é convertido para o novo formato de pacote de aplicativos do Windows ao integrá-lo à ferramenta Desktop App Converter da Microsoft.

O Empacotador automático usa a análise de instalação/captura e de tempo de execução a fim de criar um Contêiner para o aplicativo, o que inclui os arquivos do aplicativo, o registro, os tempos de execução, as dependências, além do mecanismo de compatibilidade e redirecionamento necessários para que o aplicativo seja executado no Windows 10. O Contêiner fornece isolamento do aplicativo e seus tempos de execução para que não afetem ou entrem em conflito com outros aplicativos executados no dispositivo do usuário.

Saiba mais sobre como você pode fornecer aplicativos de negócios pela Microsoft Store para Empresas. Leia tudo em nosso [Blog de lançamento](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp).

## <a name="firegiant"></a>FireGiant

A [extensão de MSIX da FireGiant](https://www.firegiant.com/products/wix-expansion-pack/msix) permite criar pacotes de aplicativo do Windows e pacotes MSI simultaneamente usando o mesmo código-fonte do WiX. Sempre que compilar, você poderá ter como destino o Windows 10 com um pacote de aplicativo do Windows e versões anteriores do Windows com MSI.

<img width="20%" src="images/FG3rdPartyLogo.png" alt-text="Logo of FireGiant">

A extensão de MSIX da FireGiant usa análise estática e emulação inteligente de projetos WiX para criar pacotes de aplicativo do Windows sem a sobrecarga de espaço em disco e de runtime de contêineres ou máquinas virtuais.

Como a extensão de MSIX da FireGiant não converte o instalador ao executá-lo, você pode manter seu instalador do WiX sem precisar convertê-lo repetidamente em pacotes de aplicativo do Windows. Todos os usuários em diferentes versões do Windows obtêm seus últimos aprimoramentos e você não precisa se preocupar com pacotes de aplicativos MSI e Windows fora de sincronia.

Confira este [vídeo](https://www.youtube.com/watch?v=AFBpdBiAYQE) e veja como, em algumas linhas de código, o CEO da FireGiant, Rob Mensching, cria uma versão do Appx (pacote de aplicativo do Windows) da conhecida ferramenta de compactação de software livre 7-Zip e, em seguida, aprimora os pacotes de aplicativos do Windows e do MSI com alterações no mesmo código-fonte WiX.

## <a name="installaware"></a>InstallAware

O InstallAware, com um [registro de acompanhamento](https://www.installaware.com/press-room.htm) de suporte rápido para inovações da Microsoft, builds de [pacotes de aplicativos do Windows (Ponte de Desktop)](https://www.installaware.com/appx-builder.htm), App-V (Application Virtualization), MSI (Windows Installer) e pacotes EXE (código nativo) de uma só origem.

<img width="20%" src="images/installaware.png" alt-text="Logo of InstallAware">

A InstallAware fornece extensões gratuitas do InstallAware para as versões 2012 a 2017 do . Você pode usá-las para criar pacotes de aplicativo do Windows com um único clique diretamente da [barra de ferramentas do Visual Studio](https://www.installaware.com/visual-studio-installer-2015.htm).

Você também pode importar qualquer instalação, mesmo se não tiver o código-fonte dessa instalação, usando o PackageAware (capturas de instalação sem instantâneos) ou o Assistente de Importação de Banco de Dados (para todos os instaladores MSI e módulos de mesclagem MSM). Você pode usar as [ferramentas de GUI](https://www.installaware.com/scripting-two-way-integrated-ide.htm) para manter e aprimorar suas importações, visualmente ou por meio de script.

As [opções avançadas de criação de APPX](https://www.installaware.com/mhtml5/desktop/appx.htm) ajudam a direcionar envios da Microsoft Store ou a produzir binários de pacote de aplicativo do Windows para distribuição de sideload para os usuários finais. Você pode, até mesmo, compilar pacotes do Instalador WSA (Aplicativos do Windows Server) destinados a implantações no **Nano Server** em uma só origem e com suporte total para [automação de linha de comando](https://www.installaware.com/scripting-automation-interface.htm), além de uma GUI.

O InstallAware também disponibilizou como [software livre](https://www.installaware.com/gnu.asp) uma **biblioteca de compilador appx**, além de um exemplo de miniaplicativo de linha de comando, sob a licença da GNU Affero GPL. Tudo isso foi projetado para uso com plataformas de software livre, como a WiX.

## <a name="installshield"></a>InstallShield

A InstallShield fornece uma única solução para desenvolver instaladores MSI e EXE, criar pacotes UWP (Plataforma Universal do Windows) e WSA (Aplicativo de Windows Server) e para virtualizar aplicativos com um mínimo de scripts, codificação e reformulação.

<img width="20%" src="images/InstallShield-logo.jpg" alt-text="Logo of InstallShield">

Examine seu projeto InstallShield em segundos para economizar horas de trabalho de investigação ao identificar automaticamente potenciais problemas de compatibilidade entre seu aplicativo e pacotes UWP e WSA.

Prepare-se para a Microsoft Store e simplifique a experiência de instalação do software no Windows 10 com a criação de pacotes de aplicativo UWP de seus projetos existentes do InstallShield. Crie pacotes do Windows Installer e de aplicativo UWP para dar suporte a todos os cenários de implantação desejados por seus clientes. Dê suporte a implantações do Nano Servidor e do Windows Server 2016 ao criar pacotes WSA de seus projetos existentes do InstallShield.

Desenvolva sua instalação em módulos para facilitar a implantação e a manutenção e então mescle os componentes e as dependências em tempo de compilação em um único pacote de aplicativo UWP para a Microsoft Store. Para distribuição direta fora da Store, empacote seus Pacotes de Aplicativo UWP e outras dependências com um instalador de IU de pacote/avançado.

Saiba mais neste [livro eletrônico](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

## <a name="pace-suite"></a>PACE Suite

O [PACE Suite](https://pacesuite.com/) é uma ferramenta de empacotamento de aplicativo que pode ser usada para levar seus aplicativos da área de trabalho para a Plataforma Universal do Windows.

<img width="20%" src="images/PACE.png" alt-text="Logo of PACE Suite">

Com o PACE Suite, você não precisa preparar ambientes de empacotamento especiais ou instalar componentes adicionais do SDK do Windows. O PACE Suite pode criar pacotes de aplicativo do Windows de maneira independente em seu ambiente de empacotamento padrão no Windows 10 ou Windows Server 2016. Confira este [exemplo ilustrado](https://pacesuite.com/convert-exe-to-appx/) para saber como o PACE Suite trata o empacotamento de um instalador em um pacote de aplicativo do Windows.

Além de criar pacotes de aplicativo do Windows, você também pode usar o PACE Suite para criar pacotes do Windows Installer (MSI), patches (MSP), transformações (MST) e pacotes do App-V. Quando se trada de criação de MSI, o PACE Suite ajuda no gerenciamento de upgrades, configurações de permissão, ações personalizadas, scripts e outros. Você também pode publicar seus aplicativos diretamente no System Center Configuration Manager.

Para revisar todos os recursos de empacotamento de aplicativo, consulte [Recursos do PACE Suite](https://pacesuite.com/features/).

## <a name="rad-studio"></a>RAD Studio

Consulte [RAD Studio da Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

## <a name="raypack-studio"></a>RayPack Studio

A solução de empacotamento da Raynet, o [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio), dá suporte à criação de pacotes para aplicativos da área de trabalho como um dos vários resultados possíveis da estrutura eficiente e fácil de configurar a conversão e o re-empacotamento.

<img width="20%" src="images/RaynetLogo_v3.png" alt-text="Logo of RayPack Studio">

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

## <a name="rimo3-cloud"></a>Rimo3 Cloud

O Rimo3 Cloud possibilita a adoção de novas tecnologias em escala por meio da automação inteligente. Assim, ele permite a criação de pacotes de MSIX prontos para implantação e ajuda os clientes a tomar decisões informadas e controladas por dados sobre quais aplicativos serão convertidos e em quais ambientes esses aplicativos funcionarão melhor.

<img width="20%" src="images/Rimo3CLOUD-Logo-Colour-RGB.png" Alt-text="Logo of Rimo3 Cloud">

A modernização de aplicativos com o Rimo3 Cloud aproveita o poder da automação e da orquestração não só para fazer testes em massa, converter e corrigir aplicativos em pacotes de MSIX prontos para implantação, mas também: 

* Entender quais aplicativos são adequados para conversão; 
* Verificar se os aplicativos realmente funcionam no sistema operacional moderno de destino antes da conversão; 
* Testar os aplicativos convertidos.

O Rimo3 Cloud elimina quase todo o esforço manual necessário ao lidar com uma iniciativa de transformação de tecnologia, seja uma transformação em larga escala de todos os seus imóveis ou de uma exploração inicial de MSIX. 

Confira o nosso guia de cinco passos para modernizar seus aplicativos para MSIX em questão de minutos [aqui](https://rimo3.com/rimo3cloud/?msix-guide).

Para obter mais informações sobre a modernização dos seus aplicativos com o Rimo3 Cloud e para ver o que mais você pode fazer com a automação inteligente em escala, acesse: https://rimo3.com


