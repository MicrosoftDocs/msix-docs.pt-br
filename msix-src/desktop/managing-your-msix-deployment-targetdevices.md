---
Description: Este artigo oferece detalhes que precisam ser considerados durante a implantação de pacotes MSIX em dispositivos Windows em um ambiente empresarial.  Este artigo destina-se a profissionais de TI e corporativos.
title: Planeje a implantação.
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, implantação, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 48b9c6661e546f515bfc7de23bed30777498223f
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074212"
---
# <a name="plan-for-your-deployment"></a>Planejar a implantação

Não importa se seu objetivo é o mercado consumidor ou empresarial, a chave para a distribuição bem-sucedida é conhecer os dispositivos visados pela implantação. Dependendo da plataforma desejada, você poderá se deparar com diversas dependências a serem solucionadas. Algumas empresas têm um único sistema operacional distribuído por toda a organização. Outras têm uma coleção mista de hardware e sistemas operacionais. Para ter sucesso em um ambiente misto, é importante criar uma solução que seja instalada com facilidade em todos os sistemas operacionais e ainda limite as variações de tecnologias de instalador. 

Todos os desenvolvedores também precisam saber os requisitos mínimos de suporte aos sistemas operacionais desejados.  Ter como objetivo o denominador comum mais baixo do sistema operacional pode fazer com que você tenha o melhor potencial de alcance, mas, com frequência, versões anteriores do sistema operacional podem não ser compatíveis com determinadas chamadas de API com as quais o aplicativo foi desenvolvido.

## <a name="msix-platform-support"></a>Suporte para Plataforma MSIX
O MSIX foi introduzido no Windows 10 versão 1709 (10.0.16299.0) e posteriores.  Isso significa que, se você estiver usando a funcionalidade básica do MSIX e visando o Windows 10 versão 1709 ou posteriores, ele funcionará.  Para obter uma lista completa de recursos e sistemas operacionais compatíveis, veja [Plataformas compatíveis.](../supported-platforms.md)

## <a name="services-packaged-in-msix"></a>Serviços empacotados no MSIX
A capacidade de empacotar serviços no MSIX foi introduzida no Windows 10 Client 2004 (10.0.19041.0) e posteriores. Portanto, se o aplicativo estiver usando serviços empacotados em MSIX, ele ficará limitado à implantação nesses sistemas operacionais. Para saber mais sobre como usar os Serviços de Pacote MSIX no MSIX, consulte [Converter um instalador que inclui os serviços.](https://docs.microsoft.com/windows/msix/packaging-tool/convert-an-installer-with-services)

## <a name="server-support-for-msix-packages"></a>Suporte a servidor para Pacotes MSIX
O MSIX não é integrado ao Windows Server.  No entanto, o MSIX é compatível com o Window 10 Server com Experiência Desktop, builds 1709 e posteriores, quando o [aplicativo AppInstaller](https://www.microsoft.com/p/app-installer/9nblggh4nns1) está instalado.  Se o seu objetivo forem builds anteriores do servidor, será necessário instalar também o MSIX Core.  Para obter informações sobre o MSIX Core, veja [MSIX Core.](../msix-core/msixcore.md)

## <a name="windows-10-1703-and-earlier-support-for-msix-packages"></a>Suporte do Windows 10 1703 e anteriores a Pacotes MSIX
Se o seu objetivo forem versões anteriores do Windows, antes do Windows 10 Client 1709 (10.0.16299.0), será necessário usar o [MSIX Core.](https://docs.microsoft.com/windows/msix/msix-core/msixcore) Ao instalar o MSIX Core em versões anteriores do Windows, você poderá implantar e executar aplicativos MSIX. 

Para obter uma lista completa de recursos e sistemas operacionais compatíveis, veja [Plataformas compatíveis.](../supported-platforms.md) 

## <a name="upgrade-downgrade-and-architecture-considerations"></a>Considerações sobre atualização, downgrade e arquitetura
Os pacotes MSIX podem ser atualizados, passar por downgrade ou reparados quando o pacote original for reinstalado.  Para fins de eficiência, quando faz o downgrade, o MSIX faz uma atualização diferencial, o que significa que não há necessidade de baixar novamente a carga.

Ao atualizar um pacote existente, há alguns fatores adicionais que devem ser considerados.  Os pacotes MSIX podem ser específicos da arquitetura.  Embora seja possível atualizar e fazer downgrade entre arquiteturas, como mostrado na tabela abaixo, não é possível reinstalar a mesma versão de diferentes arquiteturas.  

|Instalado (versão) |  Atualizar ou reinstalar a versão  | Comportamento |    Resultado|
| :---------------: | :-----------------------: | :----------------------:|    :----------------------:|  
| x86 (1.0)   |      x86 (1.0)              | Reinstalar | Suportado
| x86 (1.0)   |      x86 (3.0)              | Atualizar, Atualização (Upgrade) | Suportado
| x86 (1.0)  |      x64 (1.0)              |  Reinstalar |<b>Sem suporte</b>
| x86 (1.0)  |      x64 (3.0)              |  Atualizar, Atualização (Upgrade) | Suportado
| x86 (3.0)   |      x86 (1.0)              | Fazer downgrade | Suportado
| x86 (3.0)  |      x64 (1.0)              |  Fazer downgrade | Suportado

### <a name="downgrade"></a>Fazer downgrade
Ao desinstalar ou fazer downgrade do MSIX, ele preservará os AppData do usuário.  Por isso, é importante observar que, a não ser que os dados criados pelo aplicativo mais recente sejam compatíveis com versões anteriores, o acesso aos dados com o aplicativo que passou por downgrade pode representar um problema.  Se os dados não forem compatíveis com versões anteriores, talvez você não queira permitir que o usuário faça o downgrade.

Para saber mais sobre como é possível controlar as configurações de atualização dos aplicativos, veja [Definir as configurações de atualização no arquivo do Instalador de Aplicativo](https://docs.microsoft.com/windows/msix/packaging-tool/convert-an-installer-with-services).

### <a name="msix-bundles"></a>Grupos MSIX
Os grupos MSIX são pacotes projetados para conter diversas arquiteturas.  Os pacotes MSIX, por outro lado, têm suporte para apenas uma arquitetura.  Os grupos MSIX podem ser usados para atualizar ou fazer downgrade de pacotes MSIX, mas o inverso não é possível.  Não é possível atualizar ou fazer o downgrade de um grupo MSIX com um pacote MSIX. 

Para saber mais sobre a criação de grupos, veja [Agrupar pacotes de MSIX](https://docs.microsoft.com/windows/msix/packaging-tool/bundle-msix-packages).
