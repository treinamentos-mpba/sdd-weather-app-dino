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

- **Dado** que o usuário informou um nome de cidade válido, **quando** enviar a busca, **então** o sistema apresenta os resultados de localidade retornados pelo serviço de busca.
- **Dado** que o usuário enviou uma consulta sem resultados, **quando** a busca terminar, **então** o sistema informa que nenhuma cidade foi encontrada e permite iniciar outra busca.

### RF-02 — Selecionar cidade

- **Dado** que há resultados de busca, **quando** o usuário selecionar um resultado, **então** esse resultado passa a ser a cidade consultada e os dados exibidos correspondem a ela.
- **Dado** que os resultados incluem localidades homônimas, **quando** forem apresentados, **então** cada opção inclui os dados de região ou país disponíveis para distingui-las. A regra exata de desambiguação é uma hipótese pendente.

### RF-03 — Consultar clima atual

- **Dado** que uma cidade foi selecionada e o serviço retornou dados atuais, **quando** a consulta terminar, **então** a tela exibe a temperatura e a descrição da condição meteorológica dessa cidade.
- **Dado** que não há dados atuais disponíveis para a cidade, **quando** a consulta terminar, **então** a tela informa que os dados atuais não estão disponíveis e não inventa valores.

### RF-04 — Consultar previsão de cinco dias

- **Dado** que uma cidade foi selecionada e o serviço retornou previsão, **quando** a consulta terminar, **então** a tela apresenta cinco datas consecutivas, começando por hoje e incluindo os quatro dias seguintes.
- **Dado** que há dados para menos de cinco datas, **quando** a previsão for apresentada, **então** os dias disponíveis são exibidos e a indisponibilidade dos demais é indicada; nenhuma data ou valor ausente é fabricado.

### RF-05 — Alternar unidade de temperatura

- **Dado** que temperaturas estão visíveis em Celsius, **quando** o usuário selecionar Fahrenheit, **então** as temperaturas atuais e da previsão passam a ser exibidas em Fahrenheit, com o rótulo correto e valores equivalentes dentro do arredondamento apresentado.
- **Dado** que temperaturas estão visíveis em Fahrenheit, **quando** o usuário selecionar Celsius, **então** as temperaturas atuais e da previsão passam a ser exibidas em Celsius, com o rótulo correto e valores equivalentes dentro do arredondamento apresentado.
- **Dado** que a unidade foi alternada, **quando** a tela atualizar os valores, **então** a cidade selecionada e as datas da previsão permanecem inalteradas.

### RF-06 — Comunicar estados de consulta

- **Dado** que uma consulta está em andamento, **quando** os dados ainda não chegaram, **então** a interface indica que está carregando.
- **Dado** que o serviço falhou ou excedeu o tempo de resposta, **quando** a consulta terminar sem sucesso, **então** a interface informa que não foi possível obter os dados e oferece uma nova tentativa.
- **Dado** que uma nova cidade está sendo consultada, **quando** a consulta falhar, **então** os dados anteriores não são apresentados como se pertencessem à nova cidade.

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