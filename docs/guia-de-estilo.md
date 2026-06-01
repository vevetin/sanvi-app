# Guia de Estilo de Interface e Interação

---

## 1. Introdução

### 1.1 Objetivo do Guia de Estilo

Este documento estabelece diretrizes formais para o projeto, desenvolvimento e avaliação da interface e da interação de sistemas computacionais interativos, com o objetivo de garantir consistência, qualidade de uso e alinhamento com princípios de UI/UX

- **Escopo do sistema:** PWA (Progressive Web App) mobile-first e offline-first para gerenciamento de listas de compras colaborativas.

- **Problemas que o guia pretende mitigar:** Fricção física no uso do celular durante as compras (ex: mãos ocupadas empurrando carrinho), frustração com falhas de rede em zonas de sombra (ex: subsolo do mercado) e alta carga cognitiva na marcação de itens.

- **Benefícios esperados:** Consistência ergonômica voltada para o uso com apenas uma mão (a "zona do polegar"), visibilidade clara do estado do sistema (feedback de rede) e eficiência/rapidez na conclusão das tarefas de compra.

---

### 1.2 Organização do Documento

Este guia está estruturado em seções que abrangem desde o contexto de uso até os elementos de interface, interação e padrões de comunicação.

- **Estrutura geral do documento:** Contexto de uso (personas e cenários), elementos visuais, elementos de interação baseados em gestos, vocabulário do aplicativo e diretrizes de usabilidade transversais.

- **Tipos de conteúdo incluídos:** Leis aplicadas de Interação Humano-Computador (IHC), regras ergonômicas obrigatórias, padrões visuais e exceções de contexto.

- **Nível de detalhamento:** Operacional e conceitual (focado na implementação da interface e nas justificativas de experiência do usuário).


### 1.3 Público-Alvo

Este guia destina-se aos stakeholders envolvidos no ciclo de vida do sistema interativo.

- **Perfis envolvidos:** Desenvolvedores Front-end e Back-end, Designers de UI/UX, Profissionais de Qualidade (QA) e Product Owners.

- **Nível de conhecimento esperado:** Familiaridade com desenvolvimento ágil, princípios básicos de PWA e noções de design de interfaces móveis.

### 1.4 Diretrizes de Utilização

Define como o guia deve ser aplicado ao longo do processo de design e desenvolvimento.

- **Fases de uso:** Deve ser consultado ativamente durante a prototipação rápida (RAD) e ao longo do desenvolvimento front-end do PWA.

- **Forma de aplicação das regras:** As regras ergonômicas de manipulação com uma mão e os feedbacks de estado offline são obrigatórios. Questões puramente estéticas são recomendadas.

- **Integração com ferramentas e processos:** O guia deve ser utilizado como base de validação durante o Code Review e deve compor os critérios de aceite visual no Definition of Done (DoD) das Histórias de Usuário.

---

### 1.5 Manutenção e Evolução

Define o processo de governança do guia.

- **Responsáveis pela manutenção:** Equipe de desenvolvimento e especialistas em UX.

- **Processo de atualização:** O guia será um documento vivo (iterativo), sendo atualizado sempre que testes de aceitação ou feedbacks práticos de uso revelarem novas necessidades ou comportamentos no ambiente do supermercado.

- **Controle de versão:** O documento será versionado no repositório do projeto, acompanhando o código-fonte e o quadro Kanban.

## 2. Contexto de Uso e Resultados de Análise

### 2.1 Ambiente de Uso

Descreve o contexto em que o sistema será utilizado, considerando fatores humanos, organizacionais e tecnológicos.

- **Perfil dos usuários:** O foco principal são os "Compradores Planejadores" e "Famílias".
    - **Proto-Persona (Mariana Silva, 34 anos):** Analista Administrativa e mãe. Faz compras mensais de abastecimento acompanhada do filho pequeno. No mercado, está em "modo de execução" e tem pressa. Frustra-se se o app trava ou exige duas mãos. *Insight: "No supermercado, se o aplicativo trava sem internet ou exige muitos toques, ele vira um obstáculo. Preciso de algo que acompanhe meu ritmo natural."*

