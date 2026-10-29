# Weather App — Especificação de Produto

**Status:** Rascunho para validação. As decisões registradas no discovery são consideradas definidas para o escopo do treinamento; hipóteses adicionais estão identificadas abaixo.

## Overview

O Weather App é uma aplicação web em pt-BR para consultar o clima atual e a previsão de cinco dias de uma cidade. O usuário pesquisa e seleciona uma cidade, consulta as condições e alterna a temperatura entre Celsius e Fahrenheit. A experiência deve ser utilizável em dispositivos móveis.

**Decisões de escopo:** Open-Meteo como fonte de dados, sem chave de API; período da previsão de hoje mais os quatro dias seguintes; Celsius como unidade inicial; sem autenticação ou persistência de dados no servidor; interface em pt-BR.

**Públicos preliminares:** pessoas que planejam atividades da semana e pessoas que consultam o clima antes de sair. Essas personas ainda precisam ser validadas. Não há métrica de sucesso de negócio aprovada.

## Functional Requirements

- **RF-01 — Buscar cidades:** permitir que o usuário pesquise uma cidade pelo nome e veja os resultados correspondentes.
- **RF-02 — Selecionar cidade:** permitir que o usuário escolha uma localidade nos resultados para consultar seus dados meteorológicos.
- **RF-03 — Consultar clima atual:** exibir as condições atuais da cidade selecionada. Para esta versão, assume-se que o mínimo exibido é temperatura e uma descrição da condição meteorológica.
- **RF-04 — Consultar previsão de cinco dias:** exibir a previsão para hoje e os quatro dias seguintes. Assume-se uma visão resumida diária, pendente de validação.
- **RF-05 — Alternar unidade de temperatura:** permitir alternar entre Celsius e Fahrenheit para os valores de temperatura do clima atual e da previsão.
- **RF-06 — Comunicar estados de consulta:** informar carregamento, ausência de resultados e falhas ao buscar dados, sem apresentar dados de outra cidade como se fossem os selecionados.

## User Stories

- **US-01 — Busca de cidade:** Como pessoa que toma decisões para o dia, quero buscar uma cidade pelo nome para consultar o clima da localidade desejada. **Requisito: RF-01.**
- **US-02 — Seleção da localidade:** Como pessoa que planeja atividades da semana, quero distinguir resultados de cidades com o mesmo nome para consultar a previsão do destino correto. **Requisito: RF-02.**
- **US-03 — Clima atual:** Como pessoa que toma decisões para o dia, quero ver as condições atuais da cidade selecionada para me preparar antes de sair. **Requisito: RF-03.**
- **US-04 — Planejamento dos próximos dias:** Como pessoa que planeja atividades da semana, quero consultar a previsão de hoje e dos quatro dias seguintes para escolher quando realizar atividades ou viajar. **Requisito: RF-04.**
- **US-05 — Unidade de temperatura:** Como pessoa que toma decisões para o dia, quero alternar entre Celsius e Fahrenheit para interpretar as temperaturas na unidade que prefiro. **Requisito: RF-05.**
- **US-06 — Recuperação de consulta:** Como pessoa que planeja atividades da semana, quero receber uma mensagem clara quando uma consulta falhar e poder tentar novamente para continuar meu planejamento. **Requisito: RF-06.**

## Acceptance Criteria

### RF-01 — Buscar cidades

- **Given** o campo de busca contém `Lisboa` e o serviço retorna duas localidades, **When** o usuário envia a busca, **Then** as duas localidades retornadas são exibidas como opções selecionáveis.
- **Given** o campo de busca contém somente espaços, **When** o usuário envia a busca, **Then** nenhuma requisição de busca é enviada e a interface solicita um nome de cidade.
- **Given** o serviço retorna uma lista vazia, **When** a busca termina, **Then** a interface informa que nenhuma cidade foi encontrada e mantém disponível o campo para uma nova busca.

### RF-02 — Selecionar cidade

- **Given** a busca exibe as localidades `Springfield, Illinois` e `Springfield, Missouri`, **When** o usuário seleciona `Springfield, Missouri`, **Then** essa localidade aparece como selecionada e as consultas meteorológicas usam a localização retornada para essa opção.
- **Given** um resultado contém região ou país nos dados retornados, **When** as opções de busca são exibidas, **Then** esses dados aparecem junto ao nome da cidade.

### RF-03 — Consultar clima atual

