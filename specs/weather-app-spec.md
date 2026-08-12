# Especificação do Produto — Weather App

## Overview

A Weather App é uma aplicação web responsiva para consulta do clima em cidades, com foco principal em uso mobile. O produto deve permitir que o usuário pesquise uma cidade, visualize o clima atual e a previsão para os próximos 5 dias, e alterne entre Celsius e Fahrenheit sem exigir autenticação ou backend próprio.

A solução deve priorizar velocidade, clareza e legibilidade em telas pequenas. A interface será em português do Brasil e usará a API Open-Meteo como fonte principal de dados, sem necessidade de chave de acesso.

O escopo do MVP inclui:
- busca por cidade;
- listagem de resultados quando houver múltiplas cidades;
- clima atual da cidade selecionada;
- previsão para 5 dias, definindo hoje + 4 dias consecutivos;
- alternância de unidade de temperatura;
- estados de carregamento, erro e ausência de resultado;
- layout mobile-first com boa legibilidade.

## Functional Requirements

### FR1. Busca por cidade
O sistema deve permitir que o usuário envie o nome de uma cidade e inicie a consulta do clima correspondente.

Critérios de aceite (Given/When/Then):
- Given que o usuário está na tela inicial, When digita um nome de cidade e aciona a busca, Then o sistema deve validar a entrada e iniciar a consulta.
- Given que o campo de busca está vazio ou contém apenas espaços, When o usuário tenta buscar, Then o sistema deve impedir a ação e exibir a mensagem “Digite o nome de uma cidade”.
- Given que a busca retorna um único resultado válido, When a resposta for recebida, Then o sistema deve selecionar automaticamente a cidade e renderizar o clima atual e a previsão.
- Given que a busca retorna mais de um resultado, When o retorno for processado, Then o sistema deve apresentar uma lista de opções com as cidades encontradas.
- Given que a busca não retorna registros, When a resposta for processada, Then o sistema deve exibir a mensagem “Cidade não encontrada” e manter o campo de busca disponível para nova tentativa.

### FR2. Seleção de cidade em resultados múltiplos
Quando houver mais de uma localidade associada ao termo pesquisado, a aplicação deve permitir a seleção explícita da cidade correta.

Critérios de aceite (Given/When/Then):
- Given que a busca retornou múltiplos resultados, When a lista é exibida, Then ela deve conter, no mínimo, o nome da cidade e a localidade associada, como estado, país ou região.
- Given que o usuário seleciona uma cidade da lista, When a opção for acionada, Then o sistema deve atualizar os dados da tela com o clima da cidade escolhida.
- Given que uma nova busca é iniciada, When a nova resposta for recebida, Then o sistema deve substituir a seleção anterior pelo novo contexto da busca.

### FR3. Exibição do clima atual
O sistema deve apresentar o clima atual da cidade selecionada com temperatura e condição climática.

Critérios de aceite (Given/When/Then):
- Given que uma cidade foi selecionada, When os dados forem carregados, Then a tela deve exibir ao menos a temperatura atual e a descrição da condição climática.
- Given que o nome da cidade e a localidade estiverem disponíveis, When a tela renderizar, Then o sistema deve exibir o nome da cidade e, quando disponível, o estado ou país.
- Given que a API retorna valores parciais, When a renderização ocorrer, Then o sistema deve mostrar os campos válidos e manter a interface funcional sem quebrar o layout.

### FR4. Previsão de 5 dias
O sistema deve mostrar a previsão do tempo para os próximos 5 dias, com o dia atual e mais 4 dias consecutivos.

Critérios de aceite (Given/When/Then):
- Given que a previsão foi carregada com sucesso, When a tela for apresentada, Then o sistema deve mostrar 5 itens de previsão em sequência cronológica, começando pelo dia atual.
- Given que os dados da previsão estiverem disponíveis, When cada item for renderizado, Then ele deve exibir, no mínimo, a temperatura mínima e a máxima do dia.
- Given que alguns dados da previsão estiverem ausentes, When a lista for renderizada, Then o sistema deve manter o restante visível e indicar a ausência do dado sem interromper a leitura da previsão.

### FR5. Alternância entre Celsius e Fahrenheit
O usuário deve poder alternar a unidade de temperatura exibida em toda a interface.

Critérios de aceite (Given/When/Then):
- Given que a aplicação está em uso, When o usuário ativa a troca de unidade, Then o sistema deve atualizar o clima atual e a previsão para a unidade selecionada.
- Given que a unidade selecionada é Fahrenheit, When os valores forem convertidos, Then cada temperatura deve refletir a conversão correspondente e manter consistência visual na tela.
- Given que a unidade foi alterada, When a sessão continuar, Then a seleção deve permanecer ativa até que o usuário altere a unidade novamente.