- **Contexto físico (ambiente, dispositivos):**

    - O dispositivo primário é o smartphone (mobile).
    
    - O ambiente é dinâmico, frequentemente barulhento e repleto de distrações (atenção dividida do usuário).
    
    - **Cenário Crítico (A Zona de Sombra no Subsolo):** Mariana chega à seção de laticínios no subsolo do hipermercado, onde o sinal 4G desaparece. Ela tenta marcar itens com apenas uma mão enquanto a outra segura o carrinho. O contexto físico exige que a interface funcione perfeitamente nesse cenário de isolamento de rede e restrição motora.

- **Contexto organizacional:** Gestão colaborativa do orçamento familiar, exigindo sincronização de dados (quando online) para evitar que membros da casa comprem itens duplicados ou esqueçam produtos essenciais.

- **Principais tarefas:** Visualizar itens pendentes, marcar produtos como "comprados" no ponto de venda e restaurar itens marcados por engano de forma ágil.

- **Restrições e limitações:**
    - **Físicas/Motoras:** O usuário opera o dispositivo com apenas uma mão (limitado à área de alcance do polegar) pois a outra mão empurra o carrinho ou carrega cestas.
    
    - **Tecnológicas:** Oscilação ou ausência total de conexão com a internet durante a execução das tarefas.
    
    - **Atenção:** Alto nível de interrupções, exigindo uma interface limpa que permita ao usuário retomar a tarefa de onde parou instantaneamente.

## 3. Elementos de Interface

### 3.1 Disposição Espacial e Grid

Define a organização estrutural da interface, focada na usabilidade móvel.

- **Sistema de grid:** Layout focado no formato "Compacto" (telas menores que 600dp), utilizando um painel único (single-pane) para evitar distrações.

- **Margens e espaçamentos:** Utilizar margens laterais de 16dp. Para mitigar ativações falsas (cliques acidentais), os alvos de toque (botões, itens da lista) devem ser separados por um espaçamento mínimo de 8dp.

- **Regras de alinhamento:** Seguindo a Lei de Fitts e a otimização para a "Zona do Polegar", as ações primárias e menus de navegação devem ser alinhados na metade inferior ou no centro da tela, evitando a área superior que é de difícil alcance com apenas uma mão.

<!-- 
Diagrama da Zona do Polegar (Mapa de Calor): Uma ilustração de um smartphone mostrando as áreas de alcance do polegar. Use cores (ex: azul/verde para "Mais preciso/Confortável" e vermelho para "Menos preciso/Inconveniente") ilustrando que a área inferior é a ideal para as ações primárias, baseando-se nas premissas da Lei de Fitts.

Wireframe Estrutural: Uma imagem delineando as margens laterais de 16dp e o espaçamento mínimo obrigatório de 8dp entre os alvos de toque.  
-->

- **Responsividade:** O design é primariamente projetado para o uso móvel em orientação retrato, ajustando a densidade de informações e mantendo as restrições ergonômicas caso seja acessado em telas maiores (tablets).

### 3.2 Janelas e Contêineres

Define a estrutura e comportamento de janelas e componentes de layout.

- **Tipos de janelas:** Priorizar o layout de painel único (single-pane) para manter o foco absoluto na tarefa (fazer compras).

- **Regras de abertura/fechamento:** Menus de ação ou diálogos de confirmação devem utilizar painéis inferiores (Bottom Sheets) em vez de janelas modais centralizadas, para manter as ações de confirmação acessíveis ao alcance do polegar.

<!--
Mockup de Comparação (Certo / Errado): Mostrando a diferença entre um "Bottom Sheet" (painel inferior, que é o Certo por estar na zona de alcance) e uma janela modal flutuando no centro da tela (que é o Errado para uso com uma mão).
-->

- **Hierarquia visual:** Utilizar agrupamento explícito (contêineres sutis ou linhas divisórias) para separar claramente os itens "Pendentes" dos itens "Comprados", baseando-se no princípio gestáltico da Região Comum.

