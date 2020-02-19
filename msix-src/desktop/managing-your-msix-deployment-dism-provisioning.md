---
Description: Este artigo descreve diferentes formas de instalar um aplicativo MSIX para todos os usuários.
title: Instalar o aplicativo para todos os usuários
ms.date: 02/04/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e20d16fb5c25bc76ecccab9dd6a2b59cc1dc39a6
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77073972"
---
# <a name="install-your-app-for-all-users-with-dism"></a>Instalar o aplicativo para todos os usuários com DISM

Há várias ferramentas que podem ser usadas para instalar um aplicativo MSIX para todos os usuários. [Gerenciamento e Manutenção de Imagens de Implantação (DISM.exe)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism---deployment-image-servicing-and-management-technical-reference-for-windows) é uma ferramenta de linha de comando que pode ser usada para fazer a manutenção e preparar imagens do Windows, incluindo as usadas pelo Windows PE, Windows Recovery Environment (Windows RE) e Windows Setup. O DISM pode ser usado para fazer a manutenção de uma imagem do Windows (.wim) ou um disco rígido virtual (.vhd ou .vhdx).

O provisionamento do Windows facilita para os profissionais de TI configurar os dispositivos do usuário final sem geração de imagens. Com o provisionamento do Windows, os profissionais de TI podem especificar facilmente a configuração desejada e as configurações necessárias para registrar os dispositivos no gerenciamento e aplicar essa configuração a dispositivos de destino em questão de minutos. Por exemplo, instalar um aplicativo para todos os usuários em um dispositivo. 

A partir do Windows versão 1809, os profissionais de TI podem pré-instalar com provisionamento.   Os aplicativos provisionados, serão instalados em um local central %ProgramFiles%\WindowsApps e estarão imediatamente disponíveis para os usuários registrados. Os usuários que não tiverem o MSIX registrado na conta não terão acesso.  
Para obter mais detalhes sobre o provisionamento de pacotes, veja [Provisionamento de pacotes para o Windows 10](https://docs.microsoft.com/windows/configuration/provisioning-packages/provisioning-packages).

## <a name="user-preferences-when-provisioning"></a>Preferências do usuário ao provisionar

O provisionamento respeita as configurações dos usuários e, portanto, quando um usuário desinstala um aplicativo provisionado, a única maneira de o usuário tê-lo de volta é reinstalando-o. No Windows 10 versão 2004, um aplicativo provisionado substituirá automaticamente a configuração dos usuários e reinstalará o aplicativo quando for reprovisionado.

Você também deve ter cuidado para impedir que os usuários instalem e atualizem versões diferentes do pacote MSIX provisionado. Se o usuário atualizar ou fizer downgrade de um aplicativo provisionado, instalando o mesmo pacote da loja, você poderá obter resultados inesperados.
Para obter mais detalhes sobre o provisionamento do MSIX, veja [Criar aplicativos do Windows no Gerenciador de Configurações](https://docs.microsoft.com/configmgr/apps/get-started/creating-windows-applications).

## <a name="removing-apps-for-all-users"></a>Remover aplicativos para todos os usuários

Além de instalar aplicativos, também é possível desinstalar um aplicativo para todos os usuários. A maneira de fazer isso é pela API do Gerenciador de Pacotes, com o método [PackageManager.RemovePackageAsync](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager.removepackageasync).

Como alternativa, você pode usar o método **AppxRemovePackageForAllUsers(PCWSTR packageFullName, BOOL* reserved)()* * que pode ser encontrado no **AppxDeploymentClient. dll** que foi introduzido no Windows 10 versão 1703. No entanto, recomendamos o uso do **PackageManager.RemovePackageAsync**.
