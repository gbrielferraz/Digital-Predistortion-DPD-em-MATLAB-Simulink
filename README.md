# Digital Predistortion (DPD) em MATLAB/Simulink

Repositório destinado à documentação, simulação e desenvolvimento do projeto de **Iniciação Científica (IC)** relacionado à **Pré-Distorção Digital (Digital Predistortion — DPD)** aplicada a amplificadores de potência (PA) em sistemas de comunicação.

O projeto é desenvolvido utilizando **MATLAB/Simulink** como ambiente principal de modelagem e simulação.

> **Projeto de pesquisa — IC/DPD**
> Universidade Federal de Mato Grosso do Sul (UFMS)
> Engenharia Elétrica

---

## 📌 Sobre o projeto

Amplificadores de potência (Power Amplifiers — PAs) são elementos fundamentais em sistemas de transmissão sem fio. Entretanto, quando operados próximos de sua região de maior potência e eficiência, podem apresentar **comportamento não linear**, causando distorções de amplitude e fase e a geração de componentes espectrais indesejadas.

A **Digital Predistortion (DPD)** é uma técnica utilizada para compensar, digitalmente, parte dessas não linearidades.

A ideia fundamental pode ser representada por:

```text
Sinal digital
     │
     ▼
   DPD
     │
     ▼
   DAC/RF
     │
     ▼
    PA
     │
     ▼
 Sinal de saída
```

A DPD modifica previamente o sinal de entrada de forma que, após atravessar o PA não linear, a saída apresente comportamento mais próximo do desejado.

---

## 🎯 Objetivo

O objetivo desta etapa do projeto é desenvolver uma base de conhecimento e simulação para estudar:

* comportamento de sinais em uma cadeia de transmissão;
* análise espectral;
* filtragem;
* translação de frequência;
* amplificação linear;
* não linearidade de amplificadores de potência;
* compressão de ganho;
* distorção AM/AM;
* distorção AM/PM;
* geração de harmônicos;
* expansão espectral (*spectral regrowth*);
* fundamentos da Digital Predistortion (DPD).

A partir dessa base, o projeto poderá avançar para a **modelagem, identificação e implementação de algoritmos de DPD**, incluindo posteriormente sua aplicação em hardware/FPGA.

---

# 🧪 Metodologia

O roteiro experimental originalmente proposto para uma ferramenta de simulação RF foi adaptado para **MATLAB/Simulink**.

A sequência adotada foi:

```text
Sinal de entrada
      │
      ▼
Análise espectral
      │
      ▼
Filtro pós-DAC
      │
      ▼
Mixer / Translação de frequência
      │
      ▼
Filtro RF
      │
      ▼
PA linear
      │
      ▼
PA não linear
      │
      ▼
Análise de distorção
      │
      ▼
Estudo da DPD
```

As simulações foram construídas inicialmente utilizando modelos comportamentais, permitindo observar os principais fenômenos sem exigir uma modelagem eletromagnética detalhada dos componentes RF.

---

# 📚 Atividades desenvolvidas

## Atividade 1 — Tom único e análise espectral

Estudo inicial de um sinal senoidal e de sua representação no domínio da frequência.

### Conceitos

* frequência fundamental;
* amplitude;
* valor RMS;
* potência;
* impedância de referência de 50 Ω;
* FFT;
* Spectrum Analyzer.

### Objetivo

Compreender como um sinal senoidal ideal aparece no domínio da frequência e estabelecer uma base para as análises posteriores.

---

## Atividade 2 — Filtro após o DAC

Estudo do papel do filtro de reconstrução após a conversão digital-analógica.

### Conceitos

* DAC;
* frequência de amostragem;
* imagens espectrais;
* filtro passa-baixas;
* banda passante;
* rejeição de componentes indesejadas.

### Objetivo

Compreender por que a filtragem é necessária antes da etapa de translação para RF.

> Nesta implementação, o DAC não foi modelado em nível de circuito. O filtro representa comportamentalmente a função de reconstrução.

---

## Atividade 3 — Mixer

Modelagem comportamental da translação de frequência utilizando multiplicação entre o sinal e um oscilador local.

[
y(t)=x(t)\cos(2\pi f_{LO}t)
]

O processo produz componentes associadas às frequências:

[
f_{LO}+f_{BB}
]

e

