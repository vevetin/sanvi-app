# Documento de Visão do Produto e Projeto de Software

## 1. Cenário Atual do Cliente e do Negócio

### 1.1 Introdução ao Negócio e Contexto

Este projeto está inserido no contexto de **aplicativos de produtividade e planejamento doméstico**, que atua no setor de **gestão de compras de supermercado e organização familiar**, com foco em **sincronização em tempo real e eficiência no ponto de venda**. Atualmente, a organização (mercado) apresenta os seguintes aspectos relevantes:

- Estrutura do mercado: O segmento caracteriza-se pela coexistência de utilitários minimalistas (focados em alta eficiência) e ecossistemas integrados complexos (que conectam planos alimentares e receitas ao carrinho de compras).
    
- Principais processos de negócio: Criação de listas, compartilhamento entre membros do mesmo domicílio, ida ao supermercado físico e marcação de itens comprados. A retenção de usuários é historicamente alta devido ao forte efeito de rede intra-familiar.
    
- Sistemas atualmente utilizados: Soluções líderes consolidadas como AnyList, OurGroceries, Bring!, e Listonic, além de alternativas mais holísticas como Cozi e Mealime.
  
- Limitações identificadas: A aparente estabilidade dos líderes de mercado esconde falhas graves de usabilidade física (ergonomia no supermercado) e arquiteturas de sincronização de dados bastante frágeis.
  
- Restrições (técnicas e ergonômicas): No ambiente do supermercado, o usuário frequentemente tem apenas uma mão livre (pois empurra o carrinho ou carrega cestas) e enfrenta problemas de conectividade em subsolos ou áreas de "sombra" de sinal de internet.

O projeto está alinhado aos seguintes objetivos estratégicos:

- **Objetivo estratégico 1:** Explorar as vulnerabilidades técnicas e de usabilidade dos concorrentes atuais para introduzir uma solução construída sob uma arquitetura moderna (Local-First).
  
- **Objetivo estratégico 2:** Reduzir o atrito operacional e cognitivo do usuário antes, durante e após a ida ao supermercado.


### 1.2 Identificação da Oportunidade ou Problema

O problema/oportunidade identificado pode ser descrito como:

- Problema principal: Os aplicativos atuais falham na sincronização de dados em ambientes offline, possuem péssima ergonomia para uso com apenas uma das mãos, ignoram a privacidade individual dentro de grupos familiares e exigem muito esforço manual para gestão da despensa.

- Evidências:
  - Perda de dados crônica devido ao uso de algoritmos simplórios de "Última Escrita Vence" (LWW) para sincronização, fazendo com que itens inseridos offline sejam apagados ou itens excluídos "ressuscitem" misteriosamente ao reconectar.

  - Relatos frequentes de usuários do AnyList perdendo itens no meio da jornada de compra e desligamento automático da tela (falta de wake lock) com as mãos ocupadas (ex: Mealie).

  - Frustrações ergonômicas severas, como a realocação da barra de itens no OurGroceries, que quebrou o fluxo de usuários que utilizam o app com apenas uma mão. 

- Impactos atuais:
  - Operacionais: Erros crônicos de abastecimento doméstico (compras duplicadas ou esquecimentos) e necessidade de reescrever dados manualmente na loja.

  - Experiência do usuário: Atrito no manuseio do smartphone com apenas uma mão, poluição visual por excesso de anúncios (ex: Bring! e Listonic) e quebra de privacidade devido ao acesso vitalício e irrestrito às listas da casa.

Caso não haja intervenção (no mercado):

- **Consequência 1:** Os usuários continuarão reféns de sistemas com falhas de concorrência e que geram perda de dados essenciais em momentos críticos de compra.

- **Consequência 2:** A falta de privacidade e a permanência vitalícia de convidados/visitantes nas listas continuarão gerando brechas de segurança de dados nas rotinas de abastecimento das famílias.

A oportunidade associada inclui:

- **Ganho de eficiência:** Adoção de infraestrutura Local-First com CRDTs (Tipos de Dados Replicados Livres de Conflito) para tempo de resposta zero e sincronização perfeita sem perda de dados offline.

- **Melhoria de experiência (Ergonomia):** Criação de mecânicas de usabilidade para uma mão só, como o "Smart Uncheck" (deslize lateral para retornar itens à lista), e a funcionalidade para manter a tela ativa durante a compra.

- **Automação:** Uso de Visão Computacional (OCR) e IA (como GPT-4) para ler cupons fiscais, atualizando o inventário da despensa automaticamente e prevendo ciclos de compras, já que cerca de 70% das compras são cíclicas.


### 1.3 Segmentação de Clientes

Os stakeholders e usuários do sistema são segmentados da seguinte forma:

| Segmento | Descrição | Necessidades | Influência |
| --- | ----------- | --- | --- |
| Famílias / Agregados | Membros que moram na mesma casa e dividem o orçamento e abastecimento do lar. | Sincronização offline-first infalível; divisão de itens em tempo real no mercado via geofencing; privacidade em listas pessoais separadas das familiares. | Alta |
| Repúblicas e Grupos | Estudantes ou grandes grupos que compram em conjunto de forma ágil.| Compartilhamento temporário de listas (via links seguros temporários em vez de acessos vitalícios); usabilidade rápida. | Média |
| Compradores Planejadores | Usuários focados em controle de gastos e histórico de compras. | Automação da despensa através de leitura de cupons fiscais por IA; alertas preditivos de quando itens cíclicos vão acabar. | Média |

Principais dores identificadas:

- Dor 1: Perda de itens recém-inseridos na lista ou "ressurreição" de itens já comprados devido a falhas na sincronização quando a internet oscila dentro do mercado.

