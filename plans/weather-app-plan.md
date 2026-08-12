# Plano Técnico — Weather App

## 1. Architecture Overview

A aplicação será uma solução frontend em React + TypeScript, com foco em mobile-first e sem backend próprio. A arquitetura segue uma divisão simples em camadas:

- UI Layer: componentes React responsáveis pela apresentação, interação e acessibilidade.
- State Layer: estado local do cliente em hooks e componentes, com atualização derivada dos dados de busca e da seleção de cidade.
- Service Layer: funções isoladas para consultar a Open-Meteo e transformar respostas em modelos internos.
- Presentation Logic: conversão de unidades, formatação de texto, cálculo de faixa de previsão e decisões de renderização dos estados de erro/carregamento.

Fluxo principal:

1. O usuário digita o nome da cidade.
2. A busca dispara uma chamada ao serviço de geocoding da Open-Meteo.
3. O sistema valida e processa múltiplas respostas.
4. O usuário seleciona uma cidade.
5. O serviço de forecast carrega o clima atual e a previsão de 5 dias.
6. O estado da tela é atualizado e os componentes renderizam o resultado final.
7. A troca de unidade de temperatura reprocessa os valores exibidos sem recarregar dados da API.


## 2. Tech Stack

- React 19 + TypeScript: base da interface e tipagem estática.
- Vite: ambiente de desenvolvimento e build rápido.
- Tailwind CSS: estilo rápido e consistente com UI dark glassmorphism.
- Vitest + Testing Library: testes unitários e de interação.
- Playwright: testes E2E para navegação e fluxo crítico.
- Open-Meteo API: fonte pública de geocoding e forecast, sem chave de acesso.
- Biome: lint/format do projeto.

Justificativa:

- Mantém a solução leve, sem backend, conforme a spec do MVP.
- Facilita a implementação de layout mobile-first e estados de carregamento/erro.
- Reduz complexidade operacional e acelera a entrega em um cenário de treinamento SDD.


## 3. Project Structure

```text
src/
  components/
    SearchForm.tsx
    CityResultsList.tsx
    WeatherCurrentCard.tsx
    ForecastList.tsx
    TemperatureToggle.tsx
    StatusMessage.tsx
  hooks/
    useCitySearch.ts
    useWeatherQuery.ts
    useTemperatureUnit.ts
  services/
    weatherService.ts
    geocodingService.ts
  types/
    weather.ts
  utils/
    temperature.ts
    formatters.ts
  App.tsx
  main.tsx
  index.css

tests/
  unit/
    weatherService.test.ts
    temperature.test.ts
    SearchForm.test.tsx
  e2e/
    weather-flow.spec.ts
```

Responsabilidades:

- components/: componentes de apresentação e interação.
- hooks/: lógica reutilizável de busca, estado e conversão.
- services/: isolamento da integração com a API pública.
- types/: contratos de dados internos.
- utils/: pure functions para conversão e formatação.
- tests/: cobertura unitária e E2E baseada nos critérios de aceite.


## 4. Data Model

Os tipos devem refletir o contrato mínimo necessário para atender a spec e impedir acoplamento com a API externa.

```ts
export type TemperatureUnit = 'celsius' | 'fahrenheit';

export interface CitySearchResult {
  id: number;
  name: string;
  country?: string;
  admin1?: string;
  latitude: number;
  longitude: number;
  timezone?: string;
}

export interface CurrentWeather {
  temperature: number;
  apparentTemperature?: number;
  weatherCode: number;
  weatherDescription: string;
  isDay: number;
  time: string;
}

export interface ForecastDay {
  date: string;
  temperatureMin: number;
  temperatureMax: number;
  weatherCode: number;
  weatherDescription: string;
}

export interface WeatherSnapshot {
  city: CitySearchResult;
  current: CurrentWeather;
  forecast: ForecastDay[];
  unit: TemperatureUnit;
}

export interface QueryState {
  status: 'idle' | 'loading' | 'success' | 'empty' | 'error';
  message?: string;
  data?: WeatherSnapshot;
  options?: CitySearchResult[];
}
```

Decisões:

- O modelo interno não deve espelhar 1:1 a resposta da Open-Meteo; ele deve ser simplificado para a UX da aplicação.
- A seleção de cidade e a unidade de temperatura permanecem no estado do cliente, não no servidor.
- Cenários incompletos devem ser tratados como campos opcionais, sem quebrar a renderização.


## 5. Data Flow

Fluxo de dados principal:

1. Entrada do usuário em SearchForm.
2. Validação local do input: vazio, somente espaços, ou string com texto válido.
3. Chamada do geocoding service com o termo digitado.
4. Transformação da resposta da API em lista de CitySearchResult.
5. Se houver 0 resultados: estado empty + mensagem “Cidade não encontrada”.
6. Se houver 1 resultado: seleção automática e disparo da consulta de forecast.
7. Se houver múltiplos resultados: renderização da lista e seleção manual do usuário.
8. Chamada ao forecast service usando latitude/longitude e timezone.
9. Conversão da resposta em WeatherSnapshot.
10. Renderização do clima atual + previsão de 5 dias.
11. Alternância de unidade atualiza o display via conversão local, sem nova chamada de API.


## 6. External APIs

### 6.1 Open-Meteo Geocoding

Endpoint principal:

- GET https://geocoding-api.open-meteo.com/v1/search

Parâmetros:

- name: termo informado pelo usuário
- count: 5 ou 10 para limitar resultados relevantes
- language: pt
- format: json

Respostas esperadas:

