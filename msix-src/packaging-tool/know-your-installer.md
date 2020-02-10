---
title: Conheça o instalador
description: Este artigo lista as coisas que você precisa saber antes de empacotar seu aplicativo de desktop. Talvez você não precise fazer muito para preparar seu app para o processo de empacotamento.
ms.date: 01/29/2020
ms.topic: article
keywords: MSIX
ms.localizationpriority: medium
ms.openlocfilehash: 66577e142bfbc3c90ba9486b24463accf2c7f524
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073752"
---
# <a name="know-your-installer"></a>Conheça o instalador

Este artigo lista as coisas que você precisa saber antes de converter o instalador existente em um MSIX. Talvez você não precise fazer muito para preparar o aplicativo para o processo de empacotamento, mas se qualquer um dos itens a seguir se aplicar ao seu aplicativo, você precisará solucioná-lo antes do empacotamento.

+ __Seu aplicativo tem um serviço__. Damos suporte à conversão de [aplicativos com serviços](convert-an-installer-with-services.md), mas é importante ter em mente as [limitações](convert-an-installer-with-services.md#known-limitations) para converter um serviço. Após a conversão, você precisará de elevação de administrador para instalar o MSIX que contém um serviço. Você pode converter um aplicativo com serviços que começam na versão 1.2019.1220.0 da ferramenta de empacotamento MSIX, e você pode implantar o MSIX com os serviços que começam na versão 2020 de Spring do Windows 10.

+ __O instalador requer uma reinicialização__. Se o instalador exigir uma [reinicialização](support-restart.md), isso terá suporte na ferramenta de empacotamento MSIX a partir da versão 1.2019.701.0. Se o seu instalador retornar um código de saída incomum para indicar que precisa de uma reinicialização, você deverá adicioná-lo à seção [reiniciar códigos de saída](tool-best-practices.md#other-settings) das configurações da ferramenta de empacotamento MSIX. 

+ __Seu aplicativo .NET requer uma versão do .NET Framework anterior a 4.6.2__. Se você estiver empacotando um aplicativo .NET, recomendamos que o destino do aplicativo .NET Framework 4.6.2 ou posterior. A capacidade de instalar e executar aplicativos de desktop empacotados foi introduzida pela primeira vez no Windows 10, versão 1607 (também chamada de atualização de aniversário), e essa versão do sistema operacional inclui o .NET Framework 4.6.2 por padrão. Versões posteriores do so incluem versões posteriores do .NET Framework. Para obter uma lista completa de quais versões do .NET estão incluídas nas versões posteriores do Windows 10, consulte [Este artigo](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies).

  Espera-se que as versões de destino das .NET Framework anteriores à 4.6.2 em aplicativos de desktop empacotados funcionem na maioria dos casos. No entanto, se você visar uma versão anterior à 4.6.2, deverá testar completamente o aplicativo de área de trabalho empacotado antes de distribuí-lo aos usuários.

  + 4,0-4.6.1: os aplicativos destinados a essas versões do .NET Framework devem ser executados sem problemas no 4.6.2 ou posterior. Portanto, esses aplicativos devem ser instalados e executados sem alterações no Windows 10, versão 1607 ou posterior, com a versão do .NET Framework que está incluído no sistema operacional.

  + 2,0 e 3,5: em nossos testes, os aplicativos de área de trabalho empacotados que visam essas versões do .NET Framework geralmente funcionam, mas podem apresentar problemas de desempenho em alguns cenários. Para que esses aplicativos empacotados sejam instalados e executados, o [recurso .NET Framework 3,5](https://docs.microsoft.com/dotnet/framework/install/dotnet-35-windows-10) deve ser instalado no computador de destino (esse recurso também inclui .NET Framework 2,0 e 3,0). Você também deve testar esses aplicativos cuidadosamente depois de empacotá-los.

+ __Seu aplicativo requer um driver__. MSIX não dá suporte a drivers. 

+ __Seu aplicativo grava na pasta AppData ou no registro com a intenção de compartilhar dados com outro aplicativo__. Após a conversão, o AppData é redirecionado para o armazenamento de dados do aplicativo local, que é um armazenamento privado para cada aplicativo.

  Todas as entradas que seu aplicativo grava no HKEY_LOCAL_MACHINE hive do registro são redirecionadas para um arquivo binário isolado e as entradas que seu aplicativo grava no hive do registro de HKEY_CURRENT_USER são colocadas em um local privado por usuário e por aplicativo. Para obter mais detalhes sobre o redirecionamento de arquivos e do Registro, consulte [Bastidores da Ponte de Desktop](../desktop/desktop-to-uwp-behind-the-scenes.md). 

 + __Seu aplicativo grava no diretório de instalação para seu aplicativo__. Por exemplo, seu aplicativo grava em um arquivo de log que você colocou no mesmo diretório que o exe. Isso não tem suporte porque a pasta está protegida. É recomendável gravar em outro local, como o armazenamento de dados do aplicativo local. Adicionamos um recurso que permite isso em 1809 e posterior.

+ __Seu aplicativo usa o diretório de trabalho atual__. Em tempo de execução, o aplicativo de área de trabalho empacotado não obterá o mesmo diretório de trabalho que você especificou anteriormente na área de trabalho. O atalho LNK. Você precisa alterar seu CWD em tempo de execução se tiver o diretório correto para que seu aplicativo funcione corretamente.

  > [!NOTE]
  > Se seu aplicativo precisar gravar no diretório de instalação ou usar o diretório de trabalho atual, você também pode considerar adicionar uma correção de tempo de execução usando a [estrutura de suporte do pacote](https://github.com/microsoft/MSIX-PackageSupportFramework) ao seu pacote. Para obter mais detalhes, consulte [Este artigo](../psf/package-support-framework.md).  