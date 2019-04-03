---
title: Configurações de ferramenta de empacotamento MSIX duplicadas em vários dispositivos
description: Saiba como duplicar as configurações de ferramenta de empacotamento MSIX em vários dispositivos
author: dianmsft
ms.author: diahar
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: ea3f1d5b49be685315eda55c2fbb96704568337f
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900268"
---
# <a name="duplicate-msix-packaging-tool-settings-across-multiple-devices"></a>Configurações de ferramenta de empacotamento MSIX duplicadas em vários dispositivos 

Siga estas etapas simples para duplicar as configurações para a ferramenta de empacotamento MSIX em vários dispositivos, ou para fazer backup e restaurar as configurações se você precisar recriar a imagem de um dispositivo. Esse recurso está disponível começando na versão 1.2019.110.0 da ferramenta de empacotamento MSIX. 

## <a name="build-the-baseline"></a>Criar a linha de base

1. Em um dispositivo com a ferramenta de empacotamento MSIX instalado, clique as ícone de configurações.
2. Defina as configurações para atender os requisitos específicos do seu ambiente.
    > [!NOTE]
    > Isso geralmente significa que ajustar as exclusões de arquivos e do registro, mas pode incluir qualquer uma das configurações.
3. Depois de configurar as configurações, certifique-se de clicar em Salvar configurações e, em seguida, feche a ferramenta.  

## <a name="back-up-the-settings-configuration"></a>Faça backup da configuração de configurações

Do dispositivo com a linha de base configurada, copie o seguinte arquivo em um local de BOM conhecido, como o compartilhamento de rede do servidor.

* C:\Users\\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml  

> [!NOTE]
> Verifique se esse arquivo não é excluído ou alterado, pois ela será seu modelo para todas as outras instalações.

## <a name="restore-the-settings-configuration"></a>Restaurar a configuração de configurações

Quando um novo dispositivo estiver configurado com a ferramenta de empacotamento MSIX instalado, copie o Settings. XML do local conhecido bom volta para o dispositivo local: 

* C:\Users\\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml 

## <a name="considerations"></a>Considerações

* Verifique se o <DefaultSaveLocation> entrada é removida ou é um local válido para um usuário. Se o valor não for fornecido ou o local é inválido, a ferramenta usará como padrão C:\Users\\username%\Desktop %.

* Arquivos de configurações devem ser copiados somente para a mesma versão da ferramenta de empacotamento ou uma versão posterior. Configurações não podem ser cumpridas se a versão em Settings. XML é copiada para uma versão mais antiga da ferramenta. É recomendável que você sempre use a versão mais recente da ferramenta para obter as correções mais recentes e para a melhor experiência de empacotamento.  
