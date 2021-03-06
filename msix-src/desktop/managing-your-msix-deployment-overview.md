---
description: Este artigo tem todos os detalhes de que você precisa para gerenciar a implantação de aplicativos MSIX em um ambiente empresarial.  Este artigo destina-se a profissionais de TI e corporativos.
title: Visão geral do gerenciamento da implantação MSIX
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, implantação, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: f775895bebfba77b649f1e52aada2bb9b8f88ecb
ms.sourcegitcommit: 4ecff6f1386c6239cfd79ddf82265f4194302bbb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92686833"
---
# <a name="manage-your-msix-deployment"></a>Gerenciar a implantação do MSIX

Empacotar seu aplicativo é apenas metade do caminho. Em seguida, você precisa poder implantar seu aplicativo para os usuários. A maneira como você implanta o aplicativo depende do cliente.  Esta seção, Gerenciamento da implantação MSIX, falará sobre a implantação de pacotes do MSIX para mercados corporativos e de varejo. Ela fornecerá links e dicas e truques para garantir uma experiência bem-sucedida. 

Para implantar o MSIX com êxito, é necessário considerar o seguinte:
* Quem é o meu cliente?
* Quais são as minhas dependências?
* Como fornecerei o melhor suporte para o meu cliente?

## <a name="who-is-my-customer"></a>Quem é o meu cliente?
A maneira como você implanta muitas vezes depende de quem é o cliente e sua função como desenvolvedor ou administrador.   É importante identificar sua função para saber quais ferramentas usar.

### <a name="it-pros"></a>Profissionais de TI
Normalmente, os profissionais de TI usam um sistema de gerenciamento de software para instalar e gerenciar os aplicativos.  Na seção [Distribuir o MSIX em um ambiente corporativo](managing-your-msix-deployment-enterprise.md), falaremos sobre:
* Como usar o Microsoft Endpoint Configuration Manager e o Intune para gerenciar seus aplicativos
* Uso do provisionamento e do DISM para configurar previamente os computadores com MSIX
* Controle do acesso ao aplicativo com Políticas de Grupo e AppLocker
* Distribuição de aplicativos pela Web ou pela Microsoft Store para Empresas
* Usar o AppCenter para criar, testar, lançar e aplicativos de área de trabalho
 
### <a name="developers"></a>Desenvolvedores
Os desenvolvedores têm diferentes ferramentas para distribuir os aplicativos.  Na seção [Distribuir o MSIX em um ambiente de varejo](managing-your-msix-deployment-retail.md) falaremos sobre:  
* Microsoft Store
* Instalador da Web
* Usar o AppCenter para criar, testar, lançar e aplicativos de área de trabalho

## <a name="dependencies"></a>Dependências
O MSIX é uma experiência de instalação de aplicativo moderna e confiável. A experiência do MSIX tem uma taxa de sucesso de instalação de 99,96%.  Mas há algumas limitações. Não há suporte para MSIX por padrão em todas as versões do Windows e o conjunto de recursos com suporte pode depender da versão do Windows 10 que você está implantando.  Na seção [Planeje a implantação](managing-your-msix-deployment-targetdevices.md), falaremos sobre a importância de entender o dispositivo de destino no qual o MSIX será implantado. 

## <a name="providing-support-for-my-customer"></a>Fornecendo suporte para seus clientes
Embora o MSIX tenha uma taxa de sucesso de instalação de 99,96%, você ainda precisa planejar como dar suporte ao seu cliente.  Na seção [Solução de problemas e validação de MSIX](managing-your-msix-deployment-troubleshooting.md) falamos sobre as ferramentas para diagnosticar problemas de instalação.


 
