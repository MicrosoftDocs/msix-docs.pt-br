---
author: dianmsft
title: Pacotes de modificação MSIX no Windows 10 versão 1903
description: Nesta seção, examinaremos os pacotes de modificação no Windows 10 Atualização 1903
ms.author: diahar
ms.date: 01/14/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.openlocfilehash: bca7001b4db8ac2e6ced6763d4eca7bb7c8d294d
ms.sourcegitcommit: 789bef8a4d41acc516b66b5f2675c25dcd7c3bcf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/14/2019
ms.locfileid: "65802372"
---
# <a name="msix-modification-packages-on-windows-10-version-1903"></a>Pacotes de modificação MSIX no Windows 10 versão 1903
 
No Windows 10 versão 1809, introduzimos [pacotes de modificação MSIX](modification-packages.md) que permitem que as empresas personalizem seus aplicativos no Windows 10. Na próxima versão principal do Windows, adicionaremos suporte extra para permitir que os profissionais de TI empacotem personalizações, como plug-ins baseados em arquivo, em um pacote MSIX. 

Os recursos a seguir foram adicionados ao Windows 10, versão 1903.

## <a name="manifest-update"></a>Atualização do manifesto
Adicionamos suporte no elemento a seguir ao manifesto do pacote de modificação MSIX.

```xml
<Properties>
   <rescap6:ModificationPackage>true</rescap6:ModificationPackage>
</Properties>
```

Para garantir que os pacotes de modificação funcionem na versão 1903 ou posterior, o manifesto do pacote de modificação precisarão incluir esse elemento. Isso será feito para você se empacotar o pacote de modificação MSIX usando a versão de janeiro da Ferramenta de Empacotamento MSIX. Se você converter um pacote usando nossa ferramenta anterior à versão, poderá editar o pacote existente na ferramenta para adicionar esse novo elemento. Além disso, se os usuários instalarem o pacote de modificação, eles serão alertados de que o pacote poderá modificar o aplicativo principal.

Se você estiver usando um pacote de modificação criado antes da versão 1903, será necessário editar o manifesto de pacote para atualizar o atributo MaxVersionTested para 10.0.18362.0.

```xml
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="10.0.18362.0" />
```

## <a name="overriding-a-file-in-the-main-package"></a>Como substituir um arquivo no pacote principal
Substitua um arquivo no pacote principal com um pacote de modificação. Isso não significa que você está alterando o arquivo do pacote principal. O arquivo permanece inalterado pelo pacote de modificação. No entanto, em tempo de execução, o pacote principal vê seus arquivos e o arquivo do pacote de modificação e escolherá os arquivos do pacote de modificação a serem carregados. 

> [!NOTE]
> O arquivo que o pacote de modificação pretende substituir precisa estar em uma pasta do VSF (sistema de arquivos virtual). 

## <a name="file-system-based-plug-in"></a>Plug-in baseado no sistema de arquivos
Empacote o plug-in base do sistema de arquivos como um pacote de modificação MSIX. Se os pacotes principais carregarem o plug-in examinando uma pasta, você poderá instalar o pacote principal e o pacote de modificação separadamente. Em tempo de execução, o plug-in será exibido, pois o aplicativo principal poderá consultar suas pastas e as pastas da modificação. 

> [!NOTE]
> A pasta que o aplicativo principal usa para carregar os plug-ins precisa estar em uma pasta do VFS (sistema de arquivos virtual).  

## <a name="what-remains-the-same"></a>O que permanece o mesmo
Os plug-ins do Registro virtual que foram convertidos em pacotes de modificação MSIX ainda terão suporte na próxima versão principal do Windows. 