[
f_{LO}-f_{BB}
]

### Conceitos

* oscilador local;
* mixer;
* banda lateral superior;
* banda lateral inferior;
* translação de frequência;
* frequência imagem.

---

## Atividade 4 — Filtro passa-faixa

Estudo da seleção da banda desejada após a etapa de mistura.

### Objetivo

Observar como o filtro pode selecionar uma componente espectral e atenuar componentes indesejadas.

A atividade também introduz a importância da filtragem antes da amplificação de potência.

---

## Atividade 5 — PA linear

Modelagem inicial de um amplificador de potência ideal utilizando um ganho linear.

### Conceitos

* ganho;
* dB;
* amplificação;
* potência;
* linearidade;
* relação entrada/saída.

No modelo ideal:

[
y(t)=Gx(t)
]

O ganho altera o nível do sinal, mas não cria novas componentes espectrais.

---

## Atividade 6 — PA não linear

Introdução da não linearidade por meio de um modelo polinomial comportamental.

Um dos modelos utilizados foi:

[
y=x-0,3x^3
]

Esse modelo permite observar a geração de componentes harmônicas e a compressão da componente fundamental.

Para uma entrada senoidal:

[
x(t)=A\sin(\omega t)
]

o termo cúbico produz uma componente em (3\omega).

### Fenômenos observados

* compressão de ganho;
* geração de terceiro harmônico;
* aumento da distorção com a amplitude;
* comportamento não linear;
* relação entre amplitude de entrada e saída.

---

## Atividade 7 — Cadeia completa

Integração das etapas anteriores em uma cadeia de transmissão comportamental.

```text
Baseband
   │
   ▼
Lowpass Filter
   │
   ▼
Mixer
   │
   ▼
Bandpass Filter
   │
   ▼
PA
   │
   ▼
Spectrum Analyzer
```

A atividade permite observar a interação entre filtragem, translação de frequência e amplificação.

As frequências utilizadas nesta etapa foram reduzidas em relação a um sistema RF real para tornar a simulação mais simples e eficiente.

---

## Atividade 8 — Não linearidade e DPD

Etapa de introdução ao problema central da pesquisa.

O PA não linear pode apresentar:

* AM/AM;
* AM/PM;
* compressão;
* geração de harmônicos;
* produtos de intermodulação;
* expansão espectral.

A DPD é introduzida como uma técnica para compensar previamente o comportamento não linear do PA.

### Conceito

```text
                 ┌─────────────┐
Sinal ─────────► │     DPD     │
                 └──────┬──────┘
                        │
                        ▼
                 ┌─────────────┐
                 │     PA      │
                 │ não linear  │
                 └──────┬──────┘
                        │
                        ▼
                     Saída
```

O objetivo é fazer com que:

[
\text{DPD}+\text{PA}
\approx
\text{comportamento linear}
]

---

# 📁 Organização do repositório

A estrutura proposta para o repositório é:

```text
IC-DPD-Simulink/
│
├── README.md
│
├── docs/
│   ├── roteiro/
│   ├── respostas/
│   ├── simulacoes/
│   └── referencias/
│
├── atividade_01/
│   ├── simulink/
│   ├── resultados/
│   └── README.md
│
├── atividade_02/
│   ├── simulink/
│   ├── resultados/
│   └── README.md
│
├── atividade_03/
│   ├── simulink/
│   ├── resultados/
│   └── README.md
│
├── atividade_04/
│   ├── simulink/
│   ├── resultados/
│   └── README.md
│
├── atividade_05/
│   ├── simulink/
│   ├── resultados/
│   └── README.md
│
├── atividade_06/
│   ├── simulink/
│   ├── resultados/
│   └── README.md
│
├── atividade_07/
│   ├── simulink/
│   ├── resultados/
│   └── README.md
│
├── atividade_08/
│   ├── simulink/
│   ├── resultados/
│   └── README.md
│
├── scripts/
│   ├── parametros/
│   ├── analise/
│   └── graficos/
│
└── resultados/
    ├── figuras/
    ├── espectros/
    └── tabelas/
```

A estrutura pode ser ajustada conforme a quantidade de arquivos crescer.

---

# 📄 Documentação

Além dos arquivos `.slx`, o repositório contém documentação para permitir que as simulações sejam reproduzidas e compreendidas.

Entre os materiais estão:

