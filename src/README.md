import json
import pandas as pd
import requests
import streamlit as st

# ============ CONFIGURAÇÃO ============
OLLAMA_URL = "http://localhost:11434/api/generate"
MODELO = "phi3"

# ============ CARREGAR DADOS ============
perfil = json.load(open('./perfil_gastos.json'))
transacoes = pd.read_csv('./transacoes.csv')
historico = pd.read_csv('./historico_atendimento.csv')
solucao = json.load(open('./solucao_financeira.json'))

# ============ NOTAS CONTEXTO ============
contexto = f"""
CLIENTE: {perfil['nome']}, {perfil['idade']} anos, {perfil['perfil_usuario']}
OBJETIVO: {perfil['objetivo_principal']}
PATRIMONIO: R$ {perfil['patrimonio_total']} | RESERVA: {perfil['reserva_emergencia_atual']}

TRANSAÇÕES RECENTES:
{transacoes.to_string(index=False)}

ATENDIMENTOS ANTERIORES:
{historico.to_string(index=False)}

SOLUÇÕES DISPONIVEIS:
{json.dumps(solucao, indent=2, ensure_ascii=False)}
"""

# ============ SYSTEM PROMPT ============
SYSTEM_PROMPT = """Você é o João Educa, um educador financeiro amigável e didático.
                                              
OBJETIVO:
Ajudar pessoas com dívidas a organizar o orçamento, quitar o que devem e economizar de forma consciente, com base nos próprios dados financeiros.

REGRAS:
1. Sempre baseie suas respostas nos dados fornecidos;
2. Nunca invente informações financeiras;
3. Se não souber algo, admita e ofereça alternativas;
4. Gastos fixos devem ser avaliados se é possível economizar criando prioridades;
5. O usuário receberá dicas possíveis e não inventadas;
6. Se não souber algo, admita que não sabe;
7. Linguagem simples de fácil entendimento;
8. Sempre pergunte se o usuário entendeu;
9. Responda somente sobre as finanças atuais;
10. Não palpite sobre outros assuntos;
11. Seja breve nas respostas de forma simples;
"""

# ============ CHAMA OLLAMA ============
def perguntar(msg):
    prompt = f"""
{SYSTEM_PROMPT}

CONTEXTO DO CLIENTE:
{contexto}

Pergunta: {msg}
"""

    r = requests.post(
        OLLAMA_URL,
        json={
            "model": MODELO,
            "prompt": prompt,
            "stream": False
        }
    )

    return r.json()['response']

# ============ INTERFACE ============
st.title("João Educa - Seu Educador Financeiro")

if pergunta := st.chat_input("Sua dúvida sobre finanças..."):
    st.chat_message("user").write(pergunta)
    with st.spinner("Analisando..."):
        resposta = perguntar(pergunta)
        st.chat_message("assistant").write(resposta)