- Dor 2: Dificuldade física de interagir com o layout do aplicativo utilizando apenas uma mão enquanto a outra empurra o carrinho ou segura cestas de supermercado, com a tela apagando constantemente.

- Dor 3: Falta de privacidade dentro da própria casa (não há como ter uma lista pessoal sem expor hábitos de consumo para o grupo inteiro) e impossibilidade de revogar acessos de visitantes antigos.

> *Nota de UX: A modelagem comportamental detalhada do perfil 'Comprador Planejador' e o mapeamento de suas limitações em contexto crítico (incluindo a Persona de design e o cenário de falha de conexão no subsolo) estão documentados na [Seção 2.1 do Guia de Estilo](guia-de-estilo.md#21-ambiente-de-uso).*

## 2. Solução Proposta

### 2.1 Objetivos do Produto

**Objetivo Geral:**  

O objetivo principal é oferecer uma experiência de uso livre de fricção física (ergonomia) no supermercado e uma infraestrutura de sincronização de dados à prova de falhas em ambientes offline.

**Objetivos Específicos:**

- Eliminar a perda e "ressurreição" de dados causadas por oscilações de rede (zonas de sombra em supermercados) utilizando sincronização assíncrona descentralizada.

- Reduzir a carga cognitiva e a dificuldade física de manipular o aplicativo com apenas uma mão durante as compras.

- Garantir divisões claras e privacidade (geolocalizada ou por pessoa) dentro de um ambiente colaborativo familiar.

### 2.2 Características da Solução

A solução proposta consiste em um sistema que:

- Sincronização Local-First: O dispositivo local detém a cópia primária dos dados, permitindo tempo de resposta zero na interface e funcionamento impecável sem internet.
    
- Ergonomia "Smart Uncheck": Possibilidade de usar um simples gesto de deslize lateral (swipe) com o polegar para retornar um item para o agrupamento ativo, posicionando-o dinamicamente no topo da tela.
    
- Prevenção de Fricção em Loja: Implementação de bloqueio de tela nativo (wake lock) para impedir que a tela do celular apague sozinha com as mãos do usuário ocupadas empurrando o carrinho.
    
- Gerenciamento de Privacidade e Convites Seguros: Compartilhamento de listas por tokens criptográficos com expiração automática (ex: validade de duas horas), impedindo que convidados ou ex-colegas mantenham acesso vitalício e observem os hábitos da casa.

**Fora do escopo:**

- Automação via Visão Computacional (OCR): Funcionalidades de leitura de cupons fiscais e parser com Inteligência Artificial para autoabastecimento da despensa (adiadas para iterações futuras).

- Módulos de Receitas e Planos Alimentares Completos: Evitaremos inchar o aplicativo com gerenciadores completos de receitas e planejamentos semanais complexos (diferenciando-se de Mealime e AnyList).

### 2.3 Tecnologias a Serem Utilizadas

| Camada | Tecnologia | Justificativa |
| --- | --- | --- |
| Frontend/PWA (Mobile-First) | React, TypeScript e Vite | O React permite o desenvolvimento de interfaces reativas e baseadas em componentes, ideais para lidar com as interações de tela em tempo real. O TypeScript adiciona segurança e tipagem estática, e o Vite garante alta velocidade e eficiência no ambiente de desenvolvimento e build. |
| Estilização e UI | Tailwind CSS e shadcn/ui | Facilitam a implementação rigorosa da abordagem Mobile-First. Permitem a construção rápida de uma interface ergonômica, limpa e responsiva, essencial para o uso confortável do aplicativo com apenas uma mão (o polegar) enquanto o usuário empurra o carrinho no supermercado. |
| Roteamento | Roteamento | Gerencia a navegação da aplicação de forma fluida (no modelo SPA), melhorando a experiência de uso e garantindo a transição rápida entre as listas familiares e pessoais. |
| Persistência Local (Offline-First) | Dexie.js | Atua como uma interface robusta e minimalista para o **IndexedDB** do navegador. É a peça-chave para garantir que o PWA funcione com tempo de resposta zero (< 1 segundo), persistindo dados localmente de maneira à prova de falhas em zonas de "sombra" de internet nos subsolos dos supermercados. |
| PWA & Service Workers | vite-plugin-pwa | Responsável por transformar a aplicação web em um Progressive Web App instalável em iOS e Android, sem a fricção das App Stores. Gerencia de forma eficiente o cache por meio de Service Workers, garantindo o funcionamento do aplicativo no modo offline. |
| Backend (API de Sincronização) | Laravel (PHP) | Framework moderno, estruturado e robusto do ecossistema PHP, ideal para desenvolver de forma ágil as APIs REST responsáveis por processar a sincronização dos dados quando a conexão do usuário for restabelecida, além de gerenciar a lógica de tokens criptográficos temporários. |
| Banco de Dados (Nuvem) | PostgreSQL | Banco de dados relacional de altíssima confiabilidade e suporte consolidado no ecossistema PHP. Servirá como armazenamento primário para a "verdade global" da aplicação, guardando dados consolidados das famílias, usuários e permissões de listas. |

**Restrições tecnológicas:**

- **Limitações de Hardware via Navegador:** Por ser um PWA, o acesso nativo a certas APIs profundas do sistema operacional pode ser limitado. A funcionalidade de manter a tela sempre ativa (wake lock) para que o aparelho não desligue sozinho dependerá do suporte da API Screen Wake Lock pelos navegadores móveis (Safari e Chrome) no momento da compra.

- **Sincronização em Background no iOS:** O suporte da Apple para Background Sync em PWAs possui um histórico restritivo. Será exigido um tratamento cuidadoso entre a API em Laravel e o frontend em React/Dexie.js para garantir que os dados offline locais sejam enviados para a nuvem no momento exato em que a aplicação for reaberta ou trazida para o primeiro plano.

---

### 2.4 Pesquisa de Mercado e Análise Competitiva

| Solução         | Pontos Fortes | Pontos Fracos |
| --------------- | ------------- | ------------- |
| AnyList | Forte no segmento doméstico, importação automática de receitas e robustez do ecossistema de voz. | Perda em tempo real de itens inseridos no ponto de venda e miscategorização frequente. |
| OurGroceries | Experiência minimalista, alta velocidade para listas puras e plataforma universal (incluindo Apple Watch). | Regressões ergonômicas (realocação de barra para extremidade inferior), quebra de fluxo de uso com uma mão e ausência de privacidade individual. |
| Bring! | Abordagem visual disruptiva por blocos que facilita encontrar rápido os produtos na loja. | Alta poluição visual por publicidade de marcas (FMCG) e baixa legibilidade textual. |

**Diferenciais da solução proposta:**

- Acessibilidade Imediata via PWA: Diferente dos concorrentes que exigem downloads de mais de 50MB, o sistema será acessado via link e instalado instantaneamente na tela inicial do usuário, eliminando a barreira de entrada das App Stores.

- Ergonomia Antropomórfica (Mobile-First): Layout rigorosamente projetado para o uso com apenas uma mão (o polegar), considerando que o usuário estará empurrando o carrinho ou segurando cestas físicas no supermercado. 

  > *A justificativa teórica para essas decisões de interface (como o recurso Smart Uncheck), fundamentada em princípios de IHC como a Lei de Fitts e os Mapeamentos Naturais, encontra-se formalizada na [Seção 4.2 do Guia de Estilo](guia-de-estilo.md#42-justificativa-de-seleção).*

- Resiliência Offline-First: Enquanto líderes de mercado perdem dados por usar o algoritmo simplório de "Última Escrita Vence" (LWW), a nossa solução usará o IndexedDB no navegador para tempo de resposta zero e sincronização robusta em background via API PHP assim que o sinal de internet retornar das "zonas de sombra" do mercado.

### 2.5 Análise de Viabilidade

**Viabilidade Técnica:**
 Média. A adoção de PWA simplifica enormemente o desenvolvimento de interfaces multiplataforma utilizando web technologies. No entanto, construir um mecanismo de sincronização assíncrona robusto à prova de conflitos (para substituir as falhas de rede em supermercados) utilizando PHP no backend e IndexedDB no frontend exige uma arquitetura de dados e fila de requisições cuidadosamente planejada.

**Viabilidade Econômica:**
Alta. O ecossistema PHP (como Laravel ou Symfony) e bancos de dados relacionais open-source (MySQL/PostgreSQL) possuem custos de hospedagem extremamente competitivos e farta disponibilidade de mão de obra qualificada. A exclusão momentânea de IA (leitura OCR de cupons) reduz os custos de APIs terceirizadas a praticamente zero nesta fase inicial. Além disso, por ser PWA, o projeto isenta o negócio das taxas de 30% das lojas de aplicativos (Apple/Google). 

**Viabilidade Organizacional:**
Alta. O foco exclusivo em resolver o problema crônico de famílias no supermercado delimita o escopo de forma clara, garantindo que a equipe não se disperse construindo funcionalidades secundárias que os usuários não valorizam.

**Riscos Identificados:**

- **Risco 1:** Limitações de Background Sync no iOS (Safari), que podem atrasar o envio de dados locais para o servidor PHP quando a tela for minimizada.
  - Mitigação: Implementar Service Workers de forma conservadora e criar indicadores visuais claros de "Sincronizando..." e "Offline" na interface, forçando a sincronização no momento em que o PWA for trazido para o primeiro plano.

- **Risco 2:** Impossibilidade de manter a tela ligada o tempo todo (Wake Lock) em navegadores antigos, fazendo a tela apagar enquanto o usuário empurra o carrinho.
  - Mitigação: Utilizar a Screen Wake Lock API oficial do HTML5 onde houver suporte e, em dispositivos sem suporte, fornecer um aviso instrutivo de como aumentar o tempo de bloqueio de tela nas configurações do celular.

### 2.6 Impacto da Solução

- Impacto operacional: As famílias terão uma ferramenta leve, que não requer atualizações constantes nas lojas de aplicativos, e que elimina o retrabalho de itens perdidos e duplicados durante a oscilação de sinal de celular dentro do supermercado.

- Impacto organizacional: A empresa estabelece seu produto como uma referência em eficiência e baixo custo de operação (devido à infraestrutura PHP enxuta), criando uma base fiel de usuários através de alta confiabilidade técnica.

- Impacto no usuário: Redução drástica do estresse durante as compras semanais. O foco mobile-first respeita as restrições físicas do ambiente (iluminação do mercado, uso de apenas uma mão, carrinho pesado), permitindo que a jornada seja feita com conforto e rapidez.

## 3. Estratégias de Engenharia de Software

### 3.1 Estratégia Priorizada

- Abordagem: Ágil. A filosofia ágil prioriza a flexibilidade, a colaboração e a resposta rápida a mudanças em vez de seguir um plano rígido ou focar em documentação exaustiva.

- Ciclo de vida: Ágil. Aproveita os aspectos iterativos e incrementais para refinar e entregar partes funcionais do aplicativo com frequência. Isso permitirá que o PWA seja testado de forma contínua no ambiente real (o supermercado), garantindo feedback antecipado.

- Processo de Desenvolvimento de Software: eXtreme Programming (XP) adaptado + Rapid Application Development (RAD).
  - O XP adaptado trará o foco rigoroso na qualidade do código através do Desenvolvimento Orientado a Testes (TDD) e refatoração contínua, essenciais para garantir a robustez da complexa arquitetura Offline-First (IndexedDB + PHP).
    
  - O RAD será aplicado para a validação ergonômica, utilizando prototipação rápida e evolutiva para testar diretamente no celular (Mobile-First) as interações físicas, como o Smart Uncheck para uso com apenas uma mão, produzindo documentação apenas quando estritamente necessária.

- Framework de Gerenciamento: Kanban. Foca na visualização do fluxo contínuo de trabalho e na limitação do trabalho em progresso (WIP), com entregas sob demanda e sem a necessidade de iterações de tempo fixo (como as Sprints do Scrum).

### 3.2 Justificativa

A estratégia foi escolhida com base no princípio da Adaptabilidade Contextual, analisando as particularidades do projeto, o domínio e o desenvolvedor:
  - Tamanho e Organização da Equipe (Fator Humano): Por se tratar de um projeto pessoal de um desenvolvedor solo, a adoção de metodologias pesadas ou frameworks baseados em papéis e cerimônias temporais fixas geraria burocracia desnecessária. O Kanban se adapta perfeitamente por ser um framework ágil focado na otimização contínua do processo sem prescrever papéis fixos.
    
  - Gestão de Fluxo e Foco: A limitação do Trabalho em Progresso (WIP) inerente ao Kanban garantirá que não haja diluição de foco. Isso forçará a conclusão de uma funcionalidade complexa (como a persistência offline via IndexedDB) antes do início da próxima (como o layout mobile-first).
    
  - Estabilidade dos Requisitos e Risco Técnico: Em sistemas voltados a ambientes de mudanças e descobertas (como testar o comportamento do celular em zonas de sombra no supermercado), os processos de especificação, projeto e implementação devem ser intercalados. Um ciclo de vida ágil permite falhar rápido e ajustar a rota caso as restrições do navegador limitem alguma funcionalidade do PWA.
    
  - Cultura de Entrega Contínua: O ciclo de vida ágil permite que incrementos do aplicativo sejam liberados mais cedo. Como o PWA não depende da aprovação de lojas de aplicativos, as atualizações poderão ser implantadas e testadas imediatamente, proporcionando um ciclo de feedback de usabilidade extremamente curto.


## 4. Engenharia de Requisitos

### 4.1 Atividades e Técnicas

A disciplina de ER será executada de forma iterativa e entrelaçada, utilizando as seguintes técnicas adaptadas para o escopo individual:

| Atividade  | Técnica     | Descrição   |
| ---------- | ----------- | ----------- |
| Elicitação e Descoberta | Análise de Concorrentes e Prototipação | Uso da pesquisa de mercado já realizada e criação rápida de protótipos de tela para descobrir necessidades ergonômicas físicas no mercado. |
| Análise e Consenso | User Story Mapping (USM)| O USM será usado para estruturar visualmente o Product Backlog como uma jornada ("chegar no mercado", "marcar itens", "ficar sem internet"), permitindo ver o quadro geral do aplicativo e focar no que agrega mais valor. |
| Declaração  | Histórias de Usuário + BDD | Os requisitos serão declarados como Histórias de Usuário (focando no valor). Os critérios de aceitação usarão BDD (Behavior-Driven Development: Dado-Quando-Então) para mapear os problemas de sincronização e viabilizar a automação de testes do XP. |
| Representação | Protótipos de Interface (Alta Fidelidade) | Em vez de UML complexa, a representação usará protótipos (Mockups) com abordagem Mobile-first. Isso é essencial (via RAD) para visualizar os gestos antropomórficos, como o "Smart Uncheck" por deslize lateral. |
| Verificação e Validação | Teste de Aceitação e Definições de Pronto | Validação das Histórias de Usuário através de testes automatizados e critérios INVEST (Independentes, Negociáveis, Valiosas, Estimáveis, Pequenas e Testáveis). Uso claro de Definition of Ready (DoR) e Definition of Done (DoD). |
| Organização e Atualização | Product Backlog via Kanban | Os requisitos serão mantidos em um fluxo contínuo no quadro Kanban, sofrendo refinamento progressivo (just-in-time) à medida que se aproximam do desenvolvimento para não desperdiçar esforço. |

### 4.2 Mapeamento ao Processo

| Atividade  | Artefato                | Responsável | Fase   |
| ---------- | ----------------------- | ----------- | ------ |
| Elicitação e Análise | Mapa de Histórias de Usuário (USM) | Dev Solo (PO)| Inicial (Visão) e Refinamento Contínuo. |
| Representação | Telas e fluxos de navegação (Mockups) | Dev Solo | Antes de iniciar a implementação do PWA na interface. |
| Declaração | Cartões de Histórias de Usuário com cenários BDD | Dev Solo | Contínua (movidas no Quadro Kanban). |
| Verificação | Testes Automatizados (TDD/BDD) passando com sucesso | Dev Solo | Durante e após a codificação de cada incremento. |
| Organização | Product Backlog priorizado e refinado | Dev Solo (PO) | Rotina semanal de gestão do Kanban. |


## 5. Cronograma e Entregas

| Marco     | Descrição   | Data (Estimativa Relativa)   |
| --- | --- | --- |
| Marco 1: Fundação Técnica e Validação Ergonômica |Configuração do backend em PHP (ex: Laravel/Symfony) com banco de dados relacional. Criação da estrutura base do PWA Mobile-First. Prototipação rápida (RAD) validando exclusivamente a usabilidade física (layout para uso com uma mão) e instalação do Service Worker. | Quinzena 1 |
| Marco 2: Sincronização Local-First (Núcleo Crítico) | Implementação do IndexedDB no navegador do aparelho e criação da lógica de sincronização assíncrona com o servidor PHP, estabelecendo os pilares para resolver as perdas de dados em zonas de sombra do supermercado. | Quinzena 2 |
| Marco 3: MVP - Produto Mínimo Viável (Listas Compartilhadas) | Lançamento da primeira versão utilizável para a família. Inclui o "Smart Uncheck" (deslize lateral) e a geração de convites temporários seguros. A partir deste marco, o PWA já deve ser testado no ambiente real do supermercado. | Quinzena 3 |
| Marco 4: Incrementos de Coordenação | Aperfeiçoamento do aplicativo com base no feedback real. Inclusão de categorização avançada, histórico básico de compras e experimentações com geofencing simples (exibir listas específicas por loja). | Quinzena 4+ |

Premissas:

- Tempo Parcial do Desenvolvedor Solo: Por se tratar de um projeto pessoal, o volume de horas dedicadas por semana pode flutuar, tornando o cronograma dinâmico e focado em concluir a tarefa atual antes de iniciar a próxima (princípio do Kanban).

- Escopo Variável: Os recursos planejados para o Marco 4 podem ser alterados, substituídos ou descartados com base nos testes físicos de ergonomia e falhas de rede descobertas durante o uso do MVP (Marco 3) nos supermercados reais.

## 6. Requisitos de Software

### 6.1 Requisitos Funcionais (RFs)

| ID   | Nome | Descrição   | Prioridade |
| ---- | --- | --- | --- |
| RF01 | Criar lista de compras | O sistema deve permitir a criação imediata de novas listas de compras. | Alta (7) |
| RF02 | Manter itens da lista de compras | O sistema deve permitir que o usuário adicione, edite e remova itens da lista de compras ativa. | Alta (8) |
| RF03 | Compartilhar lista de compras | O sistema deve permitir a geração de links para que múltiplos usuários acessem e editem uma lista simultaneamente. | Média (6) |
| RF04 | Marcar item como comprado | O  sistema deve permitir a alteração do status de um item de "pendente" para "comprado", removendo-o da visão principal. | Alta (8) |
| RF05 | Restaurar item comprado | O sistema deve permitir que o usuário retorne um item do status "comprado" para "pendente" na lista ativa. |  Alta (8) |
| RF06 | Segregar listas privadas | O sistema deve oferecer a capacidade de criar e manter listas visíveis exclusivamente para o usuário criador, independentemente de haver outras listas compartilhadas no mesmo dispositivo. | Baixa (4) |
| RF07 | Categorizar itens da lista | O sistema deve permitir o agrupamento de itens por categorias ou departamentos do supermercado. | Média (6) |
| RF08 | Consultar histórico de compras | O sistema deve manter e exibir um registro das listas e itens comprados em interações anteriores. | Baixa (3) |
| RF09 | Sugerir itens recorrentes | O sistema deve analisar o histórico e sugerir produtos frequentemente adicionados durante a criação de uma nova lista. | Média (5) |

### 6.2 Requisitos Não Funcionais (RNFs)

| ID    | Categoria  | Descrição   | Métrica   |
| ----- | ---------- | ----------- | --------- |
| RNF01 | Usabilidade | A interface do PWA deve ser estritamente mobile-first, projetada para garantir que as ações de marcação de status (comprado/pendente) possam ser realizadas confortavelmente com apenas uma mão (ex: usando o polegar via gesto de swipe lateral). *(A fundamentação conceitual em UI/UX para esta restrição de layout encontra-se no Guia de Estilo).* | 100% das ações de alteração de status de itens executáveis com uma única mão. |
| RNF02 | Usabilidade | Ao restaurar um item para o status pendente, o sistema deve realocá-lo dinamicamente no topo absoluto da tela visual do agrupamento ativo. | Realocação em tela sem exigir que o usuário faça scroll manual. |
| RNF03 | Reliability (Confiabilidade) | O sistema deve garantir a integridade da lista em cenários de edição simultânea offline, utilizando resolução de conflitos (ex: CRDTs ou Relógios Vetoriais) para evitar perda de dados ou "ressurreição" de itens após reconexão. | 0% de perda de dados ou conflitos não resolvidos após saída de zonas de sombra de rede. |
| RNF04 | Reliability (Confiabilidade) | O PWA deve implementar a Screen Wake Lock API (quando suportada) para impedir o desligamento automático da tela enquanto o usuário visualiza a lista durante as compras. | A tela não deve bloquear automaticamente enquanto o app estiver em primeiro plano (quando API for suportada). |
| RNF05 | Performance (Desempenho) | O sistema deve adotar arquitetura Offline-first, lendo e escrevendo primeiramente no banco local (IndexedDB) para garantir resposta instantânea na interface, independentemente do status da conexão de internet no supermercado. | Tempo de resposta para adicionar, alterar ou remover itens na interface < 1 segundo (mesmo offline). |
| RNF06 | Supportability (Suportabilidade) | O sistema deve suportar as diretrizes modernas de Progressive Web Apps (PWA), permitindo instalação direta na tela inicial a partir dos navegadores móveis líderes de mercado. | Total compatibilidade de instalação (Service Workers) no Google Chrome (Android) e Safari (iOS). |
| RNF07 | + (Restrição de Implementação) | O backend do sistema responsável por sincronizar as mudanças quando o usuário estiver online deve ser construído no ecossistema PHP (ex: Laravel, Symfony) apoiado por um banco de dados relacional. | Uso do ecossistema PHP e persistência principal em MySQL ou PostgreSQL. |
| RNF08 | + (Restrição de Design/Segurança) | A geração de links para compartilhamento temporário deve ser construída baseada em tokens criptográficos que expiram de forma automatizada. | O token do link de convite perde a validade em, no máximo, 6 horas após a geração. |

### 6.3 Regras de Negócio (RNs)

| ID | Nome | Descrição | Tipo de Regra |
| --- | --- | --- | --- |
| RN01 | Acesso sem Cadastro (Anonimato) | A criação, edição e uso da primeira lista de compras não deve exigir nenhum tipo de cadastro ou login obrigatório (e-mail, senha, etc.) por parte do usuário. | Regra de Restrição de Operação: Especifica uma condição que deve ser verdadeira para que a operação (usar o app) seja executada corretamente. |
| RN02 | Validade de Convites Temporários | Se o link de compartilhamento gerado para um visitante ultrapassar o limite de 6 (seis) horas desde a sua criação, então o acesso desse visitante à lista da família deve ser revogado automaticamente. | Regra de Causa e Efeito: Especifica que dada uma condição verdadeira (tempo > 6h), um novo evento (revogar token) é disparado. |
| RN03 | Visibilidade de Listas Privadas | Um usuário membro de um domicílio/grupo familiar apenas poderá visualizar e editar as listas de compras que não forem compartilhadas globalmente se ele próprio for o autor/criador da mesma. | Regra de Restrição de Estrutura: Especifica políticas ou condições sobre os relacionamentos entre usuários e listas que não podem ser violadas. |


## 7. Backlog do Produto

### 7.1 Backlog Geral

| ID   | Tipo       | Descrição   |
| ---- | ---------- | ----------- |
| US01 | User Story | Eu, como organizador da família, quero criar uma lista de compras imediatamente ao abrir o PWA de forma anônima, para iniciar minhas anotações instantaneamente sem a fricção de um cadastro prévio. `[RF01]` `[RN01]` |
| US02.1 | User Story | Eu, como usuário, quero adicionar um novo item à minha lista ativa, para não esquecer de comprá-lo quando for ao supermercado. `[RF02]` |
| US02.2 | User Story | Eu, como usuário, quero visualizar a lista de itens adicionados, para saber exatamente o que falta na minha despensa. `[RF02]` |
| US02.3 | User Story | Eu, como usuário, quero editar o nome, quantidade ou detalhes de um item existente, para corrigir erros de digitação ou ajustar minha necessidade de compra. `[RF02]` |
| US02.4 | User Story | Eu, como usuário, quero excluir permanentemente um item da lista, para remover produtos que desisti de comprar e manter a lista limpa. `[RF02]` |
| US03 | User Story | Eu, como dono da lista, quero gerar um link de convite temporário (expiração em 6h) para compartilhar com membros da casa ou visitantes, para evitar que ex-convidados mantenham acesso irrestrito às compras da residência. `[RF03]` `[RN02]` `[RNF08]` |
| US04 | User Story | Eu, como comprador em loja, quero marcar um item como "comprado", para que ele saia da minha visão principal e eu foque apenas nos itens que faltam coletar. `[RF04]` |
| US05 | User Story | Eu, como comprador empurrando um carrinho (uma mão ocupada), quero restaurar um item comprado deslizando-o lateralmente (swipe), para que ele retorne ao estado "pendente" e seja injetado instantaneamente no topo absoluto da lista, sem exigir que eu faça rolagem de tela (scroll). `[RF05]` `[RNF01]` `[RNF02]` |
| US06 | User Story | Eu, como usuário em zona de sombra do supermercado (sem rede), quero que todas as minhas edições reajam em menos de 1 segundo de forma offline, para que o aplicativo não trave enquanto tento marcar meus produtos. `[RNF03]` `[RNF05]` |
| FEAT01 | Feature | Capacidade de criar e manter listas totalmente privadas e segregadas (apenas para o usuário criador), mesmo participando de um domicílio compartilhado. `[RF06]` `[RN03]` |
| FEAT02 | Feature | Capacidade de agrupar itens por departamento da loja física e consultar todo o histórico das listas de compras passadas. `[RF07]` `[RF08]` |
| FEAT03 | Feature | Motor de predição e sugestão automática de itens recorrentes baseado na ciclicidade de compras (ex: lembrar do "Leite" toda sexta-feira). `[RF09]` |

### 7.2 Priorização

A priorização do Backlog é a atividade central para congelar o conteúdo das iterações e será realizada aplicando a técnica COORG (Classificar, Ordenar e Organizar) do método PBB, baseando-se em três vetores fundamentais:
  - Frequência de Uso e Valor de Negócio: Através de uma fórmula de soma (Prioridade = Frequência de Uso + Valor de Negócio), os itens foram classificados de cima para baixo. Ações realizadas dezenas de vezes por ida ao supermercado, como adicionar e marcar itens (US02, US04), garantem pontuação e prioridade máxima, superando amplamente as Features de histórico.
    
  - Conhecimento, Incerteza e Risco: Sob o ponto de vista da viabilidade técnica, o projeto adota uma abordagem orientada à mitigação de risco. A infraestrutura base para o Offline-first (US06) concentra o maior risco arquitetural do aplicativo. Ao ser tracionada e priorizada logo para os ciclos iniciais, força-se a validação imediata da técnica ou seu fracasso precoce (fail fast), possibilitando alterar a rota arquitetural antes que seja caro demais fazê-lo.
    
  - Capacidade de Entrega (Esforço): Histórias testáveis, de esforço menor e completamente independentes foram dispostas no topo para permitir que o ciclo de vida ágil produza incrementos funcionais com rapidez. Isso alimenta a necessidade de realizar ensaios ergonômicos físicos (no próprio supermercado) o quanto antes.

### 7.3 MVP

A adoção do User Story Mapping (USM) determinou o fatiamento do projeto em Release Slices (Fatias de Lançamento). A primeira fatia define perfeitamente o MVP, contemplando o conjunto mínimo de tarefas que permite aos usuários atingirem seus objetivos e experimentarem a promessa central do projeto (sincronização offline e usabilidade ergonômica).
Funcionalidades acessórias e categorização inteligente (Épicos/Features futuras) foram deliberadamente descartadas desta etapa para focar na viabilidade técnica primária.
O MVP inclui as seguintes funcionalidades e suas respectivas Histórias de Usuário:

- Fundação Offline-First (Resiliência de Rede):
  - US06: Reatividade instantânea (< 1 segundo) no aparelho de forma offline via IndexedDB.
    
- Operação Básica Instantânea (Acesso e CRUD):
  - US01: Criar a primeira lista de compras imediatamente e de forma anônima (sem cadastro).
  - US02a: Adicionar um novo item à lista ativa.
  - US02b: Visualizar a lista de itens pendentes na despensa.
  - US02c: Editar o nome ou quantidade de um item existente.
  - US02d: Excluir permanentemente um item da lista.
  - US04: Marcar um item como "comprado" no mercado (retirando-o da visão principal).
    
- Ergonomia Avançada em Loja (Design Mobile-First):
  - US05: Restaurar um item comprado deslizando-o lateralmente (Smart Uncheck), reposicionando-o automaticamente no topo da lista sem exigir rolagem (scroll).
    
- Distribuição e Segurança:
  - US03: Gerar link de convite familiar protegido por tokens com expiração automática ativada (6 horas).

### 7.4 Critérios de Aceitação do MVP

Os critérios de aceitação do MVP foram escritos utilizando o formato Behavior-Driven Development (BDD), servindo tanto como documentação de requisitos quanto como roteiro claro para a futura automação de testes do PWA.

- US01: Criar lista anonimamente
  - Cenário: Acesso imediato sem fricção.  
  - Dado que o usuário acessa o PWA pela primeira vez e não possui nenhum tipo de cadastro ativo,  
  - Quando ele clica no botão de criar "Nova Lista",  
  - Então o sistema deve criar e exibir uma lista ativa imediatamente, sem solicitar nenhum dado pessoal ou credencial de login.

- US02a: Adicionar item (Create)
    - Cenário: Inserção de novo produto na despensa.
    - Dado que o usuário possui uma lista de compras ativa aberta na tela,
    - Quando ele digita o nome de um produto (ex: "Leite") e aciona o comando de confirmação,
    - Então o item "Leite" deve ser salvo e adicionado imediatamente à visão principal da lista com o status de "pendente".

- US02b: Visualizar lista (Read)
  - Cenário: Conferência dos itens pendentes.
  - Dado que a lista de compras do usuário possui itens previamente adicionados que ainda não foram comprados,
  - Quando o usuário abre o aplicativo,
  - Então o sistema deve carregar e exibir claramente todos os itens com status "pendente", organizados para facilitar a leitura rápida.

- US02c: Editar item (Update)
  - Cenário: Correção de nome ou quantidade.
  - Dado que o item "Maçã" consta na lista ativa com a quantidade "1",
  - Quando o usuário edita a quantidade desse item para "5",
  - Então o sistema deve atualizar imediatamente a exibição e salvar a nova quantidade (5) para o respectivo produto.

- US02d: Excluir item permanentemente (Delete)
  - Cenário: Limpeza de itens indesejados.
  - Dado que o item "Biscoito" foi inserido por engano na lista ativa,
  - Quando o usuário aciona a opção de exclusão definitiva para este item,
  - Então o sistema deve apagar o "Biscoito" da base de dados e removê-lo completamente de qualquer exibição na interface.

- US04: Marcar item como comprado
  - Cenário: Ocultação de itens já coletados na loja.
  - Dado que o usuário está caminhando pelo supermercado e visualizando a lista com o item "Arroz" pendente,
  - Quando ele tocar/marcar o item "Arroz",
  - Então o sistema deve alterar o status do item para "comprado" e removê-lo da visão principal de itens pendentes (agrupando-o em uma seção secundária ou inferior).

- US05: Ergonomia via "Smart Uncheck"
  - Cenário: Retorno ágil de item comprado usando apenas uma mão.
  - Dado que o item "Detergente" foi acidentalmente marcado como "comprado" e encontra-se na seção inferior da tela,
  - Quando o usuário deslizar o dedo lateralmente (swipe) sobre a linha do "Detergente",
  - Então o sistema deve mudar o status do item de volta para "pendente" e injetá-lo dinamicamente no absoluto topo da parte visível da lista, não exigindo rolagem de tela (scroll) por parte do usuário.

- US06: Reatividade Offline (Local-First)
  - Cenário: Operação contínua em área de sombra de internet (subsolo do supermercado).
  - Dado que o aparelho celular do usuário perdeu totalmente a conexão com a rede,
  - Quando o usuário executar qualquer ação de edição (adicionar, marcar ou restaurar um item),
  - Então o PWA deve salvar a modificação no banco de dados local (IndexedDB) e refletir a mudança visual na interface em menos de 1 segundo, sem exibir erros de conexão ou travamentos de carregamento.

- US03: Distribuição e Segurança de Convites
  - Cenário: Revogação automática de token por limite de tempo.
  - Dado que o organizador da família gerou um link protegido por token criptográfico e o compartilhou há mais de 6 (seis) horas,
  - Quando um visitante tentar acessar a lista utilizando esse mesmo link,
  - Então a API de segurança do backend em PHP deverá revogar o pedido, bloquear a visualização da lista e exibir uma mensagem padronizada de "Acesso Negado: Este convite expirou".

## 8. Processo de Validação

### 8.1 Definition of Ready (DoR)

Um item do backlog (História de Usuário) é considerado "Pronto" (Ready) para ser puxado para a coluna de desenvolvimento no Kanban quando as seguintes verificações forem validadas:

  - Clareza e Formato: O requisito está devidamente representado no formato de História de Usuário (Eu, como [papel] quero [ação] para [valor])?

  - Critérios de Aceitação: O requisito está coberto por critérios de aceitação claros e mapeados em cenários BDD (Dado-Quando-Então)?
  
  - Validação Visual (RAD): O requisito está mapeado para uma interface ou possui um protótipo visual/Mockup validando a ergonomia mobile-first (quando necessário)?
  
  - Independência e Dependências: As dependências técnicas deste requisito (ex: precisar que o banco de dados PHP já esteja configurado) estão identificadas e resolvidas?
  
  - Tamanho: A história é pequena o suficiente para ser concluída em um ciclo curto de esforço (cabendo no limite de Trabalho em Progresso - WIP)?

### 8.2 Definition of Done (DoD)

Um item é considerado totalmente "Concluído" (Done) e pronto para uso real quando:

  - Entrega de Incremento: A história entrega um incremento de produto funcional e utilizável.
 
  - Cobertura de Testes (XP/TDD): O código foi desenvolvido utilizando a prática de TDD, os testes automatizados foram escritos com base nos cenários BDD e 100% deles estão passando sem erros.
 
  - Critérios de Aceitação Atendidos: O comportamento na interface do PWA contempla com sucesso todos os critérios de aceitação estabelecidos (BDD) para a história.
 
  - Padrões de Codificação: O código fonte (PHP, React/Web, etc.) está aderente aos padrões de codificação definidos, foi refatorado para garantir simplicidade e não contém "gambiarras" provisórias.
 
  - Índices de Performance e Qualidade (RNFs): A implementação da história não quebrou o funcionamento Offline-first, mantendo os índices de performance do produto (como tempo de resposta da interface na adição de itens < 1 segundo).
 
  - Persistência e Sincronização: As modificações feitas interagem corretamente com o IndexedDB local e não causam conflitos com os CRDTs no envio para a API em PHP.

## Bibliografia

>- COHN, Mike. User Stories Applied: For Agile Software Development. Boston: Addison-Wesley Professional, 2004.

>- GOOGLE. Análise de Mercado: Sistemas de Lista de Compras Compartilhada em Tempo Real. Resposta gerada por inteligência artificial (modelo Gemini, funcionalidade Deep Research). O prompt completo utilizado encontra-se no [Apêndice A](./visao-do-produto-e-projeto.md#apêndice-a) deste documento. Mountain View: Google, 2026. Documento em formato eletrônico (PDF).

>- IEEE COMPUTER SOCIETY. Guide to the Software Engineering Body of Knowledge (SWEBOK Guide). Versão 4.0. Los Alamitos: IEEE Computer Society, 2024.

>- PATTON, Jeff. User Story Mapping: Discover the Whole Story, Build the Right Product. 1. ed. Sebastopol: O'Reilly Media, 2014.

>- SCHWABER, Ken; SUTHERLAND, Jeff. The Scrum Guide. [S. l.]: Scrum.org, 2020.

## Apêndice A

```text
Atue como um Especialista Sênior em Pesquisa de Mercado e Estrategista de Produtos Digitais.

Sua tarefa é realizar uma análise de mercado abrangente e aprofundada sobre sistemas (aplicativos, sites e PWAs) de "Lista de Compras Compartilhada em Tempo Real".

Antes de fornecer a resposta final, utilize a tag <thinking> para estruturar seu raciocínio passo a passo, analisando o estado atual do mercado, as necessidades não atendidas dos usuários e as limitações dos concorrentes atuais.

<contexto>

O nicho de aplicativos de lista de compras e supermercado possui várias opções, mas muitas pecam em aspectos cruciais, como a sincronização em tempo real impecável, usabilidade com uma mão só no momento da compra, integração com o inventário da despensa de casa, ou recursos colaborativos ágeis para a família. O meu objetivo é desenvolver um sistema que explore essas brechas e ofereça diferenciais competitivos difíceis de serem copiados.

</contexto>


<instrucoes>

Siga os passos abaixo sequencialmente para montar sua pesquisa:

1. Mapeamento de Concorrentes: Liste os principais concorrentes diretos atuais (ex: Bring!, AnyList, Out of Milk, etc.) e resuma o núcleo das suas propostas de valor.

2. Identificação de Brechas (Loopholes): Aponte as principais falhas, vulnerabilidades e frustrações dos usuários nos sistemas existentes. Quais dores de usabilidade ou tecnologia ainda não foram resolvidas?

3. Oportunidades e Diferenciais: Proponha funcionalidades avançadas e inovadoras que podem ser exploradas (ex: previsão de escassez de produtos baseada em IA, arquitetura offline-first em PWA, gamificação de divisão de tarefas familiares, etc.).

4. Estratégia de Tecnologia e Distribuição: Compare brevemente as vantagens de lançar esse sistema como um PWA versus Aplicativo Nativo focado nesse público.

</instrucoes>


<restricoes>

- Seja altamente específico e evite jargões genéricos de negócios.

- Foque na experiência do usuário final durante a jornada real: antes (planejamento), durante (no supermercado) e depois (organização/pagamento).

- Não gere nenhum preâmbulo ou conclusão genérica. Vá direto aos dados e à análise.

</restricoes>


<formato_de_saida>

Estruture a sua resposta usando estritamente os seguintes tópicos (pode utilizar formatação Markdown e tabelas se necessário):

- Panorama dos Concorrentes

- Brechas e Frustrações do Mercado

- Diferenciais Inovadores para Exploração

- Distribuição: PWA vs App Nativo

</formato_de_saida>