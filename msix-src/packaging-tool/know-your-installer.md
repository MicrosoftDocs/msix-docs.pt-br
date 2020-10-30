---
title: Conheça o instalador
description: Este artigo lista as coisas que você precisa saber antes de empacotar seu aplicativo de desktop. Talvez não seja necessário fazer muito para preparar o aplicativo para o processo de empacotamento.
ms.date: 01/29/2020
ms.topic: article
keywords: MSIX
ms.localizationpriority: medium
ms.openlocfilehash: cacff2c9ee0cdf0e7b812fe18a767afdfdef8569
ms.sourcegitcommit: 756699a3652540fca31b444179de38ebc23cb277
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93036844"
---
# <a name="know-your-installer"></a>Conheça o instalador

Este artigo lista as coisas que você precisa saber antes de converter o instalador existente em um MSIX. Talvez você não precise fazer muito para preparar o aplicativo para o processo de empacotamento, mas se qualquer um dos itens a seguir se aplicar ao seu aplicativo, você precisará solucioná-lo antes do empacotamento.

+ __Seu aplicativo tem um serviço__ . Damos suporte à conversão de [aplicativos com serviços](convert-an-installer-with-services.md), mas é importante ter em mente as [limitações](convert-an-installer-with-services.md#known-limitations) para converter um serviço. Após a conversão, você precisará de elevação de administrador para instalar o MSIX que contém um serviço. Você pode converter um aplicativo com serviços que começam na versão 1.2019.1220.0 da ferramenta de empacotamento MSIX, e você pode implantar o MSIX com os serviços que começam na versão 2020 de Spring do Windows 10.

+ __O instalador requer uma reinicialização__ . Se o instalador exigir uma [reinicialização](support-restart.md), isso terá suporte na ferramenta de empacotamento MSIX a partir da versão 1.2019.701.0. Se o seu instalador retornar um código de saída incomum para indicar que precisa de uma reinicialização, você deverá adicioná-lo à seção [reiniciar códigos de saída](tool-best-practices.md#other-settings) das configurações da ferramenta de empacotamento MSIX. 

+ __Seu aplicativo .NET precisa de uma versão do .NET Framework anterior à 4.6.2__ . Se estiver empacotando um aplicativo .NET, recomendamos que seu aplicativo dê preferência ao .NET Framework 4.6.2 ou posterior. A capacidade de instalar e executar aplicativos da área de trabalho empacotados apareceu pela primeira vez no Windows 10, versão 1607 (também chamada de Atualização de Aniversário) e essa versão do sistema operacional inclui o .NET Framework 4.6.2 por padrão. Versões posteriores do sistema operacional incluem versões posteriores do .NET Framework. Para obter uma lista completa das versões do .NET incluídas nas versões posteriores do Windows 10, confira [este artigo](/dotnet/framework/migration-guide/versions-and-dependencies).

  Espera-se que a preferência por versões do .NET Framework anteriores à 4.6.2 funcione na maioria dos casos. No entanto, caso dê preferência a uma versão anterior à 4.6.2, deverá testá-la completamente no aplicativo de área de trabalho empacotado antes da distribuição para os usuários.

  + 4.0 - 4.6.1: Espera-se que esses aplicativos que visam essas versões do .NET Framework funcionem sem problemas na versão 4.6.2 ou posteriores. Portanto, esses aplicativos devem ser instalados e executados sem alterações no Windows 10, versão 1607 ou posteriores com a versão do .NET Framework incluída no sistema operacional.

  + 2.0 e 3.5: Em nossos testes, os aplicativos de área de trabalho empacotados que visam essas versões do .NET Framework geralmente funcionam, mas podem apresentar problemas de desempenho em alguns cenários. Para que esses aplicativos empacotados sejam instalados e funcionem, o [recurso .NET Framework 3.5](/dotnet/framework/install/dotnet-35-windows-10) precisa estar instalado no computador de destino (esse recurso também inclui o .NET Framework 2.0 e 3.0). Você também deve testar esses aplicativos totalmente depois de empacotá-los.

+ __Seu aplicativo requer um driver__ . MSIX não dá suporte a drivers. 

+ __O aplicativo grava na pasta AppData ou no Registro com a intenção de compartilhar os dados com outro aplicativo__ . Após a conversão, a pasta AppData é redirecionada para o armazenamento de dados de aplicativo local, que é um repositório particular para cada aplicativo.

  Todas as entradas que o aplicativo grava para o hive de Registro HKEY_LOCAL_MACHINE são redirecionadas para um arquivo binário isolado e as possíveis entradas que o aplicativo grava no hive de Registro HKEY_CURRENT_USER são colocadas em um local privado por usuário e por aplicativo. Para obter mais detalhes sobre o redirecionamento de arquivo e Registro, consulte [Bastidores da Ponte de Desktop](../desktop/desktop-to-uwp-behind-the-scenes.md). 

 + __O aplicativo grava no diretório de instalação do aplicativo__ . Por exemplo, o aplicativo grava para um arquivo de log que você coloca no mesmo diretório do exe. Isso não tem suporte porque a pasta está protegida. É recomendável gravar em outro local, como o armazenamento de dados do aplicativo local. Adicionamos um recurso que permite isso em 1809 e posterior.

+ __Seu aplicativo usa o diretório de trabalho atual__ . No tempo de execução, o aplicativo da área de trabalho empacotado não terá o mesmo diretório de trabalho especificado anteriormente no atalho .LNK da área de trabalho. Você precisa alterar o CWD no tempo de execução caso ter o diretório correto for importante para que o aplicativo funcione corretamente.

+ __Seu aplicativo instala e carrega os assemblies da pasta lado a lado do Windows__ . Por exemplo, seu aplicativo usa as bibliotecas de tempo de execução C VC8 ou VC9 e está vinculando-as dinamicamente da pasta lado a lado do Windows, o que significa que seu código está usando os arquivos DLL comuns de uma pasta compartilhada, como C:\Windows\WinSxS. Isso não tem suporte. Você precisará vinculá-las de forma estática vinculando-as aos arquivos da biblioteca redistribuível diretamente no código.

## <a name="other-considerations"></a>Outras considerações 

+ __Reempacotando o instalador na arquitetura apropriada__ . Se o seu instalador se destina a ser instalado em um computador x86. Certifique-se de reempacotar o instalador em um computador x86. Isso é aplicável para o instalador destinado a computadores x64. 

  > [!NOTE]
  > Caso o aplicativo precise ser gravado no diretório de instalação ou usar o diretório de trabalho atual, também será possível considerar adicionar uma correção de tempo de execução usando a [Estrutura de Suporte do Pacote](https://github.com/microsoft/MSIX-PackageSupportFramework) do pacote. Para obter mais detalhes, veja [este artigo](../psf/package-support-framework.md).
