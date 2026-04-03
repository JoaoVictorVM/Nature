# Nature

Experiência single-page que combina storytelling ambiental com navegação guiada por tabs, slider e animações suaves para apresentar fauna, florestas e montanhas inspiradas na estética editorial.

## Visão Geral

O site entrega uma landing page fixa onde o usuário consome três blocos temáticos (Animais, Florestas e Montanhas) sem sair do `index.html`. O layout mistura conteúdo visual (imagens em `src/assets/img`) com textos breves, explorando `data-attributes` para controlar abas, o slider de capa e os gatilhos de animação em uma única página estática.

## Funcionalidades

- Menu fixo com scroll suave e versão mobile com botão de hambúrguer.
- Slider introdutório que alterna automaticamente entre imagens e pausa quando o cursor está sobre ele.
- Tabs baseadas em `data-click`/`data-target` para trocar blocos de conteúdo sem recarregar a página.
- Animações desencadeadas por scroll, com `debounce` personalizado para reduzir chamadas em eventos de rolagem.
- Responsividade simples com reordenação e visibilidade adaptativa para telas ≤ 768px.

## Tech Stack

- `HTML5` (sem frameworks) como estrutura principal.
- `CSS3` para regras visuais e responsividade localizada em `src/styles/style.css`.
- `JavaScript ES5` (sem compiladores) para comportamento da UI.
- `jQuery 3.1.1` como camada DOM, localizada em `src/scripts/jquery-3.1.1.min.js`.

## Estrutura do Projeto

- `index.html`  ponto de entrada; importa `style.css` e scripts, define seções (introdução, animais, florestas, montanhas) e elementos com `data-group`.
- `src/styles/style.css`  regras globais, menu fixo, tabs, slider e mobile.
- `src/scripts/app.js`  lógica de tabs, scroll suave, slider, animação ao rolar e botão mobile.
- `src/scripts/debounce.js`  helper reutilizado pela animação ao rolar (implementa versão leve do `lodash.debounce`).
- `src/assets/img/`  imagens usadas no slider e em cada item das seções.

## Começando

### Pré-requisitos

- Navegador moderno (Chrome, Firefox, Edge) com suporte a `CSS3` e `requestAnimationFrame`.
- Para servir localmente, qualquer servidor HTTP simples (Node, Python, Live Server) funciona.

### Instalação

1. Clone o repositório:

   ```bash
   git clone https://github.com/<seu-usuario>/Nature.git
   cd Nature
   ```

2. Não há dependências npm/yard; os arquivos já estão prontos no diretório.

### Variáveis de Ambiente

- Nenhuma variável de ambiente é necessária; todo o comportamento é controlado por HTML e arquivos estáticos.
- Para extensões futuras, adicione um `.env` ou `config.json` e carregue via `fetch`/`import`.

### Executando o projeto

- Recomendado (servidor estático rápido):

  ```bash
  npx http-server -c-1
  ```

- Alternativa em Python:

  ```bash
  python -m http.server 8080
  ```

- Ou abra `index.html` diretamente no navegador, lembrando que alguns navegadores bloqueiam `file://` para `fetch` (não usado aqui).

## Exemplos de Uso

1. Role até a seção Animais e clique em qualquer aba (Fox, FireFox, Wolf) para trocar os textos e imagens sem recarregar a página.
2. Clique nos links do menu fixo para observar o scroll suave; o item correspondente ganha a classe `active` automaticamente.
3. Passe o mouse sobre o slider (`section.introducao`) para pausar a rotação e veja o título ligado à imagem ativa.
4. Reduza a largura da janela para testar o menu mobile e o botão `.mobile-btn` que controla a visibilidade do menu flutuante.

## Explicação da Funcionalidade Principal

- `app.js` usa seletores com `data-attributes` para criar um sistema de tabs: ao clicar em um elemento com `data-click`, são removidas as classes `active` e o conteúdo com `data-target` correspondente é ativado.
- Smooth scroll usa `menuHeight` para compensar o menu fixo e anima a rolagem de `html, body` em 500ms.
- O slider é encapsulado em um IIFE que chama `slider('introducao', 2000)`; ele aplica `setInterval`, pausa ao `hover` e alterna `active` entre as imagens.
- A animação de scroll usa um `debounce` manual para reduzir o número de cálculos no evento `scroll`, adicionando a classe `animate` aos elementos `[data-anime="scroll"]` quando entram na viewport.

## Scripts

1. `src/scripts/jquery-3.1.1.min.js`  biblioteca DOM essencial para manipulação de eventos e animações.
2. `src/scripts/debounce.js`  versão leve de `lodash.debounce`, compartilhada pelo módulo de animação (pode ser importada manualmente ou removida se for replicada dentro de outro bundle).
3. `src/scripts/app.js`  controla tabs, scroll suave, slider e menu mobile; tudo em ES5 e depende de `jQuery` e do `debounce` inline.

## Boas Práticas e Decisões Arquiteturais

- Uso de `data-attributes` (`data-group`, `data-click`, `data-target`, `data-anime`) em vez de IDs/triggers acoplados, facilitando escalabilidade e reuso de padrões.
- Isolamento de comportamentos em IIFEs reduz riscos de poluir o `global scope` e simplifica a adição de novas animações.
- Classe `js` adicionada ao `<html>` permite alternar com facilidade entre estilos com e sem JavaScript (`.js [data-target]`).
- `debounce` reusado evita excesso de `scroll` events; ideal manter como helper separado caso surja toolchain de bundler.
- CSS modularizado por seção, priorizando tipografia (font `Playfair Display`) e layout flexível com floats controlados pela `.container`.

## Diretrizes de Contribuição

1. Fork e clone o repo.
2. Crie uma branch descritiva (`feature/descrição` ou `fix/foo`).
3. Faça mudanças no HTML/CSS/JS mantendo o estilo existente (no caso, ES5 e jQuery).
4. Teste abrindo `index.html` em um servidor local.
5. Abra um PR com descrição, screenshots (se necessário) e referências a issues.
6. Mantemos conversas por Issues antes de abordar mudanças estruturais.