* roteiro experimental;
* respostas às perguntas do roteiro;
* documentação das simulações;
* parâmetros utilizados;
* resultados;
* gráficos;
* observações sobre problemas encontrados;
* explicações teóricas;
* materiais utilizados como base para a pesquisa.

---

# ⚙️ Ferramentas

### MATLAB / Simulink

Ambiente utilizado para:

* construção dos modelos;
* processamento dos sinais;
* análise espectral;
* implementação dos blocos de RF;
* modelagem das não linearidades;
* análise dos resultados.

### Principais conceitos/blocos utilizados

* Sine Wave;
* Gain;
* Product;
* Lowpass Filter;
* Bandpass Filter;
* Memoryless Nonlinearity;
* Polynomial Nonlinearity;
* Spectrum Analyzer;
* Real-to-Complex;
* análise FFT.

---

# 📊 Principais resultados

As simulações permitiram observar progressivamente a transformação do comportamento de um sistema linear para um sistema não linear.

### Sistema linear

```text
Entrada
   │
   ▼
   PA linear
   │
   ▼
Mesma frequência
Maior amplitude
```

### Sistema não linear

```text
Entrada
   │
   ▼
PA não linear
   │
   ├──► Compressão
   ├──► AM/AM
   ├──► AM/PM
   ├──► Harmônicos
   └──► Expansão espectral
```

Esse comportamento fornece a motivação para a utilização da DPD.

---

# ⚠️ Limitações da modelagem atual

É importante destacar que as simulações atuais possuem caráter **comportamental e didático**.

Elas não representam integralmente um front-end RF físico.

Entre as principais limitações estão:

* ausência de modelagem eletromagnética dos componentes;
* ausência de parasitas;
* ausência de caracterização física de um PA específico;
* modelos simplificados de não linearidade;
* ausência de memória em alguns modelos;
* sinais simplificados em determinadas atividades;
* utilização de frequências reduzidas em algumas simulações;
* ausência, nesta etapa, de uma implementação completa de DPD em FPGA.

Portanto, os resultados devem ser interpretados como parte do processo de desenvolvimento e validação conceitual da pesquisa.

---

# 🚧 Próximas etapas

O desenvolvimento posterior do projeto deverá avançar da demonstração dos fenômenos para a modelagem efetiva do PA e da DPD.

Possíveis etapas incluem:

1. geração de sinais de comunicação de banda larga;
2. utilização de sinais OFDM;
3. caracterização AM/AM e AM/PM;
4. estudo de efeitos de memória;
5. identificação de modelos comportamentais do PA;
6. implementação de algoritmos de DPD;
7. treinamento/estimação dos parâmetros;
8. avaliação de EVM e ACPR/ACLR;
9. comparação entre sinal original e sinal pré-distorcido;
10. investigação da implementação em FPGA;
11. integração com técnicas de inteligência artificial, quando aplicável.

---

# 📖 Documentação das simulações

O objetivo deste repositório não é apenas armazenar arquivos `.slx`.

Cada simulação deve ser acompanhada, sempre que possível, de:

* objetivo;
* fundamentação teórica;
* diagrama;
* parâmetros;
* procedimento;
* resultados;
* interpretação;
* problemas encontrados;
* solução adotada;
* limitações;
* conclusão.

Dessa forma, o repositório funciona como um **caderno técnico digital da pesquisa**, permitindo acompanhar a evolução do projeto.

---

# 👨‍🔬 Projeto de Iniciação Científica

**Universidade Federal de Mato Grosso do Sul — UFMS**
**Curso:** Engenharia Elétrica
**Projeto:** Sistema 6G — Pré-Distorção Digital sintetizada em FPGA com IA
**Área:** Comunicações / RF / Processamento Digital de Sinais / Sistemas Não Lineares

---

## 📌 Status

🟢 **Etapa atual:** Modelagem e simulação do front-end RF e estudo da não linearidade do PA.

🔄 **Em desenvolvimento:** Modelagem e implementação da DPD.

🚧 **Etapa futura:** Avaliação e possível implementação em FPGA.

---

## 📜 Licença

Este repositório possui finalidade acadêmica e de pesquisa.

Os códigos, modelos e documentos podem ser utilizados para estudo e reprodução das simulações, respeitando as referências e materiais utilizados no desenvolvimento do projeto.
