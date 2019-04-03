---
author: dianmsft
title: Pacotes de MSIX modificação no Windows 10 Build 18312
description: Nesta seção, analisaremos os pacotes de modificação no Windows 10 1903 Update
ms.author: diahar
ms.date: 01/14/2019
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.openlocfilehash: 5404b01de2841206669d73dc6f2fb14cabe93ef9
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900528"
---
# <a name="msix-modification-packages-on-windows-10-insider-preview-build-18312-and-later"></a>Pacotes de modificação de MSIX no 18312 de Build do Windows 10 Insider Preview e versões posteriores 
No Windows 10 versão 1809, apresentamos [pacotes de modificação de MSIX](modification-packages.md) que permitem que as empresas personalizar seus aplicativos no Windows 10. Na próxima versão principal do Windows, estamos adicionando suporte adicional para permitir que os profissionais de TI personalizações de pacote como plug-ins baseados em arquivo em um pacote MSIX. 

Os seguintes recursos estão sendo adicionados para a próxima versão principal do Windows e estão disponíveis a partir do Windows 10 Insider Preview compilar 18312.

## <a name="manifest-update"></a>Atualização do manifesto
Adicionamos suporte para o seguinte elemento ao manifesto do pacote de modificação de MSIX.

```xml
<Properties>
   <rescap6:ModificationPackage>true</rescap6:ModificationPackage>
</Properties>
```

Para garantir que os pacotes de modificação funcionam no build 18312 ou posterior, o manifesto do pacote de modificação deve incluir esse elemento. Isso será feito para você, se você empacotar seu pacote de modificação de MSIX usando a versão da ferramenta de empacotamento MSIX de janeiro. Se você converter um pacote usando nossa ferramenta antes do lançamento, você pode editar um pacote existente na nossa ferramenta para adicionar esse novo elemento. Além disso, se os usuários instalarem o pacote de modificação, eles serão alertados que o pacote pode modificar o aplicativo principal.

Se você estiver usando um pacote de modificação que foi criado antes da compilação 18132, é necessário editar o manifesto de pacote para atualizar o atributo MaxVersionTested para 10.0.18132.0.

```xml
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.17701.0" MaxVersionTested="10.0.18132.0" />
```

## <a name="overriding-a-file-in-the-main-package"></a>Substituindo um arquivo no pacote principal
Você pode substituir um arquivo no pacote principal com um pacote de modificação. Isso não significa que você está alterando o arquivo do pacote principal. O arquivo permanece inalterado pelo pacote de modificação. No entanto, em tempo de execução do pacote principal vê seus arquivos e o arquivo do pacote de modificação, e ele escolherá a modificação de arquivos do pacote para carregar. 

> [!NOTE]
> O arquivo que pretenda substituir o pacote de modificação deve ser em uma pasta (VSF) do sistema de arquivos virtual. 

## <a name="file-system-based-plug-in"></a>Sistema de arquivos com base em plug-in
Você pode empacotar sua base de sistema de arquivo plug-in como um pacote de modificação de MSIX. Se seus pacotes principais carregar seus plug-in, observando uma pasta, você pode instalar o pacote principal e o pacote de modificação separadamente. Em tempo de execução, o plug-in aparecerá, pois o aplicativo principal pode consultar suas pastas e pastas da modificação. 

> [!NOTE]
> A pasta que o aplicativo principal usa para carregar o plug-ins deve estar em uma pasta (VFS) do sistema de arquivos virtual.  

## <a name="what-remains-the-same"></a>O que permanece o mesmo
Registro virtual plug-ins que foram convertidos em pacotes de modificação de MSIX ainda terá suporte na próxima versão principal do Windows. 

