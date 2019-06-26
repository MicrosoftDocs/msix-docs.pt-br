---
title: Duplicar as configurações da Ferramenta de Empacotamento MSIX entre vários dispositivos
description: Saiba como duplicar as configurações da Ferramenta de Empacotamento MSIX entre vários dispositivos
author: dianmsft
ms.author: diahar
ms.date: 02/06/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.openlocfilehash: ea3f1d5b49be685315eda55c2fbb96704568337f
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "59983428"
---
# <a name="duplicate-msix-packaging-tool-settings-across-multiple-devices"></a>Duplicar as configurações da Ferramenta de Empacotamento MSIX entre vários dispositivos 

Siga estas etapas simples para duplicar as configurações para a Ferramenta de Empacotamento MSIX em vários dispositivos ou para fazer backup e restaurar as configurações, caso precise restaurar ao estado anterior de um dispositivo. Esse recurso está disponível na versão 1.2019.110.0 em diante da Ferramenta de Empacotamento MSIX. 

## <a name="build-the-baseline"></a>Criar a linha de base

1. Em um dispositivo com a Ferramenta de Empacotamento MSIX instalada, clique no ícone de Configurações.
2. Defina as configurações para atender aos requisitos específicos do ambiente.
    > [!NOTE]
    > Isso geralmente significa um ajuste das exclusões de arquivo e do Registro, mas pode incluir qualquer uma das configurações.
3. Depois de definir as configurações, clique em Salvar configurações e, em seguida, feche a ferramenta.  

## <a name="back-up-the-settings-configuration"></a>Fazer backup da definição das configurações

No dispositivo com a linha de base configurada, copie o arquivo a seguir em uma localização válida conhecida, como o compartilhamento do servidor de rede.

* C:\Users\\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml  

> [!NOTE]
> Garanta que esse arquivo não seja excluído nem alterado, pois ele será o modelo para todas as outras instalações.

## <a name="restore-the-settings-configuration"></a>Restaurar a definição das configurações

Quando um novo dispositivo for configurado com a Ferramenta de Empacotamento MSIX instalada, copie o settings.xml da localização válida conhecida novamente para o dispositivo local: 

* C:\Users\\%username%\AppData\Local\Packages\Microsoft.MsixPackagingTool_8wekyb3d8bbwe\LocalState\settings.xml 

## <a name="considerations"></a>Considerações

* Garanta que a entrada <DefaultSaveLocation> seja removida ou seja uma localização válida para um usuário. Se o valor não for fornecido ou a localização for inválida, a ferramenta usará como padrão C:\Users\\%username%\Desktop.

* Os arquivos de configurações devem ser copiados somente para a mesma versão da ferramenta de empacotamento ou uma versão posterior. As configurações não poderão ser respeitadas se a versão em settings.xml for copiada para uma versão mais antiga da ferramenta. Recomendamos que você sempre use a última versão da ferramenta para obter as correções mais recentes e a melhor experiência de empacotamento.  