### FR6. Indicador de carregamento e feedback de erro
O sistema deve informar ao usuário quando a consulta está em andamento e quando a operação falha.

Critérios de aceite (Given/When/Then):
- Given que o usuário enviou a busca, When a consulta estiver em andamento, Then o sistema deve exibir um indicador de carregamento visível.
- Given que a API falha ou não responde, When a operação finalizar, Then o sistema deve exibir uma mensagem de erro e permitir nova tentativa.
- Given que a busca não encontra resultados, When a resposta for processada, Then o sistema deve mostrar uma mensagem de ausência de resultado sem bloquear a navegação.

### FR7. Layout mobile-first e legibilidade
O sistema deve priorizar a experiência em dispositivos móveis, com foco em leitura rápida e navegação simples.

Critérios de aceite (Given/When/Then):
- Given que o usuário acessa a aplicação em largura de 360px, When a página principal for carregada, Then os elementos principais de busca, clima atual e previsão devem estar visíveis sem exigir múltiplas interações.
- Given que a interface está em smartphone, When textos e controles forem renderizados, Then eles devem manter boa legibilidade, com tamanho e contraste adequados.
- Given que os dados forem exibidos em mobile, When a tela for montada, Then busca, resultado e previsão devem ficar organizados sem sobreposição de conteúdo.

## User Stories

- Como usuário casual, quero buscar uma cidade para consultar o clima atual com rapidez e sem cadastro, para obter a informação necessária em segundos.
- Como viajante, quero comparar a previsão de 5 dias entre cidades para escolher o melhor destino com base no clima esperado.
- Como pessoa em trânsito, quero visualizar a temperatura atual e a condição climática da cidade selecionada para me preparar antes de sair de casa.
- Como usuário brasileiro, quero alternar entre Celsius e Fahrenheit para ajustar a leitura da temperatura ao meu contexto de uso.
- Como usuário que busca uma cidade específica, quero receber uma lista de resultados relevantes quando houver múltiplas opções para selecionar a localidade correta.
- Como usuário em mobile, quero ver os dados do clima em uma interface legível e organizada para consultar informações rapidamente no smartphone.
- Como usuário em caso de problema, quero receber mensagens claras de erro ou ausência de resultado para entender o que aconteceu e tentar novamente.

## Acceptance Criteria

### Critérios gerais de aceitação
- A aplicação funciona em navegadores modernos e em dispositivos móveis comuns.
- O usuário consegue consultar o clima de uma cidade em até três ações principais: digitar o nome da cidade, confirmar a busca e visualizar o resultado.
- O sistema cobre os estados de sucesso, múltiplos resultados, ausência de resultado e falha de rede ou API.
- A interface mantém consistência entre os valores exibidos e a unidade de temperatura escolhida.
- A interação é clara e acessível em mobile, com foco em legibilidade e simplicidade.

### Critérios por história
- História de busca: o usuário consegue localizar uma cidade válida e obter o clima correspondente.
- História de seleção: quando há múltiplos resultados, o usuário consegue identificar e escolher a cidade correta.
- História de clima atual: o sistema apresenta temperatura e condição climática de forma evidente e limpa.
- História de previsão: a previsão de 5 dias é apresentada em uma estrutura comparável e fácil de ler.
- História de conversão de unidade: a alternância entre Celsius e Fahrenheit atualiza a interface de forma consistente.
- História de erro: mensagens de erro e ausência de dados orientam o usuário sem interromper a navegação.

## Non-Functional Requirements

### NFR1. Usabilidade
A aplicação deve ser intuitiva, direta e fácil de aprender.

Critérios de aceite:
- O fluxo principal pode ser entendido sem instruções complementares.
- A informação mais importante é exibida com destaque na tela inicial.
- Os rótulos e mensagens são compreensíveis para usuários sem conhecimento técnico.

### NFR2. Responsividade
A interface deve funcionar em diferentes larguras de tela, com ênfase em smartphones.

Critérios de aceite:
- O layout permanece funcional em telas de 360px de largura e acima.
- Elementos interativos e textos mantêm legibilidade em dispositivos móveis.
- A organização visual não compromete o uso em orientação vertical ou horizontal.

### NFR3. Performance
A aplicação deve responder de forma ágil às ações do usuário.

Critérios de aceite:
- A renderização dos dados principais ocorre em tempo aceitável em redes móveis comuns.
- A aplicação evita requisições redundantes e reutiliza dados quando possível.
- O carregamento e a atualização de conteúdo não geram atraso perceptível na experiência.

### NFR4. Confiabilidade
A solução deve lidar com falhas e dados incompletos sem quebrar a interface.

