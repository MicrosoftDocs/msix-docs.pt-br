---
title: Formatos de pacote do aplicativo
description: Este artigo descreve os diferentes tipos de formatos de pacote MSIX que são úteis para determinados cenários.
ms.date: 07/02/2019
ms.topic: article
keywords: Windows 10, msix, UWP, desktop, pacote
ms.localizationpriority: medium
ms.openlocfilehash: 30d05cc1b2f007cb2d0049608c8b003ab04dde89
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091144"
---
# <a name="app-package-formats"></a>Formatos de pacote do aplicativo

Além dos pacotes MSIX padrão que contêm um aplicativo do Windows, há vários tipos diferentes de formatos de pacote MSIX especializados que são úteis para determinados cenários.

## <a name="optional-packages"></a>Pacotes opcionais

Os pacotes opcionais são usados para complementar ou estender a funcionalidade original do pacote do aplicativo. É possível publicar um aplicativo, seguido da publicação de pacotes opcionais em um momento posterior, ou publicar pacotes de aplicativo e opcionais simultaneamente. Ao estender seu aplicativo por meio de um pacote opcional, você tem as vantagens de distribuir e monetizar conteúdo como um pacote de aplicativo separado. Os pacotes opcionais geralmente devem ser desenvolvidos pelo desenvolvedor do aplicativo original, pois são executados com a identidade do aplicativo principal (diferente das extensões de aplicativo). Dependendo de como você define o pacote opcional, você pode carregar código, ativos ou código e ativos do pacote opcional para o aplicativo principal. Se você precisar aprimorar seu aplicativo com conteúdo que possa ser monetizadas, licenciado e distribuído separadamente, os pacotes opcionais podem ser a escolha certa para você. 

Para obter mais detalhes, consulte [pacotes opcionais e criação de conjunto relacionado](optional-packages.md).

## <a name="app-streaming-install"></a>Instalação de streaming de aplicativo

A instalação de streaming é uma maneira de otimizar como seu aplicativo é entregue aos usuários. Em vez de aguardar até que o aplicativo inteiro seja baixado antes que você poder usá-lo, os usuários podem interagir com o aplicativo assim que uma parte necessária for baixada. Cabe a você, como um desenvolvedor, segmentar seu aplicativo em uma seção obrigatória para ativação básica e iniciar e conteúdo adicional para o restante do aplicativo. 

Para obter mais detalhes, consulte [instalação do streaming de aplicativos](streaming-install.md).

## <a name="flat-bundle-packages"></a>Pacotes de pacote simples

Pacotes de aplicativos de pacote simples são semelhantes aos pacotes de [aplicativos regulares](packaging-uwp-apps.md#types-of-app-packages), exceto que, em vez de incluir todos os pacotes de aplicativos dentro da pasta, o pacote simples contém apenas *referências* a esses pacotes de aplicativos. Por conter referências aos pacotes de aplicativo em vez dos próprios arquivos, um pacote simples reduz o tempo necessário para empacotar e baixar um aplicativo.

Para obter mais detalhes, consulte [pacotes de aplicativos de pacote simples](flat-bundles.md).

## <a name="asset-packages"></a>Pacotes de ativos

Os pacotes de ativos são uma fonte comum e centralizada de arquivos executáveis ou não executáveis para uso pelo seu aplicativo. Normalmente, esses arquivos não são do processador ou específicos do idioma. Por exemplo, isso pode incluir uma coleção de fotos no pacote de um ativo e vídeos em outro pacote de ativo, que são usados pelo aplicativo. Se seu aplicativo dá suporte a várias arquiteturas e a vários idiomas, esses ativos podem ser incluídos no pacote de arquitetura ou no pacote de recursos, mas isso também significa que os ativos seriam duplicados várias vezes em vários pacotes de arquitetura, ocupando espaço em disco. Se os pacotes de ativos forem usados, eles devem ser incluídos somente no pacote do aplicativos uma vez. 

Para obter mais detalhes, consulte [introdução aos pacotes de ativos](asset-packages.md).

## <a name="resource-packages"></a>Pacotes de recursos

Pacotes de recursos são apenas pacotes de ativos que permitem ao aplicativo se adaptar a vários tamanhos de tela e idiomas do sistema. O pacote de recursos visa o idioma do usuário, a escala do sistema e os recursos do DirectX, permitindo que o aplicativo seja adaptado para diversos cenários de usuário. Embora um pacote do aplicativo possa conter vários recursos, o sistema operacional baixa apenas os recursos relevantes por dispositivo do usuário, economizando espaço em disco e largura de banda.

## <a name="app-extensions"></a>Extensões de aplicativos

[As extensões de aplicativo](/uwp/api/windows.applicationmodel.appextensions) permitem que seu aplicativo hospede o conteúdo fornecido por outros aplicativos. Descubra, enumere e acesse conteúdo somente leitura desses aplicativos.

Se um aplicativo oferece suporte a extensões, qualquer desenvolvedor pode enviar uma extensão para o aplicativo. Assim, o aplicativo host precisa ser robusto quando carrega uma extensão com a qual ainda não foi testada previamente. As extensões devem ser consideradas não confiáveis.

Os aplicativos não podem carregar o código a partir das extensões. Se você precisar de execução de código, considere os serviços de aplicativos.

## <a name="app-services"></a>Serviços de Aplicativos

Os serviços de aplicativos do Windows permitem a comunicação entre aplicativos, permitindo que seu aplicativo forneça serviços para outro aplicativo. Os serviços de aplicativo permitem que você crie serviços sem interface do usuário que os aplicativos podem chamar no mesmo dispositivo e, a partir do Windows 10, versão 1607, nos dispositivos remotos. Consulte [Criar e consumir um serviço de aplicativo](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) para obter mais detalhes.

Os serviços de aplicativo são análogos aos serviços Web em um dispositivo. Um serviço de aplicativo é executado como uma tarefa em segundo plano no aplicativo host e pode fornecer seu serviço a outros aplicativos. Por exemplo, um serviço de aplicativo pode fornecer um serviço de scanner de código de barras que outros aplicativos podem usar. Ou talvez um conjunto de aplicativos Enterprise tenha um serviço de verificação ortográfica que está disponível para os outros aplicativos no pacote.

## <a name="modification-packages"></a>Pacotes de modificação 
Os pacotes de modificação permitem que os profissionais de ti personalizem os aplicativos sem precisar reempacotar. No Windows 10 versão 1809, apresentamos um novo tipo de pacote MSIX chamado de *pacote de modificação*. Os pacotes de modificação também podem ser plug-ins/complementos que podem não ter um ponto de ativação. Os profissionais de ti podem usar esse recurso para alterar de maneira flexível os contêineres de MSIX para que os aplicativos sejam sobrepostos pelas personalizações da sua empresa. 

## <a name="see-also"></a>Consulte Também

[Criar e consumir um serviço de aplicativo](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Introdução aos pacotes de ativo](asset-packages.md)  
[Criação do pacote com o layout de empacotamento](packaging-layout.md)  
[Criação de pacotes opcionais e conjunto relacionado](optional-packages.md)  
[Desenvolvendo com os pacotes de ativo e dobramento de pacote](package-folding.md)  
[Instalação de streaming de aplicativo](streaming-install.md)  
[Pacotes de aplicativo do pacote simples](flat-bundles.md)  
[Namespace Windows.ApplicationModel.AppService](/uwp/api/Windows.ApplicationModel.AppService)  
[Namespace Windows.ApplicationModel.Extensions](/uwp/api/windows.applicationmodel.appextensions)