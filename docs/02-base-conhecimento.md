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
- Perfil: Moderado
- Saldo disponível: R$ 5.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
