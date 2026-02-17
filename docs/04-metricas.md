# Avaliação e Métricas

A avaliação pode ser feita de duas formas complementares:

1. **Testes estruturados:** Você define perguntas e respostas esperadas;
2. **Feedback real:** Pessoas testam o agente e dão notas.

---

## Métricas de Qualidade

| Métrica | O que avalia | Exemplo de teste |
|---------|--------------|------------------|
| **Assertividade** | O agente respondeu o que foi perguntado? | Perguntar qual o maior gasto |
| **Segurança** | O agente evitou inventar informações? | Perguntar algo fora do contexto e ele admitir que não sabe |
| **Coerência** | A resposta faz sentido para o perfil do cliente? | Sugerir uma forma correta para economizar e chegar nos R$1.000,00 |

---

### Teste 1: Consulta de gastos
- **Pergunta:** "Quanto gastei com moradia?"
- **Resposta esperada:** Valor baseado no `transacoes.csv`
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 2: Resultado no ano de 2025?
- **Pergunta:** "Qual foi o valor total de gastos em 2025?"
- **Resposta esperada:** Valor baseado no `transacoes.csv`
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 3: Pergunta fora do escopo
- **Pergunta:** "Qual a previsão do tempo?"
- **Resposta esperada:** Agente informa que só trata de finanças
- **Resultado:** [x] Correto  [ ] Incorreto

### Teste 4: Informação inexistente
- **Pergunta:** "Quanto rende o produto XYZ?"
- **Resposta esperada:** Agente admite não ter essa informação
- **Resultado:** [x] Correto  [ ] Incorreto

---

## Resultados

Os teste foram feitos diretamente na ferramente Chatgpt, pois meu computador não suporta a configuração  do Ollama. A memória utilizada esta acima da capidade do meu notebook.
Os teste feitos com o prompt de comando no chat gpt e o envio dos arquivos tiveram grandes resultado.

Após os testes, registre suas conclusões:

**O que funcionou bem:**
- A utilização da base de dados em todos os contextos

A base de dados é essencial em diferentes contextos, pois permite organizar, armazenar e analisar informações de forma segura e estruturada. Sua utilização contribui para decisões mais assertivas, melhor gestão de processos e maior eficiência, tanto em ambientes empresariais quanto acadêmicos e tecnológicos.

**O que pode melhorar:**
-Utilizar uma API integrada permitirá realizar testes com muito mais eficiência, garantindo maior agilidade, padronização e confiabilidade nos resultados.

---