<!--
Exemplo de Agrupamento Explícito: Uma imagem ilustrando a lista de produtos, mostrando o uso de linhas divisórias ou contêineres sutis delimitando claramente o grupo de itens "Pendentes" e o grupo de itens "Comprados" (princípio da Região Comum da Gestalt).
-->

### 3.3 Tipografia

Define padrões tipográficos para garantir legibilidade e consistência, considerando o ambiente de movimento (supermercado).

- **Famílias tipográficas:** Utilizar fontes sem serifa (sans-serif) de alta legibilidade, adequadas para a varredura rápida de texto em telas digitais.

- **Hierarquia textual:** Garantir clareza diferenciando categorias de produtos (títulos com maior peso) e nomes dos itens (texto regular).

<!--
Tabela de Escala Tipográfica (Type Scale): Uma tabela que defina visualmente os estilos. Colunas recomendadas: Nome do Estilo (ex: Título da Categoria, Nome do Produto), Peso da Fonte (ex: Bold, Regular), Tamanho em sp ou rem, e uma coluna com um exemplo visual do texto renderizado.
-->

- **Tamanhos e pesos:** O design deve suportar o redimensionamento do texto pelo usuário em até 200% sem quebrar o layout ou exigir barra de rolagem horizontal (critério de acessibilidade).

<!--
Demonstração de Acessibilidade (Redimensionamento): Uma imagem dividida em duas mostrando o layout normal lado a lado com o texto ampliado em 200%. Isso provará visualmente que a lista não quebra nem gera barra de rolagem horizontal ao ser ampliada.
-->

### 3.4 Elementos Não Tipográficos

Define o uso de ícones, ilustrações e sinais visuais.

- **Estilo de ícones:** Sólidos e de rápido reconhecimento (ex: marcadores de checklist, ícones de carrinho).

- **Indicadores visuais:** Presença obrigatória de ícones de status para indicar o estado de rede do PWA (Sincronizado, Sincronizando, Offline).

- **Regra de Ergonomia (Alvos de Toque):** Todos os elementos interativos, independentemente do tamanho visual do ícone, devem ter uma área mínima selecionável de 48x48 dp (pixels independentes de densidade) para garantir a precisão no toque.

<!--
Diagrama de Alvo de Toque (Touch Target): Como a precisão do toque é crucial no seu app, adicione uma imagem técnica (blueprint) de um ícone (ex: o checkbox). Mostre o ícone desenhado em 24x24dp, mas com uma "caixa delimitadora" (bounding box) translúcida em volta mostrando a área real de clique medindo 48x48 dp.
-->

**Regra geral:** Ícones e símbolos visuais devem extrair seus significados do mundo real, evitando abstrações complexas voltadas apenas ao entendimento do sistema . Sempre que um ícone puder gerar ambiguidade ou não possuir um significado universal óbvio, ele deve ser obrigatoriamente acompanhado de uma etiqueta de texto (text label) . 

<!--
Exemplo de Rótulo de Ícones (Text Labels): Um comparativo visual para justificar a regra de ambiguidade. Mostre um ícone isolado (ex: o ícone do aplicativo) e, ao lado, o mesmo ícone acompanhado do seu rótulo de texto (ex: "Início"), reforçando que ícones sem texto aumentam a carga cognitiva.
-->

**Justificativa:** Ícones verdadeiramente universais são raros e podem significar ações diferentes dependendo da experiência e do conhecimento de cada pessoa . Se o ícone não for reconhecido, ele se torna ruído visual e aumenta a carga cognitiva. O acréscimo de texto reduz a abstração, garante clareza e facilita a descoberta . 

**Exemplo:** Utilizar o ícone de uma "casa" acompanhado da palavra "Início" na barra de navegação principal . 

**Contraexemplo:** Utilizar uma interface de "CD Player" (botões de play, pause, stop) para controlar um software antivírus, gerando falsas expectativas no usuário sobre a função do sistema .

### 3.5 Cores

Define o sistema cromático da interface.

