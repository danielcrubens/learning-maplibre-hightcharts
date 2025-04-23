# Aprendizado Rápido de MapLibre e Highcharts

Este projeto demonstra a integração de [MapLibre](https://maplibre.org/) e [Highcharts](https://www.highcharts.com) com Nuxt 3, Tailwind e TypeScript, seguindo o princípio de Pareto (80/20) para aprendizado eficiente.


## MapLibre - Conceitos Essenciais

### 1. Inicialização do Mapa
- Criação de instância básica
- Configuração inicial

### 2. Estilos e Fontes de Dados
- Configuração de aparência do mapa
- Conexão com fontes de dados geoespaciais

### 3. Layers
- Adição de camadas de símbolos
- Manipulação de linhas e polígonos
- Gerenciamento de múltiplas camadas

### 4. Interatividade
- Eventos de clique
- Manipulação de hover
- Implementação de pop-ups

### 5. Controles
- Zoom
- Rotação
- Escala

## Highcharts - Conceitos Essenciais

### 1. Configuração Básica
- Estrutura de configuração
- Inicialização do gráfico

### 2. Tipos de Gráficos
- Gráficos de linha
- Gráficos de barra
- Gráficos de pizza
- Gráficos de dispersão

### 3. Séries de Dados
- Estruturação de dados
- Métodos de adição

### 4. Eixos e Legendas
- Personalização de eixo X
- Personalização de eixo Y
- Configuração de legendas

### 5. Eventos e Interatividade
- Manipulação de eventos
- Configuração de tooltips

## Conceitos Aplicados

Neste projeto simples aplicamos os conceitos essenciais (20%) que proporcionam 80% dos resultados:

### MapLibre
- Inicialização básica do mapa
- Adição de marcadores e pop-ups
- Configuração de eventos (cliques)
- Adição de controles básicos
- Uso de fontes GeoJSON

### Highcharts
- Configuração básica de gráficos
- Troca dinâmica de tipos de gráficos
- Atualização de dados em tempo real
- Estilização básica

### Integração
- Comunicação entre os componentes
- Atualização do gráfico baseada nas interações do mapa
- Interface responsiva com Tailwind
- Tipagem com TypeScript

## Componentes Principais

### MapLibreMap.vue
Este componente gerencia um mapa interativo usando MapLibre GL JS:

- **Template**: Container do mapa com painel de instruções
- **Props**: Título customizável
- **Funcionalidades**:
  - Inicialização do mapa centrado em Brasília
  - Controles de navegação e escala
  - Eventos de clique com marcadores e popups
  - Suporte a camadas GeoJSON

### HighchartsGraph.vue
Gerencia visualizações de dados com Highcharts:

- **Template**: Container de gráfico responsivo
- **Props**: 
  - Título
  - Tipo de gráfico
  - Dados
  - Categorias
- **Funcionalidades**:
  - Múltiplos tipos de gráficos
  - Atualização dinâmica de dados
  - Tooltips informativos

### index.vue (Página Principal)
Integra os componentes e gerencia o estado da aplicação:

- **Layout**: Grid responsivo com Tailwind
- **Estado**:
  - Tipo de gráfico atual
  - Dados do gráfico
  - Ponto selecionado
- **Interatividade**:
  - Seleção de pontos no mapa
  - Alternância entre tipos de gráfico
  - Atualização dinâmica de dados

## Fluxo de Interação

1. **Inicialização**:
   - Carregamento do mapa com pontos predefinidos
   - Exibição do gráfico inicial

2. **Interação com o Mapa**:
   - Clique adiciona marcador
   - Atualização do painel de informações
   - Geração de novos dados para o gráfico

3. **Controle de Gráficos**:
   - Mudança dinâmica de tipo
   - Atualização automática da visualização
   - Manutenção dos dados entre transições

## Setup

Instale todas as dependências do projeto



```bash
# npm
npm install

# pnpm
pnpm install

# yarn
yarn install
```

## Development
Rode a aplicação



```bash
# npm
npm run dev

# pnpm
pnpm dev

# yarn
yarn dev
```