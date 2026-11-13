# ⚡ SeguWatts — Assistente Inteligente de Uso Consciente de Energia Residencial

![n8n](https://img.shields.io/badge/n8n-Automation-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-AI%20Decision-412991?style=for-the-badge&logo=openai&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-Database-34A853?style=for-the-badge&logo=googlesheets&logoColor=white)
![Gmail](https://img.shields.io/badge/Gmail-Alerts-D14836?style=for-the-badge&logo=gmail&logoColor=white)

## 📖 Descrição do Projeto

O **SeguWatts** é uma solução *CitizenTech* focada na democratização da eficiência energética residencial. O projeto atua como um assistente inteligente de monitoramento e auditoria de consumo elétrico, desenvolvido para mostrar ao morador **quando**, **onde** e **quanto** a residência está gastando, identificando anomalias ou possíveis defeitos em eletrodomésticos antes que o impacto apareça na fatura mensal.

Voltado especialmente para famílias de baixa e média renda no estado de São Paulo, o sistema combate o "desperdício invisível" transformando leituras brutas de consumo em alertas claros, proativos e acionáveis.

### 🌍 Alinhamento com os ODS (ONU)
* **ODS 10 — Redução das Desigualdades:** Proteção do orçamento doméstico de famílias vulneráveis contra custos elétricos ocultos.
* **ODS 11 — Cidades e Comunidades Sustentáveis:** Promoção do consumo consciente e gestão eficiente de recursos urbanos.
* **ODS 13 — Ação Contra a Mudança Global do Clima:** Redução do desperdício energético e otimização da demanda elétrica.

---

## 👥 Integrantes do Grupo

* **Davi Passanha de Sousa Guerra** — RM551605
* **Cauã Gonçalves de Jesus** — RM97648
* **Luan Silveira Macea** — RM98290
* **Rui Amorim Siqueira** — RM98436
* **Luigi Ferrara Sinno** — RM98047

---

## 🏗️ Arquitetura do Fluxo (n8n)

O pipeline de automação foi construído de forma modular no **n8n**, dividido em três grandes estágios:

1. **Simulação da Entrada de Energia (IoT Mock):**
   * Gera uma linha do tempo fracionada em 60 ciclos (representando 1 hora de monitoramento contínuo).
   * Utiliza laços de repetição (*Loops*) e scripts em JavaScript para simular leituras estocásticas de potência (em Watts) e probabilidades reais de funcionamento de 15 eletrodomésticos residenciais (Geladeira, Ar-condicionado, Chuveiro Elétrico, Lâmpadas, Roteador, entre outros).
2. **Análise e Armazenamento de Dados:**
   * Consolida as 60 leituras brutas, calculando a média de consumo de cada equipamento e comparando-a com a sua média geral de referência.
   * Classifica cada aparelho com o status **`Baixo`**, **`Moderado`** ou **`Alto`** e registra todo o histórico cronológico em uma planilha no **Google Sheets** para auditoria.
3. **Tomada de Decisão Autônoma e Integração com IA:**
   * Filtra apenas os itens críticos (`Moderado` ou `Alto`) e os serializa em um payload JSON estruturado.
   * Submete os dados a um agente de Inteligência Artificial (**OpenAI**), que avalia a gravidade do desvio e decide autonomamente entre `"Enviar email"` ou `"Não enviar email"`, evitando falsos positivos (*alarm fatigue*).
   * Caso o alerta seja necessário, um roteador condicional (*If*) aciona a API do **Gmail**, disparando um e-mail personalizado ao morador com diagnósticos e dicas práticas de economia.

---

## ⚙️ Instruções de Configuração e Execução

### Pré-requisitos
* Instância ativa do **n8n** (Cloud ou Self-Hosted via Docker/Local).
* Conta Google com acesso ao **Google Sheets** e **Gmail**.
* Chave de API ativa da **OpenAI**.

### Passo a Passo para Importação
1. **Clone este repositório:**
   ```bash
   git clone [INSERIR_LINK_DO_REPOSITORIO_AQUI]
   ```
2. **Importe o Workflow no n8n:**
   * Acesse o painel do seu n8n e crie um novo workflow.
   * No menu superior direito, clique em **Import from File** e selecione o arquivo `.json` do fluxo disponível neste repositório.
3. **Configure a Planilha de Auditoria (Google Sheets):**
   * Crie uma planilha no Google Sheets com as seguintes colunas na primeira linha (`A1:G1`):
     `Data` | `Hora` | `Aparelho` | `Consumo medio` | `Media geral` | `Status` | `Diferenca media`
4. **Configure as Credenciais no n8n:**
   * Vincule suas credenciais OAuth2 nos nós do **Google Sheets** e **Gmail**.
   * Insira sua API Key no nó da **OpenAI (`Message a model`)** e ajuste o destinatário no nó de envio de e-mail.
5. **Execute o Fluxo:**
   * Clique em **Execute Workflow** para iniciar a simulação de 60 minutos, acompanhar a gravação na planilha e validar a tomada de decisão da IA.

---

## 🔗 Links Importantes

* **🎥 Vídeo Demonstrativo (YouTube):** https://youtu.be/_r6gA2Y_60k?si=lUr4K27SqhDz8v0A
* **📊 Planilha Modelo de Auditoria (Google Sheets):** https://docs.google.com/spreadsheets/d/1hwuPhDQvsBa-Qne14GpYfPxAfn3KpRnjP1ebHk3pBvs/edit?usp=drive_link
* **📄 Relatório Técnico Completo:** Disponível na pasta `/docs` deste repositório.