- **Paleta primária e secundária:** Devem utilizar cores que evitem distrações, mantendo a interface limpa e focada no conteúdo (a lista).

- **Cores semânticas:** Vermelho exclusivo para erros ou ações destrutivas (ex: "Excluir Lista"); cores distintas (ex: tons de cinza ou amarelo) para alertar sobre a falta de conexão com a Internet.

<!--
Paleta de Cores (Swatches): Uma grade ou tabela exibindo pequenos blocos pintados com as cores exatas do projeto. Deve conter: Nome (ex: Cor Primária, Fundo, Erro/Ação Destrutiva), o Código Hexadecimal correspondente e o contexto de uso (ex: vermelho para excluir, cinza/amarelo para offline).
-->

- **Regras de contraste e acessibilidade:** A cor nunca deve ser o único meio de transmitir uma informação (visando usuários com daltonismo). Elementos de interface como botões devem ter um contraste mínimo de 3:1 em relação ao fundo.

<!--
Mockup de Contraste 3:1: Uma imagem ilustrando a regra de acessibilidade visual. Mostre um botão sobre o fundo da tela apontando que a diferença de contraste entre a cor do contêiner do botão e a cor de fundo obedece à proporção mínima de 3:1 para garantir legibilidade.
-->

### 3.6 Animações e Transições

Define o comportamento dinâmico da interface.

- **Tipos de animação:** Animações para o gesto de swipe (deslizar itens), carregamento de estado offline, e transições de status (marcar como comprado).

- **Duração e curvas:** Devem ser rápidas, fluidas e sutis, durando no máximo 250 milissegundos para não atrasar a execução da tarefa do usuário.

- **Finalidade:** Fornecer feedback visual imediato após interações para reduzir a incerteza. Contudo, as animações não devem ser excessivas a fim de evitar problemas de acessibilidade para usuários com sensibilidade ao movimento.

<!--
Storyboard de Transição: Como não é possível embutir um vídeo interativo no Markdown, utilize um storyboard clássico de UI (3 ou 4 quadros estáticos lado a lado). Por exemplo, mostrando a sequência do usuário fazendo o gesto de swipe em um item, o item deslizando, e o feedback visual, com uma anotação temporal abaixo indicando que o processo visual inteiro se conclui em no máximo 250ms.
-->

## 4. Elementos de Interação

### 4.1 Estilos de Interação

Define os modelos de interação adotados.

- **Tipos de interação utilizados:** Predominância do estilo de Manipulação Direta suportada por interações de toque (touch) e gestuais (swipe). Secundariamente, utilizar o estilo de Preenchimento de Formulários adaptado para interações curtas em dispositivos móveis.

- **Contextos de aplicação:** A manipulação direta será aplicada à tela principal da lista, permitindo que o usuário marque e desmarque itens interagindo de forma direta sobre o elemento visual que os representa. Os formulários serão acionados estritamente nos momentos de cadastro de texto (inserir/editar o nome ou a quantidade de um produto).

### 4.2 Justificativa de Seleção

Explica a escolha dos estilos de interação.

- **Critérios adotados (Base Teórica):** As escolhas técnicas foram determinadas pelas severas restrições motoras e cognitivas do ambiente de uso (supermercado, usuário empurrando o carrinho). O design justifica-se em dois princípios consagrados de IHC:

    - **Lei de Fitts e a Zona do Polegar:** O tempo para acessar um alvo depende do seu tamanho e da sua distância. Por ser utilizado com apenas uma mão, ações-chave devem utilizar alvos de toque grandes (pelo menos 48x48 dp) posicionados na zona de conforto e alcance do polegar na metade inferior da tela.
    
    - **Mapeamentos Naturais:** O uso de gestos para movimentar itens tira proveito de analogias físicas. Deslizar um item reproduz mentalmente a ação física de colocá-lo ou tirá-lo de um carrinho físico, tornando o modelo conceitual do sistema intuitivo.

- ((Alternativas consideradas:)) Foi cogitado o uso de botões "X" minúsculos ou menus flutuantes para a edição/exclusão de itens (estilo clássico de janelas e apontadores).

