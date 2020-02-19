---
title: implantar aplicativos pré-instalados
description: Este artigo apresenta uma visão geral dos aplicativos pré-instalados
ms.date: 07/02/2019
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, msix, uwp, pacotes opcionais, conjunto relacionado, extensão do pacote, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: a743e8e4f960bc50e904b133da07e6fb3feab181
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074232"
---
# <a name="deploying-preinstalled-apps"></a>Implantar aplicativos pré-instalados 
Este artigo apresentará uma visão geral de como os arquivos pré-instalados funcionam e como as licenças e o provisionamento funcionam com os aplicativos pré-instalados. 

## <a name="how-preinstalled-apps-work"></a>Como os aplicativos pré-instalados funcionam 

A instalação do aplicativo pode ser dividida em duas etapas: 
1. Preparando 
2. Registro 

Quando a plataforma prepara um aplicativo, ela coloca todos os arquivos do pacote do aplicativo no sistema de arquivos e define as ACLs apropriadas. Depois da preparação, o aplicativo poderá então ser registrado para o usuário atual. O registro é onde a plataforma faz toda a configuração para disponibilizar o aplicativo para o usuário e integrado ao sistema operacional, como criar locais de AppData específicos do usuário, habilitar a detecção de pontos de extensão do aplicativo como associações de tipo de arquivo e criar blocos de aplicativo. Há muitas outras coisas interessantes que podem acontecer, mas o importante é saber que o registro deve acontecer por usuário, enquanto a preparação do aplicativo precisa ser feita apenas uma vez e pode ser feita sem usuários. A plataforma precisa preparar um aplicativo apenas uma vez para qualquer número de usuários, mas cada usuário precisa estar conectado para registrar o aplicativo.

Para pré-instalar um aplicativo para um usuário, a plataforma ainda precisa fazer essas mesmas duas etapas de preparação e registro. A preparação é fácil. Ela pode ser feita a qualquer momento e até mesmo offline na imagem do Windows antes mesmo da fabricação do PC. Porém, o registro precisa que um usuário esteja conectado e, em um novo PC ou em uma nova conta de usuário, isso não pode acontecer até que o usuário se conecte pela primeira vez. Para concluir a pré-instalação, a plataforma precisa registrar o aplicativo em nome do usuário depois que ele se conectar. A plataforma consegue isso usando um serviço de sistema que sabe tudo sobre os aplicativos pré-instalados que precisam ser registrados e aciona automaticamente o registro quando o usuário se conecta pela primeira vez. A plataforma chama esse serviço de Serviço de Preparação de Aplicativos, ou ARS.

Em outras palavras, os aplicativos pré-instalados funcionam preparando o pacote do aplicativo no disco e configurando-o para o registro automático de cada usuário na próxima vez que o usuário se conectar.

## <a name="provisioning-apps"></a>Provisionamento de aplicativos 
Todo o provisionamento de aplicativos é encapsulado na ferramenta DISM e realiza tanto a preparação quanto a configuração do ARS. Para fazer o provisionamento, o profissional de TI precisa de um pacote do aplicativo (.msix, .msixbundle, .appx ou .appxbundle) e quaisquer pacotes de dependência. 

Lista de comandos relevantes do PowerShell
* **/Get-ProvisionedAppxPackages** Isso listará todos os aplicativos pré-instalados na imagem.
* **/Add-ProvisionedAppxPackage** Isso prepara o pacote appx e o configura para pré-instalação. Todas as dependências também precisam ser fornecidas, as quais podem ser encontradas no SDK ou nos pacotes baixados da Microsoft Store.
* **/Remove-ProvisionedAppxPackage** Isso pode ser usado para remover um aplicativo pré-instalado. Observe que isso não remove o aplicativo caso ele já esteja registrado para algum usuário; apenas remove o comportamento de registro automático para que ele não seja atualizado automaticamente para os novos usuários.  Caso nenhum usuário tenha instalado o aplicativo, esse comando também removerá os arquivos preparados.

## <a name="licensing"></a>Licenciamento
O licenciamento se aplica somente ao provisionamento de um aplicativo da Windows Store. Qualquer outro aplicativo pode ser provisionado sem uma licença. Se um aplicativo for da Store, uma licença de computador também deverá ser fornecida quando o aplicativo for provisionado. Nesse momento, todos os aplicativos pré-instalados da Windows Store precisam ser gratuitos e configurados como pré-instaláveis pelo Partner Center da Windows Store. Depois de configurados, o pacote pré-instalável e a licença podem ser baixados e provisionados em qualquer imagem.

