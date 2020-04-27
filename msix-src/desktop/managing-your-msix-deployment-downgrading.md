---
Description: Este artigo tem detalhes de como os aplicativos MSIX lidam com o downgrade.
title: Instalar versões anteriores de um pacote do aplicativo MSIX
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, implantação, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 9e6ab85924ae7120ba2540d6beaeb827a86b1764
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2020
ms.locfileid: "77074242"
---
# <a name="install-earlier-versions-of-an-msix-app-package"></a>Instalar versões anteriores de um pacote do aplicativo MSIX

A instalação de versões anteriores de um pacote do aplicativo MSIX pode ser realizada em um dispositivo sem remoção anterior do aplicativo. Acionar uma instalação de uma versão anterior do mesmo aplicativo fará com que o cliente analise o manifesto do aplicativo contido no pacote do aplicativo MSIX sendo instalado. A revisão do arquivo appxmanifest.xml identificará as diferenças Delta entre o aplicativo instalado atualmente e o instalador sendo executado no dispositivo. Esse processo mantém os dados armazenados na pasta *appdata*. As alterações feitas durante a instalação da versão atualizada não serão desfeitas caso uma versão anterior do pacote do aplicativo MSIX seja instalada.
