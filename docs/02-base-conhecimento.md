# Base de Conhecimento

## Dados Utilizados

| Arquivo | Formato | Qual a função na Gi? |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar as explicações sobre as dúvidas e necessidades do cliente |
| `produtos_financeiros.json` | JSON | Ter conhecimento dos produtos que estão disponíveis, visando ensiná-los ao cliente |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente e usar essas informações de forma didática |

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

O produto Fundo Imobiliário (FII) substituiu o Fundo Multimercado, pois me sinto mais confiante em usar produtos financeiros de meu conhecimento.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Há duas possibilidades:
- Injetar os dados diretamente no prompt (Ctrl + C, Ctrl + V) ou,
- Carregar os arquivos via código, como mostrado abaixo: 

```python
import pandas as pd
import json

historico=pd.read_csv('data/historico_atendimento.csv')
transacoes=pd.read_csv('data/transacoes.csv')

with open('data/perfil_investidor.json', 'r', encoding='utf-8') as f:
  perfil=json.load(f)

with open('data/produtos_financeiros.json', 'r', encoding='utf-8') as f:
  produtos=json.load(f)
```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Para simplificar, podemos simplesmente "injetar" os dados em nosso prompt, garantindo que o agente tenha o melhor contexto possível. Lembrando que, em soluções mais robustas, o ideal é que essas informações sejam carregadas dinamicamente para que possamos ganhar flexibilidade.

```text
DADOS E PERFIL DO CLIENTE:
{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}



TRANSAÇÕES DO CLIENTE:
data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida
2025-10-25,Combustível,transporte,250.00,saida



HISTÓRICO DE ATENDIMENTO DO CLIENTE:
data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
2025-09-22,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
2025-10-01,chat,Tesouro Selic,Cliente pediu explicação sobre o funcionamento do Tesouro Direto,sim
2025-10-12,chat,Metas financeiras,Cliente acompanhou o progresso da reserva de emergência,sim
2025-10-25,email,Atualização cadastral,Cliente atualizou e-mail e telefone,sim



PRODUTOS DISPONÍVEIS PARA ENSINO:
[
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CD",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Imobiliário (FII)",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "Dividend Yield (DY) costuma ficar entre 6% a 12% ao ano",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil moderado que busca diversificação e renda recorrente mensal"
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo"
  }
]
```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
DADOS DO CLIENTE:
- Nome: João Silva
- Profissão: Analista de Sistemas
- Perfil: Moderado
- Saldo disponível: R$ 5.000
- Objetivo principal: Construir reserva de emergência
- Reserva: R$ 10.000

ÚLTIMAS TRANSAÇÕES:
- 02/10: Aluguel - R$ 1200
- 03/10: Supermercado - R$ 450
- 05/10: Netflix - R$ 55.90
- 07/10: Farmácia- R$ 89
- 10/10: Restaurante - R$ 120
- 12/10: Uber - R$ 45
- 15/10: Conta de Luz - R$ 180
- 20/10: Academia - R$ 99
- 25/10: Combustível - R$ 250

PRODUTOS DISPONÍVEIS:
Tesouro Selic
Obs: baixo risco
    
CDB Liquidez Diária
Obs: risco baixo

LCI/LCA
Obs: risco baixo

Fundo Imobiliário (FII)
Obs: risco médio

Fundo de Ações
Obs: risco alto
...
```
