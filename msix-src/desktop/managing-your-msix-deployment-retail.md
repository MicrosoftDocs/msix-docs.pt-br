---
description: Este artigo tem todos os detalhes de que você precisa para gerenciar a implantação de aplicativos MSIX em um ambiente de varejo.  Este artigo destina-se a desenvolvedores.
title: Distribuir o MSIX em um ambiente do consumidor
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, implantação, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 62dc7479a888aa94156612cff103271b65bb4323
ms.sourcegitcommit: 3a9aae783331833bbf244091544c48848768137e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98041137"
---
# <a name="distribute-your-msix-in-a-consumer-environment"></a>Distribuir o MSIX em um ambiente do consumidor

Caso não seja um desenvolvedor empresarial, a distribuição de varejo do MSIX pode ser feita pelos canais normais de varejo.  Isso inclui hospedar o MSIX em uma página da Web.  

## <a name="microsoft-store"></a>Microsoft Store

A [Microsoft Store](https://www.microsoft.com/store/apps/windows) pode ser uma ótima maneira de distribuir o aplicativo.  Ela está disponível para os consumidores desde o lançamento do Windows 8, no final de 2012. Os usuários podem navegar, pesquisar e baixar aplicativos ou jogos para o Windows.

Para obter detalhes sobre a Microsoft Store e as funcionalidades que ela oferece, consulte [Microsoft Store.](/windows/uwp/publish/) 

Para saber como publicar um aplicativo na Microsoft Store e conhecer as funcionalidades que ela oferece, consulte o [Partner Center](https://partner.microsoft.com/dashboard/home)

## <a name="app-center"></a>App Center

O [App Center](https://appcenter.ms/) permite criar o aplicativo automaticamente, testá-lo em dispositivos reais e distribuí-lo para testadores beta.  O App Center permite enviar aplicativos com mais frequência, com qualidade mais alta e com maior confiança.  Com o App Center é possível conectar o repositório e, em alguns minutos, automatizar os builds, testar em dispositivos reais na nuvem, distribuir os aplicativos para testadores beta e monitorar a utilização no mundo real com dados de falha e de análise. Tudo isso em um só lugar.
Para obter mais informações sobre o App Center, consulte [App Center](/appcenter/).

## <a name="web-install"></a>Instalação na Web

Ainda que um arquivo MSIX possa ser hospedado em um servidor IIS (Serviços de Informações da Internet).  Você pode melhorar muito a experiência ao permitir a instalação direta com o Instalador de Aplicativo.  A instalação direta permitirá que o AppInstaller seja iniciado imediatamente e que a instalação seja executada pelo servidor Web em vez de baixar primeiro.  Para obter mais detalhes sobre a instalação na Web, veja [Instalação de aplicativos do Windows 10 por uma página da Web.](../app-installer/installing-windows10-apps-web.md)

