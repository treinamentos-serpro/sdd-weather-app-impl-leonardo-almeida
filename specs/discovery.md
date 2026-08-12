## Discovery — Aplicação de Previsão do Tempo

### Contexto
A empresa solicitou o desenvolvimento de uma aplicação web para consulta de previsão do tempo. O uso principal da solução é permitir que o usuário obtenha informações climáticas de forma rápida, clara e intuitiva, especialmente em dispositivos móveis.

O produto deve oferecer:
- busca por cidade;
- visualização do clima atual;
- previsão de 5 dias;
- alternância entre Celsius e Fahrenheit;
- experiência otimizada para uso em smartphones.

A aplicação deve priorizar simplicidade, legibilidade e velocidade na consulta, sem exigir cadastro ou autenticação para uso básico.

### Requisitos Funcionais
- Busca por cidade
  - O usuário deve conseguir localizar uma cidade por nome.
  - A busca deve retornar resultados relevantes.
  - Quando não houver resultado, o sistema deve informar de forma clara.
  - Quando houver múltiplas opções, a interface deve permitir a seleção correta.

- Clima atual
  - O sistema deve exibir o clima atual da cidade selecionada.
  - Deve apresentar informações como temperatura e condição do tempo.
  - A visualização deve ser clara e adequada para leitura rápida.

- Previsão de 5 dias
  - O usuário deve visualizar a previsão para os próximos 5 dias.
  - A informação deve ser organizada de forma fácil de comparar entre dias.
  - A previsão deve ser apresentada em uma estrutura compreensível para uso mobile.

- Alternância entre Celsius e Fahrenheit
  - O usuário deve trocar entre unidades de temperatura.
  - A conversão deve ser aplicada de forma consistente em toda a interface.
  - A ação de troca deve ser intuitiva e acessível.

### Requisitos Não-Funcionais
- Usabilidade
  - A aplicação deve ter uma interface simples e direta.
  - O usuário deve entender rapidamente como pesquisar e interpretar os dados.

- Responsividade
  - A interface deve ser adaptada a telas menores, especialmente smartphones.
  - Os controles e textos devem manter boa legibilidade em dispositivos móveis.
  - O layout deve funcionar em diferentes tamanhos de tela, com foco em mobile.

- Performance
  - O sistema deve responder rapidamente às consultas.
  - A renderização da informação deve ser fluida e sem atrasos perceptíveis.
  - O carregamento das informações climáticas deve ocorrer em tempo adequado, mesmo com variação de rede.

- Confiabilidade
  - O sistema deve tratar falhas de rede, dados incompletos e buscas sem resultado.
  - A experiência do usuário não deve quebrar em cenários comuns de erro.

- Acessibilidade
  - A interface deve usar elementos com boas práticas de acessibilidade.
  - A leitura e a navegação devem ser adequadas para usuários com diferentes necessidades.
  - Controles, labels e feedback visual devem ser compreensíveis para usuários com leitores de tela e teclado.

- Compatibilidade
  - A solução deve funcionar em navegadores modernos e em dispositivos móveis comuns.

- Disponibilidade
  - A aplicação deve estar acessível com alta disponibilidade e tolerar falhas temporárias de infraestrutura ou da API externa.
  - Em caso de indisponibilidade de dados, a interface deve informar o usuário de forma clara sem bloquear a navegação.

- Segurança
  - A aplicação deve usar comunicação segura (HTTPS) e validar entradas de usuário e dados recebidos de fontes externas.
  - O tratamento de erros e a manipulação de dados externos não devem expor informações sensíveis.

- Observabilidade
  - O sistema deve registrar erros, falhas de integração e métricas de uso para apoiar monitoramento e melhoria contínua.
  - Logs e relatórios de falha devem facilitar diagnósticos rápidos em produção.

- Eficiência de rede e cache
  - A aplicação deve reduzir o número de chamadas redundantes à API externa.
  - O uso de cache e estratégias de reutilização de dados pode melhorar desempenho e resiliência.