- **Trade-offs:** Houve a decisão consciente de ocultar menus tradicionais em favor de gestos ocultos, trocando a facilidade de aprendizado visual imediato (do menu) por maior limpeza na tela e muito mais velocidade na operação diária (através de aceleradores gestuais), focando o usuário apenas na execução das compras.

### 4.3 Aceleradores de Interação

Define mecanismos para aumento de eficiência.

- **Gestos:** A funcionalidade de Smart Uncheck baseia-se num acelerador gestual de deslize lateral (swipe) que permite restaurar instantaneamente um item comprado, sem obrigar o usuário a percorrer menus ou telas de configuração e sem acionar ações destrutivas irreversíveis.

- **Automação:** Ao iniciar o cadastro de um novo item, o sistema deve direcionar o foco visual automaticamente para a área de texto, exibindo imediatamente o teclado virtual do dispositivo (diminuindo o esforço do toque extra de preparação).

- **Atalhos de teclado:** Como a aplicação tem foco predominante no ambiente mobile, atalhos tradicionais de teclado são irrelevantes e secundários.

## 5. Elementos de Ação

### 5.1 Entrada de Dados

Define padrões para preenchimento de campos (formulários e inserção de produtos). 

- **Tipos de entrada:** Campos de texto simples para nome de produtos e entrada numérica para quantidades. O sistema deve acionar automaticamente o teclado virtual correspondente ao tipo de dado (ex: abrir teclado numérico direto para o campo de quantidade, poupando toques extras do usuário).

- **Validação:** O sistema deve aplicar a Lei de Postel (ser flexível e tolerante no que recebe do usuário). Por exemplo, o aplicativo deve aceitar espaços acidentais no início ou no fim das palavras e ignorar diferenças entre letras maiúsculas e minúsculas sem gerar erros.

- **Feedback:** A validação deve ocorrer em tempo real (inline). Se um erro for detectado (ex: tentar salvar um item sem nome), o item que contém o erro deve ser destacado e o erro deve ser descrito ao usuário em formato de texto claro, acompanhado de um ícone (mecanismo de Recuperação Apoiada). A cor não deve ser o único meio visual de comunicar o erro.

### 5.2 Seleção

Define padrões para escolha de opções (como marcar um item como "comprado"). 

- **Componentes utilizados:** Uso de checkboxes (caixas de seleção) para itens na lista, switches (interruptores) para opções rápidas de configuração e gestos de swipe para seleção/deseleção em massa.

- **Critérios de uso:** Para satisfazer a Lei de Fitts, as áreas de clique não devem se limitar ao desenho do componente. O target (alvo de toque) de um checkbox na lista de compras deve abranger toda a extensão horizontal da linha correspondente ao produto (etiqueta de texto combinada com a entrada). O alvo mínimo de toque de qualquer elemento de seleção deve ser rigorosamente de pelo menos 44x44 CSS pixels ou 48x48 dp.

### 5.3 Ativação

Define mecanismos de execução de ações e botões.

- **Tipos de botões:** A ação mais comum da tela primária (Adicionar Novo Produto) deve utilizar um Floating Action Button (FAB), posicionado no canto inferior direito para garantir acesso imediato pelo polegar do usuário.

- **Estados:** Os botões devem comunicar claramente seus estados visuais: Habilitado (Enabled), Pressionado (Pressed para botões tocados), Arrastado (Dragged para itens movidos via swipe) e Desabilitado (Disabled). Importante: Elementos no estado "Desabilitado" são uma exceção visual e não precisam cumprir os requisitos mínimos de contraste de cor, para que não pareçam interativos.

- **Feedback:** O retorno ao toque do usuário deve ser imediato (reação visual inferior a 50 milissegundos) para indicar que a ação foi registrada. A execução da ação no sistema subjacente deve obedecer ao Limiar de Doherty, mantendo o tempo de resposta em menos de 400 milissegundos, para garantir que o usuário não tenha que esperar e perca o fluxo de sua tarefa.

