__autor__: Henrique Pougy - Supervisor Geral, STEL/CASE

# Previsao de Impacto - Deduplicação SLCe

## Contexto:

O _Sistema Eletrônico de Licenciamento de Construções_ (__SLCe__) "é uma ferramenta que permite que o cidadão licencie diversos tipos de obras, de pequeno e médio porte, totalmente de forma eletrônica" (http://bit.ly/slce-descricao). Trata-se do sistema de licenciamento edilício eletrônico da Prefeitura de São Paulo que tem por escopo, grosso modo, os projetos de menor porte, cuja competência de análise é delegada, pela Secretaria Municipal de Licenciamento, aos arquitetos e engenheiros lotados nas Subprefeituras do Munícipio de São Paulo.

Implementado ao final de 2012, ele se utiliza de duas ferramentas proprietárias da Microsoft: 

* o Dynamics CRM versão 2011, responsável pela aplicação propriamente dita; e 

* o SharePoint 2010 que, integrado ao Dynamics, é responsável pela persistência de dados. 

Ambas as ferramentas, que não foram originalmente concebidas para modelar o domínio do licenciamento edilício, foram extensamente customizadas para atender às necessidades de modelagem das atividades de licenciamento edilício da Prefeitura no sistema. Tais customizações, ainda que inicialmente funcionais, têm hoje demonstrado sérios problemas de escalabilidade.

## O Problema:

Uma dessas customizações diz respeito à uma funcionalidade de versionamento dos arquivos das solicitações realizadas no sistema. Basicamente, toda as vezes em que são realizados dois tipos de ação no sistema pelos usuários da Prefeitura - _"Análise da Solicitação"_ e _"Análise de Requisitos Básicos"_ - são criados novos lotes de arquivos no SharePoint relacionados a essas solicitações. O objetivo destes lotes de arquivos é registrar, como um _snapshot_, a situação exata da documentação relacionada ao processo quando da realização destas ações.

No entanto, tais _snapshots_ não são realizados por meio do registro de _hyperlinks_ apontando aos arquivos originais, quando não há modificação nos mesmos. Com efeito eles __geram a duplicação de todos os arquivos que compõem a documentação da solicitação a cada novo lote, replicando todo o seu conteúdo__. Neste cenário, uma solicitação que foi analisada 6 vezes, terá todos os seus arquivos originais (plantas em .dwf, digitalizações dos documentos do requerente etc.) replicados por 5 vezes no sistema.

São evidentes as limitações de escalabilidade que essa solução possui. Com efeito, ao longo dos anos, a volumetria ocupada pelo SharePoint cresceu vertiginosamente, __ultrapassando de forma preocupante o valor máximo recomendado pela Microsoft__ (http://bit.ly/limites_Sharepoint), alcançando mais de 4 TeraBytes de dados.

## Objetivos deste _report_

Este _report_ tem por objetivo principal realizar uma __previsão de impacto, baseada em evidências, de uma ação de deduplicação dos arquivos do SLCe__. Ele busca avaliar e sensibilizar os tomadores de decisão sobre os possíveis benefícios de tal ação - que, como veremos ao final, são elevados.

O _report_ se vale de uma base de dados extraída em 2018, em formato .xlsx, que tinha por objetivo original gerar um relatório gerencial de produtividade dos servidores da PMSP no sistema. Para isso, o autor do relatório gerencial construiu indicadores relacionados à quantidade de ações realizadas por usuário em um determinado período de tempo, a duração dessas ações, entre outras informações. Para construção dos gráficos e indicadores ali presentes, foram utilizados os dados relacionados à todas as ações realizadas no sistema por usuários da Prefeitura à época, extraídos do banco de dados do sistema em 2018.

Por sorte - e confirmando aqui o valor dessa boa prática - os microdados que fundamentaram o report foram adicionados ao arquivo. É com base neles que desenvolvemos estes report. Estes mesmos microdados são disponibilizados, após remoção de dados pessoais e padronização do formato, no arquivo "dados_limpos.xlsx" que consta neste repositório.

## Metodologia

Os microdados das ações realizadas no sistema pelos usuários da PMSP no período compreendido entre outubro de 2012 e agosto de 2018 são aqui analisados para identificar e quantificar com precisão __todas as ações realizadas no sistema neste período que geraram duplicação de arquivos__. Isto requer a realização de diversas filtragens nos dados originais, para garantir que se tratam de fato de:

* Ações que foram realizadas sobre processos digitais (e não processos em papel);
* Que foram feitas sobre processos que foram protocolados com sucesso (e não em rascunho, cancelados etc.);
* Que estão dentre as ações que de fato geram duplicação (conforme mencionado acima); e,
* Por fim, que foram ações de fato concluídas, e não apenas iniciadas ou canceladas;

Uma vez realizada a filtragem no _dataset_ original, realizamos em seguida uma análise estatística descritiva simples. Esta análise permite entender melhor como se dá a distribuição das duplicações de dados nos protocolos do sistema. Os principais resultados desta análise são a quantidade média de ações que geram duplicação por solicitação no sistema, além da proporção da quantidade de arquivos originais em relação aos duplicados.

Em seguida, realizamos uma projeção da quantidade de ações que geram duplicação de arquivos no sistema em função do tempo. Esta projeção, obtida através da modelagem estatística dos dados (Regressão Linear Simples), que obteve alto coeficiente de determinação e significância estatística, permite estimar a quantidade de lotes de arquivos a serem afetados por uma desduplicação a ser realizada hoje.


## Resultados encontrados:

__Mediana de ações que geram duplicação por protocolo__ : 2 por protocolo

__Proporção do banco de dados desduplicado__ : aprox. 40% da volumetria de armazenagem de arquivos em 2018

__Estimativa de lotes a serem afetados pela desduplicação__ : aprox. 100 mil lotes

Conforme dados apresentandos acima, este _report_ conclui que a ação de deduplicação de arquivos no sistema SLCe será altamente benéfica. Ela é, portanto, __altamente recomendada__.
