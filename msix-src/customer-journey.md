---
title: Percursos dos clientes
description: Este artigo descreve a jornada do cliente MSIX.
author: dianmsft
ms.date: 05/12/2020
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: aa87e92294a970f82f1653cb1510c7eddf371dc2
ms.sourcegitcommit: 8b02a0d376ab16873607ed84d1560b1c69c9e915
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83632618"
---
# <a name="customer-journeys-with-msix"></a>Viagens de clientes com MSIX

Muitos clientes estão usando o MSIX com êxito para melhorar a implantação de seus aplicativos do Windows. Este artigo destaca várias viagens de clientes com o MSIX.

* [Saiba como o DB SYSTEL está habilitando a implantação ágil de aplicativos do Windows](customer-journey.md#db-systel)
* [Saiba como a rede de computação educacional de Ontário (ECNO) está melhorando a entrega de aplicativos de desktop e a experiência de instalação para alunos do Ontário](customer-journey.md#ecno)
* [Saiba como o Schneider Electric está desenvolvendo e Implantando aplicativos de área de trabalho do WPF](customer-journey.md#schneider-electric)

## <a name="db-systel"></a>SYSTEL DB

![Logotipo do BD SYSTEL](images/DB_logo_red_outlined_200px_rgb.png)

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

## <a name="ecno"></a>ECNO

![Logotipo do ECNO](images/ECNO_masterlogo.png)

A rede de computação educacional do Ontário (ECNO) é uma organização que localiza e executa soluções de ti efetivas para os 72 (2 milhões alunos) de Ontário de A ECNO é tarefa de acompanhar as tecnologias de computação mais recentes, pois elas se relacionam com as metas de educação da província de Ontário. Eles fazem recomendações de infraestrutura de ti e compartilham práticas recomendadas com as placas escolares membros.

Os quadros escolares são autônomos e são gratuitos para implementar suas próprias estratégias de ti para o que funciona melhor para eles. Por exemplo, um quadro da escola usa Configuration Manager exclusivamente para distribuir aplicativos, enquanto outro o usa apenas para geração de imagens de máquina e um pequeno percentual de aplicativos.

Sob a grande parte do ECNO, a equipe de projeto dos serviços de tecnologia compartilhada avalia, testa e empacota um catálogo de aplicativos de 450 aplicativos de terceiros para o espectro de necessidades de K – 12 alunos, bem como da equipe.

Até o momento, a equipe do ECNO STS converteu a maioria de seus aplicativos por meio da ferramenta de empacotamento MSIX e da ferramenta de conversão em massa incluída na caixa de ferramentas do MSIX.

Atualmente, o ECNO tem cerca de 170 aplicativos no Partner Center, com dez placas de estudante vinculadas à sua conta LOB. Atualmente, dois estão permitindo o acesso em uma base de "autoatendimento" para nossos aplicativos em seu armazenamento. O terceiro já tem Configuration Manager vinculado à sua loja e está procurando adicionar acesso aos nossos aplicativos dessa maneira. Mais seis estão procurando migrar para gerenciamento/atribuição de software por meio do Intune e solicitou que alguns aplicativos de exemplo funcionem. E outro conselho escolar está concluindo um projeto para mover seu iPads para o Intune como uma ferramenta de MDM e, em seguida, moverá os dispositivos Windows para o Intune (no qual eles estão olhando para usar o MSIX e a loja como seu modelo de distribuição de software). Eles esperam que mais placas comecem a mudar à medida que se movem para adotar o Intune/Configuration Manager.

*"É um envio por push na direção certa para que possamos entregar software aos nossos usuários que não podem estar em uma rede de estudante neste momento durante o COVID-19. Os pacotes do AppV foram um benefício para nós e essa próxima iteração usando o MSIX nos permitirá atender à nossa equipe e aos alunos com o software que eles precisam usando um sistema de entrega que seja moderno e contínuo de qualquer conexão com a Internet no mundo. "*
-Jason David Flannery, engenheiro de sistemas, conselho escolar do distrito do Simcoe County

É fácil configurar e facilitar a implantação depois de registrarmos dispositivos no Windows Intune usando o AutoPilot.

Quais benefícios o ECNO está obtendo aproveitando o MSIX?

* Como líder em implantação de software para os quadros escolares em Ontário, o MSIX forneceu uma transição direta dos nossos processos do App-V.
* Permite que nossos desktops escolas se mantenham atuais nas ferramentas modernas de implantação de software e no gerenciamento de ponto de extremidade.
* Para nossa equipe de projeto, ela aumentou a produtividade, forneceu uma implantação mais fácil e suporte para nossos quadros escolares.

*"À medida que nossa equipe de ti se esforça para se tornar mais ágil, especialmente agora com o aprendizado de distância sobre nós, estamos contando com a nuvem cada vez mais.  A próxima evolução lógica para nossa organização é o gerenciamento de nuvem. Queremos que nossa equipe de ti baseada na escola se concentre mais na implementação bem-sucedida da tecnologia, menos na implantação e solução de problemas. Como trabalhamos para a implantação Zero Touch de nossos dispositivos e aplicativos, vamos trabalhar junto com a equipe do STS do ECNO para integrar os aplicativos que todos nós usamos na sala de aula em nossa abordagem gerenciada na nuvem. "*
-Sean Heuchert, gerente de informações, Peterborough Victoria Northumberland Clarington Catholic distrito escolar

## <a name="schneider-electric"></a>Schneider elétrico

![Schneider elétrico](images/Logo_SE_Green_RGB-Screen.png)

O Schneider Electric desenvolve o software GIS para dar suporte a utilitários pequenos e grandes (elétricos, gás, água, fibra, insistir) nos EUA e internacionalmente.

Ao trabalhar com a Microsoft, queríamos trazer nossos aplicativos do WPF para a tecnologia de instalação mais recente, MSIX. Temos um caso de uso específico baseado em cliente; Precisamos lançar e implantar com frequência como uma empresa de software de opinião de desenvolvimento, mas cada empresa de utilitário deseja gerenciar as alterações e determinar individualmente qual versão do aplicativo será liberada para seus usuários e quando. Estamos trabalhando junto com a Microsoft para atender a esse requisito de cliente. Depois de concluirmos nossa atualização para MSIX, poderemos remover algumas dependências de terceiros e ser um passo mais próximo de atualizar nossos aplicativos para o .NET Core 3,0.

Aproveitando o MSIX, estamos simplificando nossa pilha de desenvolvimento. Usaremos as tecnologias da Microsoft do código (C#/WPF) para compilar (Azure dev ops) para o instalador (MSIX). Um dos principais benefícios do MSIX é que os desenvolvedores podem facilmente desenvolver, depurar e testar o instalador diretamente do Visual Studio. Isso simplifica muito a instalação de uma solução de problemas específica e nos permitiu trabalhar em conjunto com a Microsoft.

*"O suporte da Microsoft à medida que mudamos para MSIX é inestimável. Temos um caso de uso exclusivo, e a Microsoft nos ajudou a navegar pelos obstáculos encontrados durante a atualização de nossos aplicativos. Os desenvolvedores estão empolgados em migrar nossos aplicativos para MSIX. "* – Darlene Rouleau da Schneider Electric
