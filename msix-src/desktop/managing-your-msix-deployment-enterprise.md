---
Description: Este artigo tem todos os detalhes de que você precisa para gerenciar a implantação de aplicativos MSIX em um ambiente empresarial.  Este artigo destina-se a profissionais de TI e corporativos.
title: Distribuir o MSIX em um ambiente corporativo
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, implantação, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: b0cabc375e89e0c811826c04798aed252d4a6dc1
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2020
ms.locfileid: "81613992"
---
#   <a name="msix-app-distribution"></a>Distribuição de aplicativo MSIX
O formato de empacotamento pode ser distribuído para dispositivos cliente com ferramentas de gerenciamento de dispositivos e aplicativos, como o Microsoft Intune e o Microsoft Endpoint Configuration Manager. 

##  <a name="microsoft-endpoint-configuration-manager"></a>Microsoft Endpoint Configuration Manager 

Como o MSIX é um formato padronizado de empacotamento de instalação, os detalhes relacionados ao aplicativo (Editor, Nome do aplicativo e Versão) serão recuperados e apresentados automaticamente para revisão pelo assistente de criação de aplicativo no Microsoft Endpoint Configuration Manager. De forma semelhante, a cadeia de caracteres de instalação e os métodos de detecção usados com os aplicativos MSIX são constantes e configurados automaticamente pelo assistente de criação de aplicativos do Microsoft Endpoint Configuration Manager.

