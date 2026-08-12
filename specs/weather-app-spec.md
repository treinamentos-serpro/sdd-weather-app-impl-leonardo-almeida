# Weather App Specification

## Overview

A Weather App é uma aplicação web responsiva, com foco principal em uso mobile, para consulta de clima de cidades em tempo real. A solução deve permitir buscar uma cidade, visualizar o clima atual e a previsão de 5 dias, além de alternar entre Celsius e Fahrenheit.

A aplicação deve funcionar sem autenticação, sem cadastro e sem persistência no servidor. A interface deve ser em português do Brasil e priorizar velocidade, clareza visual e legibilidade em telas pequenas.

### Decisões de produto
- Fonte de dados: Open-Meteo, sem API key.
- Janela de previsão: hoje + 4 dias.
- Unidade padrão: Celsius.
- Idioma da interface: pt-BR.
- Sem autenticação e sem persistência de servidor.

## Functional Requirements

### FR1. Busca por cidade
O sistema deve permitir ao usuário buscar uma cidade pelo nome e obter o clima correspondente.

Given/When/Then:
- Given que o usuário acessa a tela inicial, When ele digita um nome de cidade e confirma a busca, Then o sistema deve validar a entrada e iniciar a consulta.
- Given que a busca retorna um único resultado válido, When a resposta for recebida, Then o sistema deve carregar o clima atual e a previsão da cidade correspondente.
- Given que a busca retorna múltiplos resultados, When a resposta for recebida, Then o sistema deve exibir uma lista de opções para o usuário selecionar a cidade correta.
- Given que a busca não retorna nenhum resultado, When a resposta for recebida, Then o sistema deve mostrar uma mensagem de “cidade não encontrada” e manter o campo de busca disponível para nova tentativa.

### FR2. Seleção de cidade em resultado múltiplo
Quando houver múltiplas cidades relacionadas ao termo buscado, o sistema deve permitir que o usuário selecione a cidade correta.

Given/When/Then:
- Given que a busca retornou mais de um resultado, When a lista de opções for exibida, Then cada item deve conter, no mínimo, nome da cidade e identificação de localidade (estado/país).
- Given que o usuário seleciona uma cidade da lista, When a seleção for confirmada, Then o sistema deve atualizar a tela com os dados daquela cidade.
- Given que o usuário realiza uma nova busca, When a nova consulta for enviada, Then a cidade previamente selecionada deve ser substituída pelo novo contexto de busca.

### FR3. Exibição do clima atual
O sistema deve mostrar o clima atual da cidade selecionada.

Given/When/Then:
- Given que uma cidade foi selecionada, When os dados forem carregados, Then a tela deve exibir a temperatura atual e a condição climática atual.
- Given que os dados estão disponíveis, When a tela renderizar o clima atual, Then o nome da cidade e, quando disponível, o estado/país devem ser exibidos.
- Given que os dados da API estão incompletos, When a renderização ocorrer, Then o sistema deve mostrar apenas os campos válidos, sem quebrar a tela.

### FR4. Previsão de 5 dias
O sistema deve exibir a previsão para os próximos 5 dias, incluindo o dia atual e 4 dias seguintes.

Given/When/Then:
- Given que a previsão foi carregada, When a tela for renderizada, Then ela deve mostrar exatamente 5 dias consecutivos, começando pelo dia atual.
- Given que cada dia da previsão estiver disponível, When a listagem for exibida, Then cada item deve mostrar, no mínimo, a temperatura mínima e a máxima.
- Given que a previsão tiver dados parciais, When a listagem for renderizada, Then o sistema deve manter o restante da previsão visível e indicar a ausência do dado em vez de quebrar a lista.

### FR5. Alternância entre Celsius e Fahrenheit
O usuário deve poder trocar a unidade de temperatura em toda a interface.

Given/When/Then:
- Given que a aplicação está ativa, When o usuário altera a unidade de temperatura, Then o clima atual e a previsão devem atualizar para a unidade selecionada.
- Given que a unidade foi definida como Fahrenheit, When a conversão for aplicada, Then cada valor exibido deve seguir a fórmula °F = °C × 9/5 + 32.
- Given que a unidade foi alterada, When a sessão continuar, Then a escolha deve permanecer ativa até o fechamento da sessão atual.

### FR6. Tratamento de erro e ausência de dados
O sistema deve tratar falhas de rede, busca sem resultado e dados parcialmente vazios sem quebrar a experiência.

Given/When/Then:
- Given que a API falha ou responde com erro, When a busca for executada, Then o sistema deve exibir uma mensagem de erro clara e permitir nova tentativa.
- Given que a busca não retorna resultados, When a consulta for concluída, Then o sistema deve informar que nenhum resultado foi encontrado.
- Given que a API retorna dados incompletos, When a renderização ocorrer, Then o sistema deve mostrar os dados disponíveis e marcar os ausentes como indisponíveis.

### FR7. Indicador de carregamento e estado de espera
O sistema deve informar ao usuário que a busca está em andamento.

Given/When/Then:
- Given que o usuário envia uma busca, When a consulta estiver em andamento, Then o sistema deve mostrar estado de carregamento explícito.
- Given que a busca estiver em andamento, When a resposta demorar mais que o esperado, Then o sistema deve manter o botão/estado em um estado legível e não bloquear a interação do usuário.

