# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar as explicações sobre dúvidas e  necessidades do cliente |
| `produtos_financeiros.json` | JSON |Conhecer os produtos disponíveis para explicar ao cliente |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Os arquivos produtos_financeiros.json e transacoes.csv foram atualizados para melhorar o realismo e a usabilidade dos dados. No arquivo de produtos financeiros, as informações foram padronizadas e ampliadas com novos tipos de investimentos. Já no arquivo de transações, foram adicionados mais meses de dados, mantendo valores fixos para receitas e despesas recorrentes e valores variáveis para gastos do dia a dia, simulando um comportamento financeiro mais próximo do real.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

```python
import pandas as pd
import json

# CSVs
historico = pd.read_csv('data/historico_atendimento.csv')
transacoes = pd.read_csv('data/transacoes.csv')

# JSONs
with open('data/perfil_investidor.json', 'r', encoding='utf-8') as f:
    perfil = json.load(f)

with open('data/produtos_financeiros.json', 'r', encoding='utf-8') as f:
    produtos = json.load(f)
```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

São carregados dinamicamente a partir dos arquivos (como transacoes.csv e produtos_financeiros.json) e utilizados conforme necessário durante a execução. Isso permite que o agente consulte informações atualizadas, personalize respostas com base no perfil do usuário e analise o histórico financeiro de forma mais eficiente, sem sobrecarregar o prompt com grandes volumes de dados.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: João Silva
- Idade: 32
- Profissão: Analista de Sistemas
- Renda mensal: R$ 5.000
- Perfil de investidor: Moderado
- Objetivo principal: Construir reserva de emergência
- Patrimônio total: R$ 15.000
- Reserva de emergência atual: R$ 10.000

Metas:
- Completar reserva de emergência: R$ 15.000 até 06/2026
- Entrada do apartamento: R$ 50.000 até 12/2027

Últimas transações:
- 01/01: Salário +R$ 5.000
- 02/01: Aluguel -R$ 1.200
- 05/01: Supermercado -R$ 480
- 13/01: Restaurante -R$ 110
- 16/01: Uber -R$ 42

Histórico de atendimento:
- 15/09: Dúvida sobre CDB (resolvido)
- 01/10: Explicação sobre Tesouro Selic (resolvido)
- 12/10: Acompanhamento de metas financeiras (resolvido)

Produtos disponíveis (resumo):
- Tesouro Selic (baixo risco, ideal para reserva)
- CDB Liquidez Diária (baixo risco, rendimento diário)
- Fundo Multimercado (risco médio, diversificação)
- Ações (alto risco, longo prazo)

```
