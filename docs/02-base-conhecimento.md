# Base de Conhecimento

## Dados Utilizados

| Arquivo | Formato | Utilização no processo financeiro |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_gastos.json` | JSON | Perfil da pessoa |
| `solucao_financeira.json` | JSON | As metas a serem alcançadas|
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados?

Sim, 
- Os dados foram modificados para refletir salário de R$ 2.500
- As despesas foram ajustadas para incluir gastos fixos e variáveis
- Foi incluída dívida acumulada de R$ 1.000 no cartão de crédito
- O histórico foi expandido para cobrir todo o ano de 2025
- Foram adicionadas metas de quitação (06/2026) e reserva (12/2027)

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os JSON/CSV são carregados no início da sessão e incluídos no contexto do prompt

```text

DADOS DO USUÁRIO:

{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 2500.00,
  "perfil_gastos": "incontrolado",
  "objetivo_principal": "Liquidar divida com cartão",
  "patrimonio_total": 0,
  "reserva_emergencia_atual": 0,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Quitar divida do cartão de crédito",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Reserva de Emergência",
      "valor_necessario": 1000.00,
      "prazo": "2027-06"
    }
  ]
}

TRANSAÇÕES(Resumo):

data,descricao,categoria,valor,tipo
2025-01-01,Salário,receita,2500.00,entrada
2025-01-01,Aluguel,moradia,900.00,saida
2025-01-01,Internet,moradia,120.00,saida
2025-01-01,Conta de Luz,moradia,165.40,saida
2025-01-01,Supermercado,alimentacao,480.30,saida
2025-01-01,Transporte,transporte,210.00,saida


HISTÓRICO DE ATENDIMENTO:

data,canal,tema,resumo,resolvido
2025-09-15,chat,Limite do cartão,Cliente perguntou sobre aumento de limite disponível,sim
2025-09-22,telefone,Fatura em aberto,Cliente solicitou informações sobre valor mínimo e vencimento,sim
2025-10-01,chat,Parcelamento de fatura,Cliente pediu simulação para parcelar o saldo do cartão,sim
2025-10-12,chat,Juros do rotativo,Cliente solicitou explicação sobre cobrança de juros no cartão,sim
2025-10-25,email,Contestação de compra,Cliente informou compra não reconhecida na fatura,sim
2025-11-03,chat,Dívida acumulada,Cliente entrou no chat para entender o valor total da dívida e opções de negociação,sim


SOLUÇÃO FINANCEIRA(Possibilidades:

[
  {
    "nome": "Quitar Cartão de Crédito",
    "categoria": "prioridade_financeira",
    "risco": "alto (se não quitar)",
    "valor_total": 1000.00,
    "prazo_limite": "06/2026",
    "estrategia": "Parcelar ou negociar juros e pagar no máximo em 6 meses",
    "indicado_para": "Quem está pagando juros no rotativo e precisa sair da dívida"
  },
  {
    "nome": "Plano de Pagamento Mensal",
    "categoria": "organizacao",
    "risco": "baixo",
    "valor_mensal_sugerido": 170.00,
    "duracao_meses": 6,
    "objetivo": "Quitar os R$ 1.000 até 06/2026",
    "indicado_para": "Quem precisa de disciplina e previsibilidade"
  },
  {
    "nome": "Reserva Pós-Dívida",
    "categoria": "reserva_emergencia",
    "risco": "baixo",
    "valor_meta": 1000.00,
    "prazo_limite": "12/2027",
    "valor_mensal_sugerido": 60.00,
    "indicado_para": "Quem quer segurança financeira após sair da dívida"
  },
  {
    "nome": "Conta com Rendimento Diário",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% do CDI ou próximo",
    "aporte_minimo": 1.00,
    "indicado_para": "Guardar a reserva com liquidez imediata"
  },
  {
    "nome": "Aumento de Renda Complementar",
    "categoria": "estrategia",
    "risco": "baixo",
    "meta_extra_mensal": 200.00,
    "objetivo": "Acelerar quitação e formação da reserva",
    "indicado_para": "Quem pode fazer renda extra temporária"
  }
]


### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Dados do Cliente:
- Nome: João Silva
- Perfil: Incontrolado
- Saldo disponível: R$ 2.500
- Dívida no cartão: R$ 1.000
- Prazo para quitar dívida: 06/2026
- Meta de reserva: R$ 1.000 até 12/2027

Últimas transações:
- 01/11: Supermercado - R$ 520
- 03/11: Streaming - R$ 55
- 05/11: Farmácia - R$ 89
- 08/11: Fatura Cartão (pagamento mínimo) - R$ 150
  10/11: Conta de Luz - R$ 182