### Riscos
- Dependência de dados externos
  - A disponibilidade e a qualidade da API meteorológica podem afetar a funcionalidade.
  - Falhas temporárias ou limites de uso podem impactar a experiência.

- Ambiguidade de busca
  - O mesmo nome pode corresponder a várias cidades ou localidades.
  - Sem tratamento adequado, pode haver resultados confusos.

- Inconsistência de dados
  - APIs diferentes podem devolver campos e formatos variados.
  - Isso pode exigir padronização antes da exibição.

- Desempenho em mobile
  - A interface pode perder usabilidade se não for pensada especificamente para telas pequenas.

- Conversão de unidades
  - Erros na conversão entre Celsius e Fahrenheit podem gerar inconsistência na experiência do usuário.

### Perguntas em Aberto
- A cidade inicial da aplicação deve ser definida automaticamente pela localização do usuário ou será sempre escolhida manualmente?
- Quando houver múltiplos resultados para uma busca, a aplicação deve mostrar uma lista de opções para o usuário ou escolher a melhor correspondência automaticamente?
- A previsão de 5 dias deve incluir apenas temperatura mínima e máxima, ou também outros indicadores, como umidade e vento?
- O produto deve contemplar apenas os dados essenciais do clima ou também informações complementares?
- A busca deve aceitar apenas nomes de cidades ou também regiões, países e outros filtros geográficos?
- Qual unidade deve ser usada como padrão inicial na tela: Celsius, Fahrenheit ou a preferência do usuário?

### Decisões
- Fonte de dados: Open-Meteo (sem API key)
  - Justificativa: a API pública oferece geocodificação e previsão, sem custo de autenticação e sem exigir credenciais de cliente, o que reduz atrito de setup e acelera o desenvolvimento inicial.
  - Resolve: define a tecnologia de dados do produto e elimina a incerteza sobre a origem dos dados climáticos e a necessidade de chaves de acesso.

- "5 dias" = hoje + 4 dias
  - Justificativa: a definição clara de janela de previsão evita ambiguidade de escopo e facilita a modelagem e a apresentação da interface, mantendo o foco em uma previsão curta e útil para uso diário.
  - Resolve: responde diretamente à pergunta sobre o que exatamente está incluído na previsão de 5 dias e padroniza a expectativa do usuário e do time.

- Unidade padrão: Celsius
  - Justificativa: Celsius é a unidade mais comum para uso geral no Brasil e na maior parte das aplicações de clima brasileiras, além de ser a base mais intuitiva para a audiência principal do produto.
  - Resolve: fecha a dúvida sobre a unidade inicial da tela e define a base para a conversão para Fahrenheit como função complementar do usuário.

- Sem autenticação e sem persistência de servidor
  - Justificativa: a aplicação prioriza uso rápido e sem fricção, sem cadastro e sem armazenamento do lado do servidor; isso reduz complexidade operacional e atende ao objetivo de consulta imediata e simples.
  - Resolve: elimina a necessidade de decidir fluxo de login, dados do usuário e arquitetura de backend, além de simplificar a primeira versão do produto.

- Idioma da UI: pt-BR
  - Justificativa: o público-alvo e o contexto do projeto são brasileiros e a aplicação deve priorizar legibilidade, familiaridade e clareza na linguagem para usuários do mercado local.
  - Resolve: define a interface em português do Brasil e elimina a incerteza sobre idioma da experiência, especialmente em textos, rótulos e feedbacks do sistema.

### Suposições
- A aplicação será desenvolvida como uma solução web responsiva, com foco principal em dispositivos móveis.
- A busca por cidade será feita por meio de uma API pública de geocodificação e previsão do tempo.
- O usuário não precisará se autenticar para utilizar as funcionalidades principais.
- O clima atual e a previsão de 5 dias atendem ao objetivo funcional principal do produto.
- A conversão entre Celsius e Fahrenheit será consistente em toda a interface.
- O projeto deve priorizar simplicidade e clareza de uso em vez de recursos avançados.
- A primeira versão será funcional e centrada em usabilidade e confiabilidade básica.