## 6. Vocabulário e Padrões

### 6.1 Terminologia

Define a linguagem adotada pelo sistema.

- **Termos principais:** "Item" (produto), "Pendente" (falta pegar), "Comprado" (ou "No Carrinho"), "Restaurar" e "Lista Ativa".

- **Definições:**
    - *Item:* O produto físico que será adquirido.
    
    - *Pendente:* Status padrão do item que ainda precisa ser encontrado no supermercado.
    
    - *Comprado:* Status do item que já foi colocado no carrinho físico pelo usuário.

- **Regras de uso:** O vocabulário deve mapear o mundo real do supermercado (Ex: usar "Comprado" em vez de termos orientados a sistema como "Concluído" ou "Registro atualizado"). Mensagens de erro de sincronização devem focar no status visível (ex: "Aguardando conexão...") em vez de expor arquiteturas complexas (ex: "Falha de POST na API").

**Regra geral:** O sistema deve se comunicar no idioma do usuário, utilizando palavras, expressões e conceitos naturais ao seu cotidiano . Jargões técnicos, termos orientados ao sistema ou nomenclatura de desenvolvedores devem ser estritamente evitados . O vocabulário escolhido deve ser padronizado em todas as telas, botões, mensagens de erro, menus e sistemas de ajuda . 

**Justificativa:** O uso de uma terminologia familiar e consistente constrói uma correspondência entre o sistema e o "mundo real" (o modelo mental do usuário) . Isso torna a comunicação eficiente, acelera o aprendizado e evita que o usuário tenha que parar para deduzir o significado das opções disponíveis . 

**Exemplo:** Utilizar termos concisos, diretos e familiares, como "Inserir Tabela", e manter a palavra "Gravar" (ou "Salvar") exatamente igual em todas as interações do aplicativo . 

**Contraexemplo:** Misturar os botões "Salvar" e "Gravar" para executar a mesma função em partes diferentes do sistema, ou apresentar uma mensagem de erro contendo códigos de programação indecifráveis para tentar explicar uma falha ao usuário .

---

### 6.2 Tipos de Tela

Define padrões estruturais recorrentes.

**Tipo 1: Tela Principal de Compras (Modo de Execução)**

- **Nome:** Lista Ativa (Painel Único).

- **Objetivo:** Permitir a visualização rápida, marcação de itens e adição de novos produtos enquanto o usuário caminha pelo supermercado empurrando o carrinho.

- **Estrutura**: Layout de painel único (single-pane), recomendado para telas compactas e uso focado. Barra flutuante de adição de itens na área inferior (ao alcance do polegar) e divisão gestáltica clara entre a lista de "Pendentes" (topo/centro) e a lista de "Comprados" (parte inferior).

- **Componentes:** Listas contínuas (Lists), Checkboxes com alvos de toque estendidos cobrindo toda a linha, separadores visuais subtis e um Botão de Ação Flutuante (FAB) para inserções rápidas.

### 6.3 Sequências de Interação

Define fluxos de interação.

- **Fluxo principal:** O "Modo de Compras" - O usuário abre o aplicativo, visualiza os itens pendentes, toca no item para marcá-lo como comprado (movendo-o para a área inferior).

- **Etapas (Restaurar Item via Smart Uncheck):**
    1. O usuário identifica um item marcado por engano na área de "Comprados".

    2. Com apenas o polegar, o usuário desliza o item lateralmente (swipe).

    3. O sistema retorna o item ao status "Pendente" e o reposiciona dinamicamente no topo da lista ativa, sem exigir rolagem (scroll) manual da tela.

- **Tratamento de erros (Falta de Rede):**
    - Caso a internet oscile ou falhe (como em zonas de sombra no subsolo do mercado), a interação do usuário **não deve ser bloqueada**.
    
    - O sistema adota um comportamento de Prevenção de Erros utilizando o armazenamento local (IndexedDB) para dar uma resposta imediata (< 1 segundo).
    
    - Uma indicação visual de "Sincronizando..." informará o estado da conexão, garantindo que o usuário tenha um feedback constante sem ser interrompido por pop-ups de erro.