- **Given** uma cidade selecionada e uma resposta atual contendo temperatura `18.4 °C` e descrição `Parcialmente nublado`, **When** a consulta termina, **Then** a tela exibe `18.4 °C` e `Parcialmente nublado` para essa cidade.
- **Given** a resposta não contém dados atuais, **When** a consulta termina, **Then** a tela informa que os dados atuais não estão disponíveis e não exibe uma temperatura substituta nem dados de outra cidade.

### RF-04 — Consultar previsão de cinco dias

- **Given** uma cidade selecionada e uma resposta diária com as datas `D`, `D+1`, `D+2`, `D+3` e `D+4`, **When** a consulta termina, **Then** as cinco datas são exibidas uma vez cada e em ordem cronológica, incluindo `D` como hoje.
- **Given** a resposta contém dados para menos de cinco datas, **When** a previsão é exibida, **Then** somente as datas e os valores recebidos são mostrados e a ausência dos demais dias é indicada.

### RF-05 — Alternar unidade de temperatura

- **Given** o clima atual e a previsão exibem temperaturas em Celsius, **When** o usuário seleciona Fahrenheit, **Then** todos os valores de temperatura atuais e previstos passam a usar o rótulo `°F`, e os valores `0 °C` e `100 °C` são exibidos como `32 °F` e `212 °F`.
- **Given** o clima atual e a previsão exibem temperaturas em Fahrenheit, **When** o usuário seleciona Celsius, **Then** todos os valores de temperatura atuais e previstos passam a usar o rótulo `°C`, e os valores `32 °F` e `212 °F` são exibidos como `0 °C` e `100 °C`.
- **Given** há uma cidade e cinco datas de previsão exibidas, **When** o usuário alterna a unidade, **Then** a cidade e as cinco datas permanecem iguais antes e depois da alternância.

### RF-06 — Comunicar estados de consulta

- **Given** uma consulta foi enviada e a resposta ainda está pendente, **When** a interface aguarda a resposta, **Then** um indicador de carregamento é visível.
- **Given** o serviço retorna uma falha ou timeout, **When** a consulta termina, **Then** a interface exibe uma mensagem de falha e um controle para tentar novamente.
- **Given** a consulta de uma cidade nova falha após outra cidade ter sido exibida, **When** o estado de erro aparece, **Then** os dados da cidade anterior não são apresentados como dados da cidade nova.
- **Given** uma consulta falhou e o controle de nova tentativa está visível, **When** o usuário o aciona, **Then** uma nova consulta é enviada para a cidade selecionada.

## Non-Functional Requirements

- **RNF-01 — Responsividade:** os fluxos de busca, seleção, consulta e troca de unidade devem ser utilizáveis em dispositivos móveis. Larguras e navegadores de referência ainda precisam ser aprovados.
- **RNF-02 — Localização:** textos da interface devem estar em pt-BR. Datas, horários e números devem seguir formatos compatíveis com pt-BR; fuso horário da exibição permanece pendente.
- **RNF-03 — Desempenho (meta proposta):** em conexão estável, a busca e a apresentação dos dados devem concluir em até 3 segundos no percentil 95. Ambiente de medição e dependência da latência do provedor devem ser definidos antes de aprovar a meta.
- **RNF-04 — Acessibilidade (meta proposta):** buscar conformidade com WCAG 2.2 nível AA, incluindo navegação por teclado, nomes acessíveis para controles e suporte a leitores de tela. A meta requer aprovação.
- **RNF-05 — Resiliência:** falhas, ausência de dados e demora do serviço externo devem resultar em estados compreensíveis, sem exibir valores inexistentes como válidos.
- **RNF-06 — Privacidade e escopo de dados:** não exigir conta e não persistir dados de usuário em servidor. A persistência local de cidade ou unidade não está definida e não deve ser presumida.

## Edge Cases

| Caso | Comportamento esperado |
| --- | --- |
| Consulta vazia ou composta somente por espaços | Não enviar a busca; informar que é necessário inserir uma cidade. |
| Cidade inexistente ou sem correspondência | Informar que nenhum resultado foi encontrado e permitir nova consulta. |
| Cidades homônimas | Mostrar os detalhes de região ou país disponíveis para apoiar a seleção; se ainda forem indistinguíveis, pedir esclarecimento antes de consultar. |
| Nome com acentos, caracteres especiais ou grafia alternativa | Enviar a consulta sem corromper o texto; apresentar resultados do serviço sem erro de interface. O suporte a aliases permanece pendente. |
| Serviço de busca ou previsão indisponível | Exibir mensagem de falha e opção para tentar novamente; não substituir a falha por valores fictícios. |
| Tempo limite excedido | Encerrar o estado de carregamento, informar a falha e permitir nova tentativa. |
| Resposta parcial, sem clima atual ou sem previsão para todos os dias | Exibir somente os dados válidos e indicar quais informações não estão disponíveis. |
| Usuário troca de cidade enquanto uma consulta anterior ainda está em andamento | Associar a resposta à cidade correta e não sobrescrever a cidade mais recentemente selecionada com dados antigos. |
| Alternância repetida entre Celsius e Fahrenheit | Manter cidade e datas; exibir os valores convertidos com a unidade correspondente, sem acumular erro de conversão por alternância. |
| Atualização sem conectividade | Informar que os dados não puderam ser atualizados. Não há requisito de modo offline ou cache nesta versão. |