### FR8. Mobile-first e legibilidade
O sistema deve considerar mobile como prioridade e manter boa legibilidade em telas pequenas.

Given/When/Then:
- Given que o usuário acessa a aplicação em smartphone, When a tela principal for carregada, Then os principais elementos (campo de busca, temperatura e previsão) devem estar visíveis sem rolagem complexa.
- Given que a aplicação está em mobile, When o conteúdo for renderizado, Then os textos e controles devem manter legibilidade adequada e interação confortável.

## User Stories

- Como Maria, profissional em trânsito, quero buscar rapidamente uma cidade para decidir o que vestir antes de sair de casa.
- Como Carlos, viajante e planejador de rota, quero comparar cidades e datas para escolher melhor o destino da viagem.
- Como Ana, usuária casual, quero consultar o clima atual de forma clara e direta, sem precisar interpretar dados técnicos.
- Como Maria, quero alternar entre Celsius e Fahrenheit para manter a leitura confortável de acordo com o contexto de uso.
- Como Ana, quero receber mensagens claras quando a busca não encontrar resultados ou quando a API falhar.
- Como Carlos, quero ver a previsão de 5 dias em uma estrutura fácil de comparar para planejar a agenda.

## Acceptance Criteria

### Global acceptance criteria
- A aplicação funciona em navegadores modernos e em telas mobile.
- O usuário consegue consultar o clima atual de uma cidade válida em até 3 ações principais: inserir texto, confirmar busca e visualizar resultado.
- O sistema apresenta corretamente os estados de sucesso, múltiplos resultados, ausência de resultado e erro de API.
- A interface mantém consistência entre os valores exibidos e a unidade de temperatura ativa.

## Non-Functional Requirements

### NFR1. Usabilidade
A interface deve ser direta e intuitiva, com foco em leitura rápida.

Acceptance Criteria:
- O usuário consegue entender como procurar uma cidade sem instruções adicionais.
- A informação principal aparece acima do restante da tela.

### NFR2. Responsividade
A interface deve funcionar em smartphones e também em telas maiores.

Acceptance Criteria:
- O layout principal continua funcional em largura mínima de 360px.
- Os elementos interativos permanecem acessíveis e legíveis em mobile.

### NFR3. Performance
A aplicação deve responder rapidamente às consultas.

Acceptance Criteria:
- A tela principal deve renderizar os dados essenciais em até 3 segundos em rede móvel comum.
- Requisições redundantes devem ser evitadas quando houver dados recentes em cache.

### NFR4. Confiabilidade
A aplicação deve tratar falhas de forma segura e previsível.

Acceptance Criteria:
- Falhas de rede ou dados incompletos não quebram a interface.
- O sistema mantém feedback claro ao usuário em todos os cenários de erro.

### NFR5. Acessibilidade
A aplicação deve seguir boas práticas de acessibilidade.

Acceptance Criteria:
- Os campos de busca e os controles de temperatura possuem labels acessíveis.
- O foco de teclado é visível.
- O conteúdo principal é legível e o contraste é suficiente para leitura em mobile.

### NFR6. Segurança
A aplicação deve tratar entradas e respostas de API com cuidado.

Acceptance Criteria:
- A entrada do usuário é validada antes do envio da busca.
- Dados externos não são exibidos em logs ou em mensagens técnicas ao usuário.

## Edge Cases

- Input vazio: o sistema deve impedir a busca e mostrar mensagem de validação.
- Cidade inexistente: o sistema deve exibir “cidade não encontrada” e manter o usuário em tela estável.
- Múltiplos resultados: o sistema deve listar opções e exigir seleção do usuário.
- Caracteres especiais: o sistema deve aceitar nomes com acentos, cedilha e variações válidas.
- Falha de API: o sistema deve exibir mensagem de erro e permitir nova tentativa.
- Timeout da API: o sistema deve informar indisponibilidade temporária e não travar a tela.
- Geocoding sem resultado: o sistema deve indicar ausência de localidade correspondente.
- Resposta parcial da API: o sistema deve renderizar apenas os campos disponíveis e sinalizar a ausência do dado.

## Assumptions

- A API pública Open-Meteo será usada para geocodificação e clima.
- O produto prioriza uso casual, rápido e sem autenticação.
- O idioma da interface será pt-BR.
- O produto não terá backend próprio nem banco de dados servidor.
- O sistema usará Celsius como padrão e oferecerá conversão para Fahrenheit.
- O escopo inicial não inclui favoritos, histórico persistente, notificações ou alertas meteorológicos.

## Risks

- Dependência de API externa.
- Ambiguidade de busca por nomes repetidos.
- Dados incompletos retornados pela API.
- Rede móvel lenta ou instável.
- Conversão de unidades inconsistentes.
- Falta de clareza no comportamento em cenários de falha.

## Out of Scope

- Autenticação e cadastro.
- Histórico ou favoritos persistidos em servidor.
- Notificações push.
- Mapa interativo.
- Dados meteorológicos avançados como radar, alertas severos e previsão horária detalhada.
- Backend próprio ou persistence de dados.

## Open Questions

- A aplicação deve abrir com cidade padrão, geolocalização automática ou estado vazio?
- A busca deve aceitar apenas cidade ou também região/país?
- Os dados complementares (umidade, vento, precipitação) entram no MVP ou ficam para versões futuras?