Ao criar um aplicativo no Microsoft Endpoint Configuration Manager, selecione o tipo de aplicativo: **pacote do aplicativo Windows (* .appx, *.appxbundle, *.msix, *.msixbundle)**. Para obter orientação sobre como criar e implantar um aplicativo por meio do Microsoft Endpoint Configuration Manager, confira [criar e implantar um aplicativo](https://docs.microsoft.com/configmgr/apps/get-started/create-and-deploy-an-application).

## <a name="microsoft-intune"></a>Microsoft Intune

O Microsoft Intune é compatível com a implantação de aplicativos MSIX em dispositivos cliente por meio do modelo de aplicativo cliente. Como o MSIX é um formato padronizado de empacotamento de instalação, os detalhes relacionados ao aplicativo (Nome do aplicativo, Descrição e Editor) são preenchidos automaticamente nas informações do Aplicativo.

A instalação de um aplicativo MSIX é padronizada. Sendo assim, ao adicionar um novo aplicativo de linha de negócios ao Microsoft Intune, não há requisitos para configurar os parâmetros de instalação silenciosa necessários para a instalação. Para obter orientação sobre como criar e implantar um aplicativo por meio do Microsoft Intune, confira [Criação de aplicativos de linha de negócios no Intune](https://docs.microsoft.com/mem/intune/apps/lob-apps-windows).

## <a name="web-app-installer"></a>Web (Instalador de aplicativo)

O MSIX pode ser implantado com um servidor IIS (Serviços de Informações da Internet).  Se você adicionar o protocolo ms-appinstaller; ele criará uma experiência de instalação muito melhor.  
Para saber como distribuir um arquivo MSIX no IIS e como configurar o servidor IIS para oferecer suporte à distribuição de aplicativos MSIX, veja [Distribuir um aplicativo Windows 10 por um servidor IIS.](https://docs.microsoft.com/windows/msix/app-installer/web-install-iis)

## <a name="microsoft-store-for-business"></a>Microsoft Store para Empresas

[O Microsoft Store for Business](https://businessstore.microsoft.com/store) é um armazenamento projetado especificamente para distribuição de aplicativos de Educação e Negócios. É possível usar a Microsoft Store para encontrar, adquirir, distribuir e gerenciar aplicativos de sua organização ou escola.  Para obter detalhes sobre a Microsoft Store para Empresas, veja [Microsoft Store para Empresas e Educação.](https://docs.microsoft.com/microsoft-store/)

## <a name="app-center"></a>App Center

O [App Center](https://appcenter.ms/) permite criar o aplicativo automaticamente, testá-lo em dispositivos reais e distribuí-lo para testadores beta.  O App Center permite enviar aplicativos com mais frequência, com qualidade mais alta e com maior confiança.  Com o App Center é possível conectar o repositório e, em alguns minutos, automatizar os builds, testar em dispositivos reais na nuvem, distribuir os aplicativos para testadores beta e monitorar a utilização no mundo real com dados de falha e de análise. Tudo isso em um só lugar.

## <a name="deployment-image-servicing-and-management-dismexe-and-provisioning"></a>Gerenciamento e provisionamento de imagens de implantação (DISM.exe)

### <a name="dism"></a>DISM
Os profissionais de TI podem usar os cmdlets DISM (Gerenciamento de imagens de implantação) para instalar, desinstalar e configurar pacotes MSIX em uma imagem do Windows antes da implantação.  
Para saber mais sobre o provisionamento, veja [Gerenciamento, manutenção e provisionamento de imagens de implantação.](deploy-preinstalled-apps.md)

### <a name="provisioning"></a>Provisionamento
Os profissionais de TI usam o provisionamento para configurar dispositivos de usuário final sem precisar gerar a imagem novamente.  Os profissionais de TI podem pré-instalar pacotes MSIX nos sistemas dos usuários finais.
Para saber mais sobre o provisionamento, veja [Gerenciamento, manutenção e provisionamento de imagens de implantação.](deploy-preinstalled-apps.md)

## <a name="managing-your-msix-app"></a>Gerenciamento do aplicativo MSIX

Os pacotes MSIX têm um conjunto de controles abrangente que os profissionais de TI podem usar para controlar a instalação.  Os profissionais de TI podem decidir como e quando os aplicativos MSIX podem se atualizar, fazer downgrade ou desinstalar.  Os pacotes MSIX também podem ser limitados com serviços de caixa de entrada do Windows como AppLocker e Políticas de Grupo. 

### <a name="prevent-msix-app-installs-through-applocker"></a>Impedir que o aplicativo MSIX seja instalado pelo AppLocker

O suporte à capacidade de permitir ou negar a execução de aplicativos MSIX em um dispositivo corporativo é oferecida no [AppLocker](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview). Isso é feito pela definição de regras com base nos atributos do aplicativo MSIX. Esses atributos incluem: nome do editor e do produto, nome, versão, caminho e hash do arquivo. Os aplicativos MSIX identificados por essas regras são então configurados para permitir ou negar a execução.

Há vários métodos pelos quais é possível aproveitar o AppLocker em uma organização para controlar quais aplicativos podem ou não ser executados em um dispositivo corporativo. Para obter uma lista completa, veja [Trabalhando com regras do AppLocker](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/applocker/working-with-applocker-rules).

### <a name="manage-access-through-group-policy"></a>Gerenciar acesso pela Política de Grupo

As Políticas de Grupo oferecem configuração e gerenciamento centralizado de sistemas operacionais, aplicativos e configurações de usuários em um ambiente do Active Directory. Os aplicativos de pacotes MSIX podem ler as chaves de registro de política de grupo e respeitar as configurações de política de grupo.  
Para saber mais sobre o suporte e as limitações de MSIX no suporte às políticas de grupo, veja [Política de Grupo e aplicativos empacotados MSIX.](https://review.docs.microsoft.com/windows/msix/group-policy-msix)

### <a name="manage-msix-updates"></a>Gerenciar atualizações do MSIX

Configure o comportamento da atualização do aplicativo usando o arquivo do Instalador de Aplicativo.  Os profissionais de TI podem definir quando um usuário recebe atualizações em um MSIX e se a experiência de atualização será silenciosa.  O usuário poderá ser solicitado a atualizar ao iniciar ou postergar a atualização.    

Para saber mais sobre a Configuração de programação de atualização do MSIX, veja [Definir configurações de atualização no arquivo do Instalador de Aplicativo.](https://docs.microsoft.com/windows/msix/app-installer/update-settings)

#### <a name="downgrades"></a>Downgrades

O MSIX oferece suporte ao downgrade de aplicativos, portanto, o aplicativo não precisa ser desinstalado antes da instalação de uma versão mais antiga do mesmo aplicativo. Ao especificar ForceUpdateFromAnyVersion, o MSIX poderá ser revertido para uma versão anterior. Isso é útil caso um bug sério já tenha sido implantado.  

Para saber mais sobre ForceUpdateFromAnyVersion, veja [Definir configurações de atualização no arquivo do Instalador de Aplicativo.](https://docs.microsoft.com/windows/msix/app-installer/update-settings)

#### <a name="critical-updates"></a>Atualizações Críticas

Os usuários ocasionalmente ignoram prompts para atualizar o aplicativo.  Com o MSIX, os profissionais de TI podem forçar a atualização de um aplicativo marcando-a como crítica e especificando UpdateBlocksActivation.

Para saber mais sobre UpdateBlocksActivation, veja [Definir configurações de atualização no arquivo do Instalador de Aplicativo.](https://docs.microsoft.com/windows/msix/app-installer/update-settings)

## <a name="uninstall"></a>Desinstalar

O MSIX oferece um bom histórico de instalação e desinstalação.  Já que os pacotes MSIX são pacotes em contêiner, a desinstalação do pacote removerá todos os artefatos do aplicativo, incluindo todos os arquivos gravados em %ProgramFiles%WindowsApps, bem como os arquivos de sistema na pasta AppData ou as configurações de registro criadas para o aplicativo.  A desinstalação não removerá os arquivos de usuário criados.
