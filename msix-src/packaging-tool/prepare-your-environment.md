---
title: preparar seu ambiente para conversão
description: Fornece uma visão geral de como converter um MSIX de um instalador existente.
ms.date: 01/27/2020
ms.topic: article
keywords: MSIX
ms.localizationpriority: medium
ms.openlocfilehash: 1b4a171039e4f442a17150154119aa4027669887
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073612"
---
# <a name="prepare-your-environment-for-conversion"></a>Preparar seu ambiente para conversão

Preparar seu ambiente de conversão é uma etapa importante no processo de conversão. As recomendações a seguir ajudarão a garantir o seu sucesso quando você estiver convertendo os instaladores existentes no MSIX.

- O requisito mínimo de versão do sistema operacional para a ferramenta de empacotamento MSIX é o Windows 10 1809. Entendemos que nem todo mundo está usando a Atualização de outubro de 2018 para o Windows 10 ou nem mesmo o Windows 10. Portanto, recomendamos que você crie uma VM limpa pré-configurada para a versão mínima do suporte para a ferramenta de empacotamento MSIX. A implantação do pacote MSIX gerado tem requisitos de suporte diferentes.

- Uma máquina limpa para conversão é importante porque, durante a etapa de instalação da ferramenta de empacotamento MSIX, escutaremos tudo no ambiente para capturar o que o instalador está fazendo. Uma máquina limpa significa que não há aplicativos ou serviços em execução no computador que possam ser capturados em seu pacote.

- É recomendável configurar o computador de conversão para imitar o ambiente em que o pacote MSIX será executado, portanto, se houver serviços ou políticas que estarão lá, você poderá testar se o pacote realmente funcionará.

- Seu ambiente de conversão deve corresponder à arquitetura de onde você implantará o aplicativo. Por exemplo, se você for implantar o pacote MSIX em um computador x64, deverá executar a conversão em um computador x64. 

- Se não for algo que você tenha, oferecemos uma **VM de criação rápida**, o [ambiente da ferramenta de empacotamento MSIX](quick-create-vm.md) no Hyper-V, que está pronto para ser convertido com o Windows 10 1809 e a versão mais recente da ferramenta de empacotamento MSIX. 

- Siga as recomendações de práticas recomendadas para [Configurar a ferramenta de empacotamento MSIX](tool-best-practices.md) (ou a ferramenta de sua escolha) e, em seguida, crie um ponto de verificação para a VM. Dessa forma, você pode usar a VM para converter, reverter para o ponto de verificação anterior e ela será uma máquina limpa e configurada pronta para conversão novamente ou para verificar se o pacote MSIX foi convertido com êxito.

- Também é bom saber que tipo de dependências você tem para que possa entender quais devem ser executadas com o seu aplicativo e quais devem ser empacotadas como um pacote de modificação. Por exemplo, se você tem dependências de runtime, é uma boa ideia incluí-las em seu aplicativo principal. Se você tiver um plug-in, deverá empacotá-lo como um pacote de modificação para associá-lo ao aplicativo principal.

- Se você quiser executar a conversão em um computador remoto, será necessário fazer algumas [configurações](remote-conversion-setup.md) adicionais para habilitá-lo para conversão.