## Assumptions

- Os requisitos abaixo usam as decisões registradas em `specs/discovery.md` como decisões do escopo do treinamento: Open-Meteo, hoje mais quatro dias, Celsius inicial, pt-BR, sem autenticação e sem persistência no servidor.
- A busca é iniciada pelo usuário digitando o nome da cidade; geolocalização automática não faz parte do fluxo definido.
- A previsão é diária, e não horária, para esta proposta. Essa interpretação não está confirmada no discovery.
- O mínimo de dados meteorológicos é temperatura e descrição da condição; outros campos, como precipitação, vento e sensação térmica, não estão aprovados.
- Trocar a unidade atualiza os valores em tela sem alterar a cidade ou os dados meteorológicos de origem. Persistir essa preferência entre visitas não está definido.
- Quando possível, a localidade é exibida com região e país para distinguir homônimos.
- Os limites propostos de desempenho e acessibilidade são recomendações de trabalho, não metas aprovadas por stakeholders.
- As personas são hipóteses e não há métricas de sucesso de negócio validadas.

## Risks

| Risco | Probabilidade | Impacto | Mitigação |
| --- | --- | --- | --- |
| Limites, indisponibilidade ou mudança nos serviços Open-Meteo. | Média | Alto | Confirmar termos, cobertura e limites de uso; tratar falhas e indisponibilidade sem exibir dados incorretos. |
| Resultado de geocodificação seleciona uma cidade homônima incorreta. | Média | Alto | Mostrar contexto geográfico disponível e validar a regra de desambiguação com usuários. |
| Definição diária e fuso horário não representam a expectativa dos usuários. | Média | Alto | Validar granularidade e fuso antes de congelar critérios de aceite e apresentação de datas. |
| Requisitos de desempenho ou responsividade não são aprovados ou não são mensuráveis. | Média | Médio | Definir ambiente, navegadores, larguras e metas com stakeholders antes da implementação. |
| Conversões ou arredondamentos produzem temperaturas inconsistentes. | Baixa | Médio | Validar conversões nas duas direções, usar a unidade de origem e testar valores de fronteira. |

## Out of Scope

- Autenticação, contas de usuário e sincronização de dados em servidor.
- Geolocalização automática.
- Favoritos, histórico de buscas ou persistência local de preferências.
- Alertas meteorológicos e notificações.
- Modo offline ou garantia de dados em cache.
- Previsão horária; a proposta atual considera somente resumo diário, pendente de confirmação.
- Idiomas diferentes de pt-BR.

## Open Questions

As decisões abaixo são necessárias para tornar a especificação pronta para implementação. Até serem respondidas, os itens relacionados permanecem como hipóteses.

- A previsão deve ser diária ou horária? Como tratar o fuso horário da cidade selecionada ao definir as cinco datas?
- Quais campos, além de temperatura e descrição, são obrigatórios no clima atual e em cada dia da previsão?
- Como a busca deve tratar cidades homônimas, aliases e resultados sem região ou país disponíveis?
- Qual frequência de atualização é necessária e quando os dados devem ser considerados desatualizados?
- Deve haver cache, funcionamento offline ou apenas uma mensagem quando não houver conexão?
- A escolha Celsius/Fahrenheit ou a última cidade devem persistir localmente entre visitas?
- Quais navegadores, tamanhos de tela e dispositivos devem ser suportados para validar responsividade?
- Stakeholders aprovam WCAG 2.2 AA e a meta proposta de desempenho de até 3 segundos no percentil 95? Em qual ambiente de medição?
- Quais personas são prioritárias e quais métricas demonstrarão que o produto atende às suas necessidades?
- Quais são os limites de uso e a cobertura geográfica do serviço selecionado para os mercados pretendidos?