Critérios de aceite:
- Falhas de rede, erros de API e respostas incompletas não interrompem a experiência.
- O sistema mantém mensagens claras e consistentes para todos os cenários de erro.
- A aplicação continua operável mesmo quando informações extras não estejam disponíveis.

### NFR5. Acessibilidade
A interface deve favorecer uso com teclado, leitura e compreensão por pessoas com diferentes necessidades.

Critérios de aceite:
- Campos e controles possuem rótulos ou identificadores acessíveis.
- A navegação por teclado mantém foco visível e previsível.
- Textos, contrastes e áreas de interação mantêm boa legibilidade em mobile.

### NFR6. Segurança
A aplicação deve tratar dados externos e entradas do usuário com cuidado.

Critérios de aceite:
- A entrada do usuário é validada antes de ser enviada para busca.
- Dados e mensagens vindos de serviços externos não são expostos em logs ou detalhes técnicos ao usuário final.
- A aplicação utiliza comunicação segura para acesso à API e não requer sensibilidade de dados do usuário.

## Edge Cases

- Entrada vazia no campo de busca: o sistema deve impedir a consulta e informar ao usuário que é necessário digitar uma cidade.
- Cidade inexistente: a aplicação deve exibir mensagem de “cidade não encontrada” e manter a tela em estado estável.
- Múltiplos resultados para um mesmo nome: a interface deve permitir a escolha correta entre as opções disponíveis.
- Nomes com acentos ou caracteres especiais: o sistema deve aceitar e tratar corretamente a busca por cidades com grafias variadas.
- Falha de rede: a aplicação deve mostrar erro de conexão e permitir nova tentativa.
- Timeout de resposta da API: a interface deve informar indisponibilidade temporária sem travar a experiência.
- Resposta incompleta da API: o sistema deve renderizar somente os dados válidos e indicar ausência de informação quando necessário.
- Cidade parcialmente conhecida: se a API retornar mais de uma coincidência ou dados ambíguos, a aplicação deve priorizar clareza na seleção.
- Unidade ativa em sessão: quando o usuário alterna entre Celsius e Fahrenheit, nenhuma parte da tela deve continuar exibindo a unidade anterior de forma inconsistente.

## Assumptions

- A fonte de dados será a API pública Open-Meteo, sem necessidade de credenciais do usuário.
- A aplicação será uma solução web responsiva, com foco em uso mobile.
- A interface será em português do Brasil.
- A aplicação não exigirá autenticação nem cadastro para uso básico.
- A unidade padrão da tela será Celsius, com opção de conversão para Fahrenheit.
- A previsão contemplará os próximos 5 dias, definidos como hoje + 4 dias consecutivos.
- O MVP não incluirá backend próprio, persistência de dados do usuário ou histórico de buscas no servidor.
- O escopo inicial prioriza simplicidade, velocidade e clareza sobre recursos avançados.

## Risks

- Dependência de uma API externa: indisponibilidade, latência ou mudanças de contrato podem impactar a funcionalidade.
- Ambiguidade na busca por cidade: nomes repetidos em diferentes estados ou países podem gerar confusão.
- Dados incompletos: a API pode devolver campos ausentes ou inconsistentes.
- Rede móvel instável: consultas em redes lentas podem aumentar o tempo de resposta e afetar a percepção de performance.
- Conversão de unidade: erros na conversão entre Celsius e Fahrenheit podem causar inconsistência na interface.
- Falta de clareza em cenários de falha: mensagens pouco objetivas podem gerar frustração e reduzir a confiança do usuário.

## Out of Scope

- Autenticação e cadastro de usuários.
- Persistência de dados em servidor, histórico de buscas ou favoritos.
- Notificações push ou alertas meteorológicos automáticos.
- Mapa interativo, radar e imagens de satélite.
- Previsão horária detalhada, alertas severos e dados meteorológicos avançados.
- Funcionalidades de geolocalização automática, salvo se definidas em um escopo futuro.
- Integração com redes sociais, compartilhamento ou personalização de perfil.

## Open Questions

- A aplicação deve abrir com uma cidade inicial padrão, com geolocalização automática ou com tela vazia até o usuário buscar?
- A busca deve aceitar somente nomes de cidades ou também regiões, estados e países relevantes para a consulta?
- Os dados complementares, como umidade, vento e precipitação, devem entrar no MVP ou ficar para versões futuras?
- A conversão de temperatura deve ser aplicada apenas na visualização ou também em qualquer mecanismo de comparação interna?
- O produto precisa de um estado de “localização atual” para facilitar consultas recorrentes em um mesmo dispositivo?
- A aplicação deve armazenar pesquisas recentes somente no cliente ou esse comportamento é considerado fora do escopo inicial?
