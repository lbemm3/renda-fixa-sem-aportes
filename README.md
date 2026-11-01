[README (6).md](https://github.com/user-attachments/files/34230304/README.6.md)
# 📈 Simulador de Renda Fixa — CDB, RDB e Tesouro SELIC

Simulador educacional, em um único arquivo HTML, que projeta a evolução de um **capital inicial** aplicado em renda fixa pós-fixada, com o **Imposto de Renda regressivo descontado apenas no resgate**.

Esta é a versão **sem aportes**: o investidor aplica um valor uma única vez e acompanha o saldo mês a mês.

---

## 📑 Sumário

- [📌 Sobre o projeto](#-sobre-o-projeto)
- [✨ Funcionalidades](#-funcionalidades)
- [🧮 Fórmulas utilizadas](#-fórmulas-utilizadas)
- [🧾 Imposto de Renda regressivo](#-imposto-de-renda-regressivo)
- [✏️ Exemplo](#️-exemplo)
- [🚀 Como usar](#-como-usar)
- [⚠️ Premissas e limitações](#️-premissas-e-limitações)
- [🛠️ Tecnologias](#️-tecnologias)
- [📁 Estrutura sugerida do repositório](#-estrutura-sugerida-do-repositório)
- [🔗 Projeto relacionado](#-projeto-relacionado)
- [👤 Autoria](#-autoria)
- [📄 Licença](#-licença)

---

## 📌 Sobre o projeto

Em aplicações como CDB, RDB e Tesouro SELIC, o saldo cresce **bruto** ao longo do tempo e o IR incide **uma só vez**, sobre o **ganho total**, no momento do resgate. A alíquota diminui conforme o prazo da aplicação.

O simulador mostra, mês a mês, o saldo bruto e o que o investidor receberia **se resgatasse naquele mês**, já descontado o IR.

| Símbolo | Significado |
|:-------:|-------------|
| $C_0$ | Capital inicial |
| $S$ | Selic projetada (% a.a.) |
| $i_f$ | Taxa anual do fundo, $i_f = S - 0{,}10\%$ |
| $t$ | Número de meses decorridos |
| $B_t$ | Saldo bruto ao fim do mês $t$ |
| $G_t$ | Ganho total, $G_t = B_t - C_0$ |
| $\alpha_t$ | Alíquota de IR aplicável ao prazo |
| $L_t$ | Saldo líquido caso o resgate ocorra no mês $t$ |

---

## ✨ Funcionalidades

- Parâmetros ajustáveis: capital inicial, Selic projetada, mês e ano de início e número de meses (de 1 a 360).
- **Tabela mensal** com saldo bruto, rendimento, alíquota de IR, IR e saldo líquido "se resgatar".
- **Gráfico de evolução** comparando o saldo bruto e o saldo líquido.
- **Aba Metodologia** explicando cada etapa do cálculo.
- Resultado final do resgate: saldo bruto, IR sobre o ganho total, saldo líquido e retorno líquido.
- Faixa de alíquotas em destaque, com cores por prazo.
- Valores no padrão brasileiro (`R$ 1.234,56`).
- Layout responsivo: computador, tablet e celular.

---

## 🧮 Fórmulas utilizadas

### Taxa do fundo

Considera-se $\mathrm{CDI} = \mathrm{Selic} - 0{,}10\%$ ao ano, com a Selic constante:

$$
i_f = \frac{S}{100} - 0{,}001
$$

### Taxa diária e fator mensal

A taxa anual é convertida para base de **252 dias úteis** e capitalizada por **21 dias úteis** em cada mês:

$$
i_d = (1 + i_f)^{1/252} - 1
\qquad
F = (1 + i_d)^{21}
$$

### Saldo bruto

O saldo cresce sem desconto de IR mês a mês:

$$
B_t = B_{t-1}\cdot F, \qquad B_0 = C_0
$$

### IR e saldo líquido

O IR incide apenas sobre o ganho total, no resgate:

$$
G_t = B_t - C_0
\qquad
\mathrm{IR}_t = \max(0,\,G_t)\cdot\alpha_t
\qquad
L_t = B_t - \mathrm{IR}_t
$$

### Retorno líquido

$$
R = \frac{L_t}{C_0} - 1
$$

### Resumo

| Grandeza | Fórmula |
|:--------:|:-------:|
| $i_f$ | $\dfrac{S}{100} - 0{,}001$ |
| $i_d$ | $(1+i_f)^{1/252} - 1$ |
| $F$ | $(1+i_d)^{21}$ |
| $B_t$ | $C_0\cdot F^{\,t}$ |
| $\mathrm{IR}_t$ | $\max(0,\,B_t - C_0)\cdot\alpha_t$ |
| $L_t$ | $B_t - \mathrm{IR}_t$ |

---

## 🧾 Imposto de Renda regressivo

A alíquota depende dos **dias corridos** desde a aplicação, contados como **30 dias por mês**:

| Prazo | Dias corridos | Alíquota |
|:-----:|:-------------:|:--------:|
| Até 6 meses | até 180 | 22,5% |
| 6 a 12 meses | 181 a 360 | 20,0% |
| 1 a 2 anos | 361 a 720 | 17,5% |
| Acima de 2 anos | acima de 720 | 15,0% |

---

## ✏️ Exemplo

Os valores abaixo podem ser digitados diretamente no simulador para conferência: capital inicial de **R$ 50.000,00** e Selic projetada de **13,50% a.a.** (taxa do fundo de 13,40% a.a.).

| Prazo | Saldo bruto | Ganho bruto | Alíquota | IR | Saldo líquido | Retorno líquido |
|:-----:|------------:|------------:|:--------:|---:|--------------:|:---------------:|
| 6 meses | R$ 53.244,72 | R$ 3.244,72 | 22,5% | R$ 730,06 | R$ 52.514,66 | 5,03% |
| 12 meses | R$ 56.700,00 | R$ 6.700,00 | 20,0% | R$ 1.340,00 | R$ 55.360,00 | 10,72% |
| 24 meses | R$ 64.297,80 | R$ 14.297,80 | 17,5% | R$ 2.502,11 | R$ 61.795,68 | 23,59% |
| 36 meses | R$ 72.913,71 | R$ 22.913,71 | 15,0% | R$ 3.437,06 | R$ 69.476,65 | 38,95% |

Conferência do caso de 12 meses (252 dias úteis):

$$
B_{12} = 50000\cdot(1{,}134)^{1} = 56700
\qquad
\mathrm{IR} = 6700\cdot 0{,}20 = 1340
\qquad
L_{12} = 55360
$$

---

## 🚀 Como usar

### Localmente

1. Baixe o arquivo `index.html` (ou clone o repositório):
   ```bash
   git clone https://github.com/SEU-USUARIO/NOME-DO-REPOSITORIO.git
   ```
2. Abra o arquivo no navegador (duplo clique ou `Arquivo → Abrir`).
3. Informe o capital inicial, a Selic projetada, o início e o número de meses.
4. Consulte a tabela, o gráfico, a metodologia e o resultado final.

Não há instalação nem build. O arquivo carrega React, Babel e Tailwind por CDN, então é necessária **conexão com a internet** na primeira abertura.

### Online com GitHub Pages

1. No repositório, vá em **Settings → Pages**.
2. Em **Build and deployment**, selecione **Deploy from a branch**.
3. Escolha a branch `main` e a pasta `/ (root)` e clique em **Save**.
4. Mantenha o arquivo com o nome `index.html` para que o site abra direto no simulador.
5. Após alguns instantes, o simulador estará disponível em:
   ```
   https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/
   ```

---

## ⚠️ Premissas e limitações

- A Selic é considerada **constante** durante todo o período.
- Não há aportes: o capital é aplicado uma única vez.
- Não são considerados IOF, taxa de custódia ou outras taxas.
- Cada mês tem **21 dias úteis** e **30 dias corridos** (este último para definir a alíquota).
- As colunas "IR (se resgatar)" e "Saldo Líq. (se resgatar)" são **simulações de resgate**, e não descontos efetivos: o saldo bruto continua rendendo normalmente.
- Os valores reais dependem do emissor, do produto e da data exata do resgate.
- A ferramenta tem finalidade **didática**; não constitui recomendação de investimento.

---

## 🛠️ Tecnologias

- HTML5 e CSS3
- [React](https://react.dev/) 18 (via CDN)
- [Babel Standalone](https://babeljs.io/docs/babel-standalone) para compilar JSX no navegador
- [Tailwind CSS](https://tailwindcss.com/) (via CDN)
- Gráfico em SVG puro, sem biblioteca de gráficos

---

## 📁 Estrutura sugerida do repositório

```
├── index.html     # o simulador
├── README.md      # este arquivo
└── LICENSE        # licença (opcional)
```

---

## 🔗 Projeto relacionado

Este simulador tem uma versão irmã, o **Simulador de Renda Fixa com Aportes**, que permite aportes periódicos reajustáveis e calcula o IR regressivo **por lote**, de acordo com o prazo de cada aporte.

---

## 👤 Autoria

**Prof. Dr. Laerte Bemm**
Departamento de Matemática — Universidade Estadual de Maringá (UEM)

---

## 📄 Licença

Defina a licença de sua preferência (por exemplo, [MIT](https://opensource.org/licenses/MIT)) e adicione o arquivo `LICENSE` ao repositório. Para uso estritamente acadêmico, você também pode indicar aqui: *"Material educacional de uso livre, com citação da autoria."*