- results[] com name, country, admin1, latitude, longitude, timezone

Risco: nomes de cidades comuns podem retornar múltiplos resultados; a UI precisa apresentar contexto geográfico para reduzir ambiguidade.

### 6.2 Open-Meteo Forecast

Endpoint principal:

- GET https://api.open-meteo.com/v1/forecast

Parâmetros:

- latitude
- longitude
- current=temperature_2m,apparent_temperature,weather_code,is_day
- daily=weather_code,temperature_2m_max,temperature_2m_min
- timezone=auto
- forecast_days=5

Objetivo:

- Obter clima atual e previsão para 5 dias (dia atual + 4 dias seguintes).

Tratamento de resposta:

- Campos ausentes devem ser tratados como opcionais.
- Erros de rede ou timeouts devem ser convertidos em mensagens amigáveis ao usuário.


## 7. State Management

A complexidade local é baixa, então a solução deve priorizar state local simples em React, sem biblioteca adicional de estado global.

Estratégia:

- useState para estados de busca, seleção e status.
- useMemo para derivar valores exibidos e reduzir recalculos.
- useEffect para sincronizar o carregamento com a seleção de cidade.
- Hooks específicos para encapsular comportamento reuso:
  - useCitySearch(): valida input, dispara geocoding e gerencia resultados.
  - useWeatherQuery(): dispara forecast e atualiza estado de UI.
  - useTemperatureUnit(): concentra a unidade ativa e a conversão de temperaturas.

Vantagens:

- Simplicidade de manutenção.
- Menos boilerplate.
- Adequado ao escopo do MVP sem backend ou autenticação.

Limites:

- Não é a melhor solução para dados compartilhados complexos entre muitos componentes, mas atende ao MVP sem over-engineering.


## 8. Error Handling Strategy

O sistema deve tratar os principais cenários da spec com mensagens claras e UI estável.

Estados:

- idle: tela inicial vazia, sem busca ainda.
- loading: indicador visível enquanto a API responde.
- success: dados válidos renderizados.
- empty: busca sem resultados ou sem cidade ativa.
- error: falha de rede, timeout ou erro da API.

Mensagens esperadas:

- Campo vazio: “Digite o nome de uma cidade”
- Sem resultado: “Cidade não encontrada”
- Falha genérica: “Não foi possível carregar os dados do clima. Tente novamente.”

Políticas:

- Nenhum estado deve quebrar a interface por campos nulos ou faltantes.
- A tela deve manter o input disponível para qualquer nova tentativa.
- Requisições falhas devem permitir retry sem recarregar toda a página.
- Respostas parciais devem renderizar somente o que está disponível.


## 9. Testing Strategy

### 9.1 Unit Tests (Vitest + Testing Library)

Cobrir:

- validação de input vazio
- múltiplos resultados de geocoding
- seleção de cidade e disparo do forecast
- conversão Celsius ⇄ Fahrenheit
- renderização de campos faltantes sem quebrar layout
- estados loading, empty e error

Exemplos:

- SearchForm renderiza botão desabilitado quando input vazio.
- CityResultsList exibe nome e localidade de cada opção.
- WeatherCurrentCard renderiza temperatura e descrição climática.
- ForecastList mostra 5 itens em sequência cronológica.

### 9.2 E2E Tests (Playwright)

Fluxos críticos:

- pesquisa de cidade existente
- seleção em lista com múltiplos resultados
- conversão de unidade
- exibição de erro para cidade inexistente
- carregamento e feedback visual para falhas de API/mock

Critérios de aceitação derivados da spec devem ser automatizados em cenários end-to-end.

### 9.3 Test Data Strategy

- Preferir mocks de resposta da API com payloads realistas e incompletos.
- Cobrir tanto sucesso quanto falhas para garantir que UX e consumo de dados estejam protegidos.
- Evitar mocks excessivos em componentes visuais e priorizar comportamento realista.


## 10. Risks & Trade-offs

### Riscos

- Dependência da Open-Meteo: mudanças de endpoint ou indisponibilidade afetam o produto.
- Ambiguidade na busca por nome de cidade: cidades repetidas em países diferentes exigem UI clara.
- Respostas incompletas: alguns campos podem vir ausentes, exigindo robustez na renderização.
- Rede móvel instável: latência e timeouts podem impactar percepção de velocidade.
- Conversão de unidade: erro de implementação pode gerar inconsistência visual.

### Trade-offs

- Sem backend: reduz custo e complexidade, mas limita histórico, persistência e personalização.
- Estado local: é simples e adequado ao MVP, mas não escala tão bem para múltiplos fluxos ou dados compartilhados complexos.
- API pública: elimina autenticação e infraestrutura, mas aumenta depender do contrato externo.
- UI mobile-first: favorece leitura e velocidade, mas exige atenção rigorosa aos layouts e acessibilidade.

### Decisão final

A arquitetura escolhida prioriza simplicidade, velocidade de entrega e aderência à spec do MVP. Ela resolve bem as necessidades do produto sem introduzir camada extra desnecessária.


## 11. Traceability to Spec

Este plano rastreia diretamente para a specification:

- FR1, FR2, FR3, FR4, FR5, FR6 e FR7 → arquitetura, dados, UI, estados de erro e testes.
- NFR1–NFR6 → acessibilidade, responsividade, confiabilidade, performance e segurança.
- Edge cases → estratégia de validação, fallback e recuperação de erro.
- User Stories → fluxo de uso e componentes principais de interface.

O plano, portanto, funciona como base para a fase de tarefas e implementação mantendo alinhamento com a especificação aprovada.