## 7. Diretrizes Transversais de UI/UX

### 7.1 Consistência

As interfaces devem manter padrões visuais e comportamentais consistentes.

- Padrões Internos: A mesma terminologia (ex: "Pendente", "Comprado") e as mesmas cores semânticas devem ser usadas em todas as telas, botões e mensagens. O usuário nunca deve ter de se perguntar se palavras ou ações diferentes significam a mesma coisa.

- Mapeamento Natural: O design deve explorar os mapeamentos naturais, criando uma correspondência lógica entre as ações na interface e a tarefa física no mundo real (ex: deslizar um item mimetiza retirá-lo do carrinho).

### 7.2 Visibilidade do Estado do Sistema

O sistema deve manter o usuário informado sobre o que está acontecendo.

- Feedback Imediato: Cada ação do usuário deve ter um efeito visual óbvio e imediato. Para manter a fluidez e a produtividade, o tempo de resposta da interface ao marcar/desmarcar itens deve respeitar o Limiar de Doherty, ocorrendo em menos de 400 milissegundos.

- Estado da Rede (Offline-First): Como a aplicação opera em zonas de sombra (subsolos), o estado de sincronização de dados (Offline, Sincronizando, Sincronizado) deve estar sempre atualizado e facilmente perceptível, sem exigir que o usuário procure por essa informação.

### 7.3 Prevenção e Tratamento de Erros

O sistema deve minimizar erros e oferecer mecanismos claros de recuperação. 

- Design para o Erro: A interface deve assumir que erros serão cometidos (ex: toques acidentais em itens). O sistema deve minimizar as consequências, tornando as ações facilmente reversíveis através de recuperações apoiadas, como o gesto de "Desfazer" (Smart Uncheck), em vez de focar apenas em diálogos de confirmação excessivos.

- Lei de Postel (Princípio da Robustez): O sistema deve ser compreensivo e flexível com a entrada do usuário ("ser liberal no que aceita"). Por exemplo, espaços em branco acidentais ou erros de capitalização na digitação de novos produtos devem ser tolerados e tratados pelo sistema internamente.

### 7.4 Acessibilidade

A interface deve ser utilizável por usuários com diferentes capacidades

- Legibilidade e Redimensionamento: O texto deve ser legível e suportar o redimensionamento nativo do dispositivo em até 200% sem perda de conteúdo, funcionalidade ou quebra do layout direcional (exigência da WCAG 2.2).

- Independência da Cor: Para garantir o uso por pessoas com deficiência de visão de cores (ex: daltonismo), a cor nunca deve ser o único meio visual de comunicar informações críticas ou contrastes (como uma falha de conexão ou erro de preenchimento). Deve-se utilizar dicas secundárias, como ícones ou rótulos de texto.

### 7.5 Eficiência de Uso

O sistema deve permitir execução eficiente de tarefas frequentes.

- Aceleradores: Para usuários que dominam a ferramenta, o sistema deve fornecer aceleradores gestuais (como o swipe rápido) que acelerem o passo da interação sem prejudicar o aprendizado de usuários novatos.

- Carga Cognitiva e Lei de Hick: O tempo para tomar uma decisão aumenta com o número e a complexidade das opções. A interface deve limitar estritamente os controles em tela ao que é essencial para a tarefa de compra, agrupando funcionalidades acessórias e evitando a sobrecarga de escolhas.

## 8. Padrão de Especificação de Componentes

### 8.1 Componente: Item da Lista (Checkbox Estendido)

Regra: O alvo de toque (touch target) para marcar um produto como comprado ou pendente não deve se limitar ao ícone do checkbox. A área clicável deve abranger toda a extensão horizontal da linha do produto, com uma altura mínima rigorosa de 48x48 dp. 

Justificativa: Segundo a Lei de Fitts, aumentar o tamanho do alvo reduz o tempo de alcance e a taxa de erro. Como o usuário está empurrando o carrinho de supermercado e operando o celular com o polegar, um alvo minúsculo geraria enorme frustração. 

