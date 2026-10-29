# Discovery — Weather App

## Contexto

A empresa solicitou uma aplicação de previsão do tempo para permitir que usuários consultem condições meteorológicas de cidades. O briefing identifica quatro capacidades: buscar cidades, visualizar o clima atual, consultar a previsão de cinco dias e alternar entre Celsius e Fahrenheit. Também exige que a aplicação possa ser usada em dispositivos móveis.

O problema de negócio, os públicos prioritários e os critérios de sucesso ainda não foram detalhados. Portanto, os requisitos abaixo são uma primeira análise e devem ser validados com as partes interessadas antes da especificação do produto.

### Personas preliminares

- **Pessoa que planeja atividades da semana:** quer consultar a previsão para decidir quando realizar atividades ou viajar.
- **Pessoa que toma decisões para o dia:** quer verificar rapidamente as condições atuais e escolher a unidade de temperatura que prefere.

Essas personas são hipóteses baseadas nas funcionalidades descritas e precisam ser confirmadas.

## Requisitos Funcionais

- **RF-01 — Buscar cidades:** permitir que o usuário pesquise uma cidade.
- **RF-02 — Consultar clima atual:** exibir as condições meteorológicas atuais da cidade selecionada.
- **RF-03 — Consultar previsão:** exibir a previsão do tempo para um período de cinco dias.
- **RF-04 — Alternar unidade:** permitir alternar a apresentação da temperatura entre Celsius e Fahrenheit.

Os detalhes de busca, conteúdo meteorológico e período da previsão permanecem em aberto.

## Requisitos Não-Funcionais

Os itens marcados como candidatos são necessidades razoáveis para validação; o briefing não define métricas nem critérios de aceite para eles.

- **RNF-01 — Uso em dispositivos móveis (explícito):** a aplicação deve ser utilizável em dispositivos móveis. Os tamanhos de tela e critérios de validação ainda precisam ser definidos.
- **RNF-02 — Usabilidade (candidato):** busca, condições atuais, previsão e unidade selecionada devem ser compreensíveis para o usuário. Validar com as partes interessadas.
- **RNF-03 — Desempenho (candidato):** definir um tempo aceitável para buscar uma cidade e apresentar os dados meteorológicos; o briefing não estabelece uma meta.
- **RNF-04 — Acessibilidade (candidato):** definir requisitos de navegação por teclado, leitores de tela e contraste; o briefing não os especifica.
- **RNF-05 — Confiabilidade (candidato):** definir o comportamento esperado quando os dados meteorológicos estiverem indisponíveis ou demorarem a responder.

## Riscos

| Risco | Probabilidade | Impacto | Mitigação inicial |
| --- | --- | --- | --- |
| A fonte de dados meteorológicos pode ter indisponibilidade, limites de uso ou restrições de acesso. | Média | Alto | Avaliar provedores, limites, cobertura, custo e necessidade de credenciais antes de definir a integração. |
| A busca pode retornar cidades homônimas ou uma localidade diferente da pretendida. | Média | Alto | Validar como resultados serão diferenciados, por exemplo, com região ou país, e confirmar a seleção antes de exibir dados. |
| O escopo de “previsão de cinco dias” pode ser interpretado de formas diferentes. | Alta | Médio | Definir período, granularidade e fuso horário com as partes interessadas antes de escrever critérios de aceite. |
| A apresentação pode não funcionar bem em diferentes tamanhos de tela. | Média | Alto | Definir dispositivos e larguras de referência e validar os principais fluxos em telas móveis e maiores. |
| Falhas de rede ou dados atrasados podem impedir a consulta. | Média | Alto | Definir mensagens e comportamento de recuperação para falhas, ausência de dados e demora. |
| A alternância entre Celsius e Fahrenheit pode produzir valores inconsistentes ou confusos. | Baixa | Médio | Definir regras de conversão, arredondamento e se a preferência deve persistir entre visitas. |

## Perguntas em Aberto

| Pergunta | Impacto se não for respondida |
| --- | --- |
| Qual fonte de dados deve fornecer o clima e a previsão? Há custos, limites de uso ou necessidade de chave? | Pode alterar custo, arquitetura, cobertura e viabilidade da integração. |
| “Previsão de cinco dias” inclui hoje ou os cinco dias seguintes? | Pode levar a conteúdo e critérios de aceite diferentes. |
| A previsão deve ser diária, por hora ou oferecer ambos? | Define a quantidade e a forma de apresentar os dados. |
| Como diferenciar cidades com o mesmo nome? É necessário mostrar estado, região ou país? | O usuário pode selecionar a localidade errada e receber dados incorretos. |
| Deve haver detecção automática da localização do usuário? | Afeta permissões, privacidade e o fluxo inicial de consulta. |
| Qual deve ser a unidade padrão: Celsius ou Fahrenheit? A preferência precisa ser salva? | Afeta a experiência inicial e a persistência da preferência. |
| Em quais idiomas a interface deve estar disponível? Como datas e horários devem ser localizados? | Afeta textos, formatos regionais e critérios de internacionalização. |
| Qual comportamento é esperado sem conexão ou quando o provedor falha? É necessário cache ou modo offline? | Sem definição, erros podem bloquear o uso ou gerar dados desatualizados sem aviso. |
| Quais dispositivos, navegadores e metas de acessibilidade devem ser suportados? | Impede validar objetivamente a compatibilidade e a experiência móvel. |
| Quais elementos compõem “clima atual” e a previsão: temperatura, sensação térmica, precipitação, vento ou outros? | O conteúdo pode não atender às necessidades dos usuários ou crescer sem controle de escopo. |

## Suposições

### Hipóteses a validar

- A busca será iniciada pelo usuário informando o nome de uma cidade; geolocalização automática não foi solicitada no briefing.
- O usuário poderá consultar dados sem criar uma conta; autenticação não foi mencionada.
- Celsius e Fahrenheit são unidades de apresentação dos mesmos dados meteorológicos; o comportamento de persistência da escolha ainda precisa ser definido.
- As personas preliminares representam necessidades plausíveis, mas não foram confirmadas por pesquisa com usuários.
- Não se assume provedor, idioma, frequência de atualização, armazenamento de dados ou funcionamento offline com base apenas no briefing.

### Decisões de referência do treinamento

Para o exercício, o desafio do Module 02 propõe usar Open-Meteo sem chave de API, considerar “cinco dias” como hoje mais os quatro dias seguintes, usar Celsius como unidade padrão, não incluir autenticação nem persistência em servidor e disponibilizar a interface em pt-BR. Essas são decisões de referência do treinamento; devem ser confirmadas antes de tratá-las como requisitos aprovados do negócio.