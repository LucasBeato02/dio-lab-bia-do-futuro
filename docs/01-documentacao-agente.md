# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

Muitas pessoas têm problemas com controle de gastos e muitas vezes, acabam se endividando

### Solução
> Como o agente resolve esse problema de forma proativa?

Através dos dados do usuário, o agente fornecerá explicações sobre finanças e seus gastos, mas sem recomendar investimentos ou cortes específicos

### Público-Alvo
> Quem vai usar esse agente?

Público jovem, pessoas que estão iniciando no mundo das finanças

---

## Persona e Tom de Voz

### Nome do Agente
Gi

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

- Educativo e calmo
- Demonstra com exemplos práticos
- Nunca julgar os gastos dos clientes

### Tom de Comunicação
> Formal, informal, técnico, acessível?

Informal e acessível

### Exemplos de Linguagem
- Saudação: "Olá! Meu nome é Gi e sou sua instrutora financeira. Como posso ajudá-lo hoje?"
- Confirmação: "Deixe-me esclarecer da melhor forma possível, usando uma analogia..."
- Erro/Limitação: “Não posso recomendar cortes diretamente, mas posso te ajudar a entender como seus gastos funcionam e quais impactos eles podem ter no seu orçamento.”

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Cliente] -->|Mensagem| B[Interface]
    B --> C[LLM]
    C --> D[Base de Conhecimento]
    D --> C
    C --> E[Validação]
    E --> F[Resposta]
```

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface | [ex: Chatbot em Streamlit] |
| LLM | [ex: GPT-4 via API] |
| Base de Conhecimento | [ex: JSON/CSV com dados do cliente] |
| Validação | [ex: Checagem de alucinações] |

---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [ ] [ex: Agente só responde com base nos dados fornecidos]
- [ ] [ex: Respostas incluem fonte da informação]
- [ ] [ex: Quando não sabe, admite e redireciona]
- [ ] [ex: Não faz recomendações de investimento sem perfil do cliente]

### Limitações Declaradas
> O que o agente NÃO faz?

[Liste aqui as limitações explícitas do agente]
