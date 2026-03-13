# Teste Técnico — Central de Enquetes (75% Bubble + 25% Highcode)

## Sobre a Pipeimob

A Pipeimob é uma plataforma de gestão imobiliária que nasceu 100% em Bubble e está em migração para highcode. Este teste avalia suas habilidades como desenvolvedor Bubble que participa da migração gradual para highcode.

## Prazo

- **48 horas** após recebimento
- Tempo estimado: **4-6 horas**
- Uso de IA é permitido e esperado (documente no NOTES.md)

## O Desafio

Construir uma **Central de Enquetes** no Bubble com um microserviço de análise em Python/FastAPI.

---

## Parte 1: Bubble (principal)

### 1. Autenticação

- Login e cadastro com e-mail/senha
- Redirecionamento adequado (logado → página principal, não logado → login)
- Validação de campos
- Usar **API Connector** para chamar API externa gratuita (ex: ipify.org) que retorna o IP do usuário
- Registrar o IP em tabela de log a cada login
- Tratar erros da API (se fora do ar, login funciona normalmente)

### 2. Enquetes

- Formulário com título, opções (mín. 2), tipo via **Option Set** ("Opinião", "Pesquisa", "Votação")
- Toggle público/privado (**Ionic Toggle** ou similar)
- **Repeating Group com elemento reutilizável** para exibir enquetes
- Botão criar enquete funcional

### 3. Timeline / Feed

- Enquetes públicas para todos, privadas só para o criador
- Resultados atualizam em **tempo real**
- Voto **único** por enquete
- CRUD: editar e excluir (apenas criador)

### 4. Filtro

- Filtro por intervalo de datas

### 5. Database Trigger (DBT)

- Ao registrar voto, criar automaticamente registro em `historico_de_votos` (user, enquete, opção, data/hora)

### 6. Recursividade (API Workflows)

- API Workflow recursivo para **arquivar** múltiplas enquetes (bulk)
- API Workflow recursivo para **desarquivar** múltiplas enquetes (bulk)

### 7. Webhooks (Backend Workflows)

- **POST**: Recebe UniqueID e arquiva a enquete
- **GET**: Retorna JSON com dados da enquete (título, opções, votos)

### 8. Retorno Síncrono

- **API Connector** chamando um **Backend Workflow do próprio app Bubble** para retornar dados de forma síncrona

---

## Parte 2: Microserviço Highcode (complementar)

Criar um microserviço simples em **Python/FastAPI** que analisa dados de enquetes.

### Requisitos do microserviço

- **Endpoint `POST /analyze`**: Recebe dados de uma enquete (título, opções com contagem de votos) e retorna:
  - Opção vencedora
  - Percentual de cada opção
  - Total de votos
  - Índice de dispersão (quão equilibrada está a votação)
- **Deploy**: O microserviço deve estar deployado com URL pública (Railway, Render, Fly.io, etc.)
- **Integração no Bubble**: Usar API Connector para chamar o microserviço e exibir os resultados de análise na interface

### Exemplo de request/response

```json
// POST /analyze
// Request:
{
  "title": "Melhor linguagem 2026?",
  "options": [
    {"name": "Python", "votes": 45},
    {"name": "JavaScript", "votes": 30},
    {"name": "Rust", "votes": 25}
  ]
}

// Response:
{
  "winner": "Python",
  "total_votes": 100,
  "percentages": {
    "Python": 45.0,
    "JavaScript": 30.0,
    "Rust": 25.0
  },
  "dispersion_index": 0.82
}
```

---

## Padrões Esperados

- **Nomenclatura de elementos**: nomes descritivos
- **snake_case** em tabelas e campos do banco
- **Organização de workflows**: pastas e cores
- **Estrutura visual**: design limpo e funcional
- **Código Python**: organizado, com docstrings básicas

## Diferenciais (opcionais)

- Lazy loading / paginação
- Group Focus com hover
- Popup reutilizável para edição
- Navegação com parâmetros de URL
- JavaScript (scroll suave, tooltips)
- Layout responsivo

---

## Entrega

1. **Link do app Bubble** (editor em modo view + app rodando)
2. **Repositório GitHub** do microserviço Python
3. **URL pública** do microserviço deployado
4. **NOTES.md** documentando:
   - Ferramentas de IA usadas
   - Como a IA ajudou no desenvolvimento
   - Decisões técnicas importantes
   - Dificuldades e soluções

---

**Boa sorte! 🚀**
