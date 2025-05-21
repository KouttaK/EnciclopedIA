# Diretrizes para Programação JavaScript Desmodularizada

## Filosofia Fundamental

Trabalhamos com uma abordagem **desmodularizada e controlada**, onde:

* Sempre responda em português brasileiro
* O código é centralizado em arquivos maiores e mais completos
* O desenvolvimento segue um fluxo rigoroso de aprovação por etapas
* A reutilização de código e estilos é priorizada
* Cada alteração é feita com pleno conhecimento do contexto

## Estrutura de Diretórios

```
projeto/
├── src/
│   ├── core/              # Funcionalidades essenciais e utilitários globais
│   ├── utils/             # Utilitários compartilhados
│   └── services/          # Serviços para comunicação externa
├── tests/                 # Testes automatizados
└── docs/                  # Documentação específica
```

## Diretrizes para Cada Ação de Desenvolvimento

### Desenvolvimento Desmodularizado

Ao criar ou editar arquivos JavaScript:

* **Centralização**: Todo o código relacionado a uma funcionalidade deve ser mantido em um único arquivo
* **Utilitários locais**: Implemente funções utilitárias específicas dentro do próprio arquivo
* **Server Actions**: Mantenha ações do servidor específicas dentro do mesmo arquivo que as consome

### Gerenciamento de Estado e Bibliotecas

* Priorize componentes de bibliotecas existentes para evitar reinventar soluções
* Use a pasta `/api/` exclusivamente quando necessário para rotas da API
* Mantenha estilos DRY (Don't Repeat Yourself) importando de um arquivo central de estilos

## Fluxo Obrigatório de Desenvolvimento

### 1. Fase de Estudo

Antes de qualquer modificação:

* Liste todos os arquivos que serão modificados
* Para cada arquivo, identifique com precisão:
  * Quais trechos de código devem permanecer imutáveis
  * Quais estilos precisam ser preservados
  * Quais funcionalidades não podem ser alteradas
* Documente estas informações para referência durante todo o desenvolvimento

### 2. Fase de Análise

Com base no estudo inicial:

* Divida a tarefa em passos menores e gerenciáveis
* Estabeleça uma sequência lógica para execução
* Identifique e documente potenciais riscos ou dependências entre os passos
* Crie um plano explícito de execução com todos os detalhes necessários

### 3. Fase de Execução Controlada

Para cada passo identificado:

1. Apresente detalhadamente o passo a ser executado e solicite aprovação explícita
2. Aguarde confirmação antes de prosseguir com qualquer alteração
3. Execute apenas após receber aprovação formal
4. Revise minuciosamente o resultado e confirme a integridade do código
5. Prossiga para o próximo passo somente após nova aprovação

## Benefícios desta Abordagem

* **Maior controle**: Cada modificação é conscientemente revisada e aprovada
* **Contexto preservado**: Ao manter código relacionado junto, facilita-se a compreensão global
* **Redução de erros**: A abordagem passo-a-passo minimiza problemas de integração
* **Melhor colaboração com IA**: Os AI Copilots têm acesso ao contexto completo do código
* **Manutenibilidade aprimorada**: Código centralizado é mais fácil de compreender e modificar

## Pontos de Atenção

* Esteja constantemente atento para não duplicar estilos ou lógica
* Execute apenas uma ação por vez, com foco total e atenção aos detalhes
* Sempre valide a integridade do código após cada modificação
* Documente decisões de design importantes diretamente no código através de comentários claros
* Priorize a legibilidade e organização interna dos arquivos maiores usando seções bem demarcadas

## Práticas Recomendadas

* Use comentários descritivos para separar seções lógicas em arquivos maiores
* Mantenha uma estrutura consistente em todos os arquivos do projeto
* Utilize formatação padronizada para facilitar a leitura do código    
* Documente dependências e requisitos no topo de cada arquivo
* Implemente validação de dados e tratamento de erros de forma abrangente
