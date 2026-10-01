# 🔄 Conciliação de Repasses — Sistema Financeiro x Google Sheets (Multi-Competência)

Script em Python para **Google Colab** que automatiza a conciliação de pagamentos entre um relatório financeiro exportado em `.xlsx` e uma planilha de controle no Google Sheets, organizada por competência (mês/ano).

Criado para substituir um processo manual: antes, alguém precisava abrir dois sistemas financeiros diferentes, cruzar empenho por empenho e preencher a referência de cada pagamento na mão, todo mês.

---

## ✨ O que o script faz

1. **Lê um arquivo `.xlsx` de origem** (relatório financeiro) e filtra apenas os registros com situação "Paga"
2. **Identifica automaticamente** a aba de destino certa na planilha, com base na competência (mês/ano) de cada registro — sem precisar informar manualmente
3. **Cruza cada pagamento** com a linha correspondente na planilha, usando o número de empenho como chave e uma tolerância de centavos na comparação de valor
4. **Preenche automaticamente** a coluna de referência (O.B.) quando encontra correspondência
5. **Gera um relatório completo** ao final, mostrando:
   - Quantos registros foram atualizados
   - Divergências de valor (quando o empenho bate, mas o valor não confere dentro da tolerância)
   - Empenhos que não foram encontrados
   - Linhas com valor inválido/vazio

---

## 🧰 Tecnologias usadas

- Python (Google Colab)
- [gspread](https://docs.gspread.org/) — integração com Google Sheets
- [pandas](https://pandas.pydata.org/) — leitura e tratamento do arquivo `.xlsx`
- Google OAuth (autenticação nativa do Colab)

---

## 🚀 Como usar

1. Abra o script no **Google Colab**
2. Informe o **ID da planilha do Google Sheets** no campo indicado (está na URL da planilha, entre `/d/` e `/edit`)
3. Clique no botão de **Play (▶️)**
4. Quando solicitado, faça login com sua conta Google e autorize o acesso
5. Selecione o arquivo `.xlsx` de origem quando o botão de upload aparecer
6. Acompanhe o processamento e o relatório final no console

---

## ⚙️ Configuração

O script espera que a planilha de origem (`.xlsx`) tenha, nas colunas corretas:

| Coluna | Conteúdo |
|---|---|
| C | Número da O.B. (referência a ser preenchida) |
| E | Número do empenho |
| J | Situação do pagamento (considera apenas "Paga") |
| Q | Competência da despesa (formato `AAAAMM`, ex: `202607`) |
| W | Valor do pagamento |

E que a planilha do Google Sheets tenha uma aba por competência, nomeada no padrão `Competência {Mês}` (ex: `Competência Julho`), com:

| Coluna | Conteúdo |
|---|---|
| K | Valor a conciliar |
| M | Referência (O.B.) — preenchida automaticamente pelo script |
| R | Número do empenho |

Se alguma aba tiver nome diferente do padrão, é possível mapear manualmente editando o dicionário `MAPA_ABAS_MANUAL` no início do script.

Os índices de coluna (tanto do arquivo de origem quanto da planilha) podem ser ajustados nas constantes no topo do script, caso seu layout seja diferente.

---

## ⚠️ Observações

- O script **nunca sobrescreve** uma O.B. já usada: cada correspondência é "consumida" para evitar duplicidade
- A tolerância de centavos (`TOLERANCIA_CENTAVOS`) pode ser ajustada conforme necessário
- Projetado para rodar em Google Colab; não requer instalação local de dependências

---

## 📄 Licença

Sinta-se livre para adaptar este script para o seu próprio caso de uso.
