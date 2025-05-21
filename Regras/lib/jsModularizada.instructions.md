# Diretrizes para Programação JavaScript Modularizada

## Filosofia Fundamental

Trabalhamos com uma abordagem **modularizada e estruturada**, onde:

* O código é dividido em módulos pequenos e especializados
* Cada módulo possui uma responsabilidade única e bem definida
* A reutilização de código é facilitada através de importações claras
* O desenvolvimento é ágil e paralelo, permitindo trabalho simultâneo em diferentes módulos

Esta abordagem foi desenvolvida para maximizar a manutenibilidade, escalabilidade e reutilização de código em projetos JavaScript.

## Estrutura de Diretórios

```
projeto/
├── src/
│   ├── core/              # Funcionalidades essenciais e utilitários globais
│   ├── modules/           # Módulos independentes da aplicação
│   │   ├── moduleA/       # Cada módulo em sua própria pasta
│   │   │   ├── index.js   # Ponto de entrada do módulo
│   │   │   ├── helpers.js # Funções auxiliares específicas
│   │   │   └── constants.js
│   │   └── moduleB/
│   ├── utils/             # Utilitários compartilhados
│   └── services/          # Serviços para comunicação externa
├── tests/                 # Testes automatizados
└── docs/                  # Documentação específica
```

## Diretrizes para Modularização JavaScript

### Definição de Módulos

* **Módulo = Responsabilidade única**: Cada arquivo JS deve ter um propósito claro e limitado
* **Tamanho ideal**: Módulos devem ter geralmente menos de 300 linhas
* **Coesão**: Funções em um módulo devem estar logicamente relacionadas
* **Acoplamento baixo**: Minimizar dependências entre módulos diferentes

### Exportações e Importações

* **Exportações nomeadas**: Priorize exportações nomeadas para maior clareza
  ```javascript
  // utils.js
  export function formatDate(date) { /* ... */ }
  export function parseValue(value) { /* ... */ }
  ```

* **Exportação padrão**: Use para a função/classe principal de um módulo
  ```javascript
  // UserManager.js
  function UserManager() { /* ... */ }
  export default UserManager;
  ```

* **Importações organizadas**: Agrupe importações por origem e mantenha ordem consistente
  ```javascript
  // Bibliotecas externas primeiro
  import EventEmitter from 'events';
  
  // Depois módulos internos
  import { formatDate } from '../utils/dateUtils.js';
  import Logger from '../core/Logger.js';
  ```

### Padrões de Design para Módulos

* **Module Pattern**: Encapsule variáveis privadas
  ```javascript
  // counter.js
  const Counter = (function() {
    let count = 0;
    
    return {
      increment() { count += 1; return count; },
      decrement() { count -= 1; return count; },
      getValue() { return count; }
    };
  })();
  
  export default Counter;
  ```

* **Factory Functions**: Para criação de objetos similar a classes
  ```javascript
  // userFactory.js
  export function createUser(name, email) {
    return {
      name,
      email,
      getDisplayName() {
        return `${name} <${email}>`;
      }
    };
  }
  ```

* **Namespaces**: Agrupe funções relacionadas
  ```javascript
  // mathUtils.js
  export const MathUtils = {
    add(a, b) { return a + b; },
    subtract(a, b) { return a - b; },
    multiply(a, b) { return a * b; },
    divide(a, b) { return a / b; }
  };
  ```

## Fluxo de Desenvolvimento

### 1. Fase de Planejamento

* Identifique as principais funcionalidades e divida-as em módulos potenciais
* Mapeie as dependências entre os módulos
* Crie um diagrama de fluxo de dados para visualizar as interações

### 2. Fase de Implementação

* Desenvolva módulos de baixo para cima (utilitários primeiro, depois funcionalidades de alto nível)
* Implemente um módulo por vez, com testes unitários
* Documente a API pública de cada módulo

### 3. Fase de Integração

* Integre os módulos gradualmente
* Realize testes de integração
* Verifique a consistência das interfaces entre módulos

## Gerenciamento de Estado

* **Eventos**: Use sistema de eventos para comunicação entre módulos desacoplados
  ```javascript
  // eventBus.js
  export const EventBus = {
    events: {},
    
    subscribe(event, callback) {
      if (!this.events[event]) this.events[event] = [];
      this.events[event].push(callback);
    },
    
    publish(event, data) {
      if (!this.events[event]) return;
      this.events[event].forEach(callback => callback(data));
    }
  };
  ```

* **Store Pattern**: Para estado global compartilhado
  ```javascript
  // store.js
  export const Store = {
    state: {},
    listeners: [],
    
    setState(newState) {
      this.state = {...this.state, ...newState};
      this.notifyListeners();
    },
    
    getState() {
      return this.state;
    },
    
    subscribe(listener) {
      this.listeners.push(listener);
      return () => {
        this.listeners = this.listeners.filter(l => l !== listener);
      };
    },
    
    notifyListeners() {
      this.listeners.forEach(listener => listener(this.state));
    }
  };
  ```

## Melhores Práticas

* **Nomes descritivos**: Nomeie arquivos e funções de forma clara e descritiva
* **Comentários JSDoc**: Documente funções com comentários JSDoc
  ```javascript
  /**
   * Formata um número para exibição com separadores de milhar e decimais
   * @param {number} value - O valor a ser formatado
   * @param {number} [decimals=2] - O número de casas decimais
   * @param {string} [decimalSeparator=','] - O separador decimal
   * @param {string} [thousandsSeparator='.'] - O separador de milhar
   * @return {string} O valor formatado
   */
  export function formatNumber(value, decimals = 2, decimalSeparator = ',', thousandsSeparator = '.') {
    // Implementação...
  }
  ```

* **Testes por módulo**: Crie testes unitários correspondentes para cada módulo
* **Lazy loading**: Carregue módulos sob demanda quando possível
* **Imutabilidade**: Evite modificar diretamente os argumentos das funções

## Pontos de Atenção

* Evite dependências circulares entre módulos
* Mantenha as interfaces públicas estáveis e bem documentadas
* Prefira composição em vez de herança para reutilização de código
* Estabeleça convenções claras de nomenclatura para todo o projeto
* Implemente validação de dados nas fronteiras dos módulos (entrada/saída)

## Exemplo de Implementação Modular

### Módulo de Autenticação
```javascript
// auth/validation.js
export function validateEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}

export function validatePassword(password) {
  return password.length >= 8;
}
```

```javascript
// auth/user.js
import { validateEmail, validatePassword } from './validation.js';

export function createUser(email, password) {
  if (!validateEmail(email)) {
    throw new Error('Email inválido');
  }
  
  if (!validatePassword(password)) {
    throw new Error('Senha deve ter pelo menos 8 caracteres');
  }
  
  return {
    email,
    passwordHash: hashPassword(password),
    createdAt: new Date()
  };
}

function hashPassword(password) {
  // Implementação de hash
  return `hashed_${password}`;
}
```

```javascript
// auth/index.js
import { createUser } from './user.js';
import { EventBus } from '../core/eventBus.js';

export function register(email, password) {
  try {
    const user = createUser(email, password);
    saveUser(user);
    EventBus.publish('user:registered', { email });
    return true;
  } catch (error) {
    console.error('Erro no registro:', error.message);
    return false;
  }
}

function saveUser(user) {
  // Implementação para salvar o usuário
  console.log('Usuário salvo:', user);
}

export function login(email, password) {
  // Implementação de login
}

// Exporta a API pública do módulo
export default {
  register,
  login
};
```