Exemplo: 
> (Adicionar Imagem: Um retângulo ilustrando a linha inteira do produto com um fundo leve indicando a área clicável total). 

Contraexemplo: 
> (Adicionar Imagem: Apenas o quadradinho do checkbox circulado, exigindo alta precisão de clique do usuário). 

Exceções: Botões de edição detalhada ou exclusão definitiva (ícone de lixeira) não devem fazer parte dessa grande área clicável primária, para evitar ações acidentais irreversíveis.

### 8.2 Componente: Gesto de Swipe (Smart Uncheck)

Regra: Para retornar um item da lista de "Comprados" para "Pendentes", o usuário deve deslizar (swipe) a linha do item lateralmente. O sistema deve fornecer retorno visual imediato do item se movendo, seguido do seu reposicionamento no topo da tela. 

Justificativa: Emprega o Princípio de Mapeamento Natural, associando a ação física de "tirar do carrinho" a um gesto de arrastar na tela. É também um mecanismo de Recuperação Apoiada e "Design para o Erro", permitindo desfazer um clique acidental de forma muito mais rápida do que procurar e clicar em um botão de "Desfazer" tradicional. 

Exemplo: 
(Adicionar Imagem: Uma linha de produto sendo arrastada para a esquerda, revelando um fundo com uma cor de ação e um ícone de retorno). 

Contraexemplo: 
(Adicionar Imagem: Um item comprado onde o usuário precisa dar um clique longo, abrir um menu suspenso e clicar em "Mover para Pendentes"). 

Exceções: O swipe não deve ser utilizado em formulários de texto (ex: inserção de nomes) para não conflitar com a navegação nativa do texto.

### 8.3 Componente: Feedback de Estado de Rede (Offline)

Regra: O estado da rede (Offline, Sincronizando, Atualizado) deve ser indicado por um ícone sutil, acompanhado opcionalmente de um texto curto na barra superior ou inferior. O aplicativo nunca deve interromper o fluxo do usuário com caixas de diálogo (pop-ups) para avisar que a internet caiu. 

Justificativa: O PWA é Offline-First. Utilizar mensagens modais intrusivas para problemas temporários de rede viola a autonomia do usuário e quebra seu fluxo de concentração (foco na compra), criando rupturas na comunicação. A indicação deve ser de Prevenção Passiva e silenciosa, mantendo a "visibilidade do estado do sistema" sem ser bloqueante. 

Exemplo: 
<!--Adicionar Imagem: Um pequeno ícone de nuvem cortada no topo da tela, enquanto a lista de itens continua totalmente visível e interativa.-->

Contraexemplo: 
<!--Adicionar Imagem: A tela escurecida por um pop-up gigante dizendo "Erro: Sem conexão com a internet. Tente novamente", impossibilitando o usuário de continuar marcando seus itens.-->

Exceções: Problemas graves de autenticação (ex: sessão expirada que exige re-login de segurança) podem usar diálogos modais para capturar o foco, pois impedem o uso seguro da ferramenta.

## Bibliografia

>- BARBOSA, Simone D. J. et al. Interação Humano-Computador e Experiência do Usuário. [S. l.]: Autopublicação, 2021.

>- GOOGLE. Material Design 3. [S. l.]: Google, [20--].

>- NORMAN, Donald A. O design do dia a dia. Tradução de Ana Deiró. 1. ed. Rio de Janeiro: Anfiteatro, 2018.

>- WORLD WIDE WEB CONSORTIUM (W3C). Web Content Accessibility Guidelines (WCAG) 2.2. [S. l.]: W3C, 12 dez. 2024. Disponível em: https://www.w3.org/TR/WCAG22/. Acesso em: 1 jun. 2026.

>- YABLONSKI, Jon. Leis da Psicologia Aplicadas a UX: Usando psicologia para projetar melhores produtos e serviços. Tradução de Cláudio José Adas. 1. ed. São Paulo: Novatec, 2020.

## Controle de Versão

|  |  |  |