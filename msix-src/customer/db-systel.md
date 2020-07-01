---
title: DB-SYSTEL
description: O DB SYSTEL descreve sua experiência com o MSIX
author: dianmsft
ms.date: 06/25/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: eb8a99bda70ddf884d7dca1150631334390de61f
ms.sourcegitcommit: fe85d57b3c9478de5fd5347010d2dee1e73f2268
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85549079"
---
# <a name="db-systel"></a>SYSTEL DB

![Logotipo do BD SYSTEL](../images/DB_logo_red_outlined_200px_rgb.png)

O DB SYSTEL GmbH, com sede em Frankfurt am Main, é uma subsidiária totalmente proprietária do BD AG e um parceiro digital para todas as empresas de grupo. O alemão Bahn AG é a segunda maior empresa de transporte do mundo e é o maior operador de ferrovia e proprietário da infraestrutura na Europa. Ele opera grandes partes do ferrovia alemão e carrega cerca de 2.000.000.000 passageiros por ano.

Os funcionários de SYSTEL do BD em torno de 4.600 pessoas, operam com 600 aplicativos de linha de negócios, estações de trabalho de PC 100.000, 93.000 PBXs de VoIP e 200.000 dispositivos móveis, etc. Eles lidam com toda a infraestrutura de ti da empresa, desde os tradicionais serviços de ti até o desenvolvimento de todos os aplicativos internos usados para controlar todos os aspectos do sistema ferrovia. 

Para o DB SYSTEL, os aplicativos de área de trabalho são um componente essencial da infra-estrutura. Eles são a interface principal para muitas tarefas críticas, desde o gerenciamento de funcionários e a garantia do funcionamento correto do sistema ferrovia. BD SYSTEL desenvolver, manter e implantar um total de 600 aplicativos de área de trabalho de cliente FAT e em cerca de 200 aplicativos Java.

Quando se trata de aplicativos de desktop, eles estavam enfrentando alguns desafios, principalmente em relação aos seguintes tópicos:

* Muitos de seus aplicativos do lado do servidor são criados, testados e fornecidos por meio de pipelines de compilação usando processos altamente automatizados – várias vezes por dia (DevOps). No entanto, as tecnologias de implantação atuais tornaram-se impossíveis, até agora, para atingir o mesmo objetivo com os aplicativos da área de trabalho do Windows.
* Muitas equipes estão envolvidas no processo de desenvolvimento e implantação que atrasou em vários dias antes que os usuários pudessem obter as versões mais recentes do software.
* O antigo processo de implantação de software era muito demorado, longo e caro.
* Muitos de seus aplicativos de negócios são baseados na tecnologia Java Web Start, que foi descontinuada.

Como resultado desses desafios, o DB SYSTEL foi capaz de fornecer atualizações de curto prazo com muito esforço. Isso se tornou um problema crítico porque muitos de seus aplicativos dependem de uma versão de software específica no back-end. É essencial que o software cliente do usuário seja atualizado diretamente após a atualização de software no back-end. Se esse não for o caso, a capacidade do usuário de trabalhar com o software em questão não será mais garantida e poderá levar a interrupções nos serviços de trilhos.

O DB SYSTEL primeiro ouviu falar sobre MSIX quando começou a investigar como substituir a tecnologia de início da Web do Java. A MSIX foi promissora porque permitiria que eles criassem aplicativos independentes que não dependem do Java Runtime Environment que está sendo instalado. Isso economizaria a coordenação demorada das equipes e os esforços de sincronização e levaria a uma operação mais estável. Quando começaram a experimentar o MSIX, eles entenderam rapidamente que era a tecnologia certa, não apenas para dar suporte à migração de início da Web Java, mas também para resolver seus principais pontos problemáticos em relação ao empacotamento e à distribuição.

MSIX SYSTEL DB habilitado para:

* Simplifique o empacotamento e a implantação tradicionais de pacotes de software.
* Permita que os desenvolvedores de software tenham um processo completo de ponta a ponta de criação e implantação de software, em vez de delegar os processos de empacotamento e distribuição a equipes especiais.
* Automatize os processos manuais existentes graças aos pipelines.
* Habilite a velocidade e a simplicidade na implantação de aplicativos da área de trabalho do Windows, o que levará a uma economia de custo significativa por meio da nova abordagem de autoatendimento.

*"No passado, temos várias equipes envolvidas no processo e levamos tempo antes de chegar ao ponto em que nossos gerenciadores de aplicativos podiam usar e atualizar nosso software. Consequentemente, pudemos apenas distribuir versões (atualizações) aos nossos clientes com muito esforço. Seguindo um workshop de MSIX muito informativo e proveitosa junto com especialistas da Microsoft, temos certeza de que podemos revolucionar o processo de provisionamento de software no DB SYSTEL usando o autoatendimento do MSIX. O MSIX oferece grandes vantagens como um formato de contêiner em termos de velocidade e simplicidade. Os próprios gerenciadores de aplicativos podem empacotar software usando o MSIX e fornecer seus softwares por meio de nossa loja. "*
-Markus Thomann, consultora de software da equipe de implantação moderna no BD

O sistema de BD está integrando o MSIX ao processo de compilação como um formato de contêiner. A maioria de seus aplicativos, incluindo muitos aplicativos críticos, será transportada para o formato MSIX. Isso tornará o processo de provisionamento de software mais simples, rápido e mais barato. Graças ao MSIX e à equipe de implantação moderna, os gerenciadores de aplicativos agora podem fornecer atualizações de software aos usuários finais diretamente e muitas vezes por dia.

*"A tecnologia MSIX nos permite adotar a abordagem DevOps, embora forneçamos software cliente em vez de software em nuvem. Isso era inconcebível até muito recentemente. "* -Markus Thomann, consultora de software da equipe de implantação moderna no BD
