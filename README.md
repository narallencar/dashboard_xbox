# 🎮 Dashboard de Vendas — Xbox Game Pass

Dashboard interativo em **Excel** que analisa as vendas de assinaturas do Xbox Game Pass em 2024: faturamento por tipo de renovação, receita dos complementos (**EA Play** e **Minecraft Season Pass**) e filtro por tipo de cobrança.

<img width="1892" height="999" alt="image" src="https://github.com/user-attachments/assets/b7a97e47-350b-4f4d-bb7c-8edf62dd1858" />

> ⚠️ Os dados são fictícios e foram criados apenas para fins de estudo e portfólio.

---

## 📌 Sumário

- [Objetivo](#-objetivo)
- [Perguntas de negócio](#-perguntas-de-negócio)
- [Principais números](#-principais-números)
- [Estrutura do arquivo](#-estrutura-do-arquivo)
- [Dicionário de dados](#-dicionário-de-dados)
- [Regra de cálculo do Total Value](#-regra-de-cálculo-do-total-value)
- [Como o dashboard funciona](#-como-o-dashboard-funciona)
- [Identidade visual](#-identidade-visual)
- [Como usar](#-como-usar)
- [Ferramentas](#-ferramentas)
- [Pontos de atenção e melhorias](#-pontos-de-atenção-e-melhorias)
- [Autoria](#-autoria)

---

## 🎯 Objetivo

Transformar uma base de **295 assinaturas** (01/01/2024 a 16/12/2024) em um painel de leitura rápida, que responde a perguntas de faturamento e permite filtrar tudo por tipo de assinatura (mensal, trimestral ou anual) com um único clique.

---

## ❓ Perguntas de negócio

| # | Pergunta | Resposta |
|---|----------|----------|
| 1 | Qual o faturamento total de vendas de planos **anuais** (todas as assinaturas agregadas)? |
| 2 | Qual o faturamento dos planos anuais **separado por auto renovação** (com e sem)? | 

> Para reproduzir: no dashboard, selecione **Annual** na segmentação *Subscription Type*.

---

## 📊 Principais números

Considerando a base completa (sem filtro na segmentação):

| Indicador | Valor |
|-----------|-------|
| Assinaturas | 295 |
| Faturamento total (`Total Value`) | R$ 7.633 |
| EA Play Season Pass | R$ 2.940 |
| Minecraft Season Pass | R$ 3.880 |
| Descontos (cupons) | R$ 2.122 |

**Por plano**

| Plano | Assinaturas | Faturamento |
|-------|------------:|------------:|
| Core | 101 | R$ 444 |
| Standard | 96 | R$ 1.801 |
| Ultimate | 98 | R$ 5.388 |

**Por tipo de assinatura**

| Tipo | Assinaturas | Faturamento |
|------|------------:|------------:|
| Monthly | 139 | R$ 3.571 |
| Quarterly | 85 | R$ 2.308 |
| Annual | 71 | R$ 1.754 |

**Por auto renovação**

| Auto Renewal | Assinaturas | Faturamento |
|--------------|------------:|------------:|
| Yes | 148 | R$ 3.786 |
| No | 147 | R$ 3.847 |

---

## 🗂️ Estrutura do arquivo

O arquivo `dashboard_xbox_finalizado.xlsx` tem 4 abas. Apenas o **Dashboard** fica visível; as demais ficam ocultas para manter a apresentação limpa.

| Aba | Visibilidade | Conteúdo |
|-----|--------------|----------|
| **Assets** | Oculta | Paleta de cores, logos e ícones usados no layout |
| **Bases** | Oculta | Base de dados formatada como tabela (`Tabela1`, intervalo `A1:M296`) |
| **Cálculos** | Oculta | Perguntas de negócio, 3 tabelas dinâmicas, fórmulas `GETPIVOTDATA` e a segmentação de dados |
| **Dashboard** | Visível | Painel final: cabeçalho, segmentação, cartões de KPI e gráfico |

---

## 🧾 Dicionário de dados

Aba **Bases** — uma linha por assinatura.

| Coluna | Descrição | Valores observados |
|--------|-----------|--------------------|
| `Subscriber ID` | Identificador único da assinatura | 3231 a 3525 |
| `Name` | Nome do assinante | Texto |
| `Plan` | Plano contratado | Core, Standard, Ultimate |
| `Start Date` | Data de início da assinatura | 01/01/2024 a 16/12/2024 |
| `Auto Renewal` | Renovação automática ativa? | Yes, No |
| `Subscription Price` | Preço base do plano | 5, 10, 15 |
| `Subscription Type` | Periodicidade da cobrança | Monthly, Quarterly, Annual |
| `EA Play Season Pass` | Contratou o EA Play? | Yes, No |
| `EA Play Season Pass Price` | Valor do EA Play | 30 ou `-` |
| `Minecraft Season Pass` | Contratou o Minecraft Season Pass? | Yes, No |
| `Minecraft Season Pass Price` | Valor do Minecraft Season Pass | 20 ou 0 |
| `Coupon Value` | Desconto aplicado por cupom | 0 a 20 |
| `Total Value` | Valor final da assinatura | Calculado (ver abaixo) |

---

## 🧮 Regra de cálculo do Total Value

```
Total Value = Subscription Price + EA Play Season Pass Price + Minecraft Season Pass Price − Coupon Value
```

Esta regra vale para **as 295 linhas** da base.

| Item | Valor |
|------|------:|
| Plano Core | R$ 5 |
| Plano Standard | R$ 10 |
| Plano Ultimate | R$ 15 |
| EA Play Season Pass | R$ 30 |
| Minecraft Season Pass | R$ 20 |

**Padrões da base:** o EA Play aparece somente nas assinaturas Ultimate; o Minecraft Season Pass aparece em todas as assinaturas Standard e Ultimate e em nenhuma Core.

---

## 🖥️ Como o dashboard funciona

**Tabelas dinâmicas** (aba Cálculos), todas alimentadas pela `Tabela1`:

| Tabela dinâmica | O que mostra |
|-----------------|--------------|
| `tbl_annual_total` | Soma de `Total Value` por `Auto Renewal` |
| `TBL_some` | Soma de `EA Play Season Pass Price` por `Plan` |
| `tbl_mini` | Soma de `Minecraft Season Pass Price` por `Plan` |

**Segmentação de dados:** o filtro **Subscription Type** (Monthly, Quarterly, Annual) está ligado às três tabelas dinâmicas ao mesmo tempo. Ao clicar nele, todos os números e o gráfico do dashboard se atualizam juntos. Existem duas segmentações (uma em Cálculos e outra no Dashboard) compartilhando o mesmo filtro.

**Elementos do Dashboard:**

- Cabeçalho **XBOX GAME PASS SUBSCRIPTIONS SALES**, com período de cálculo (01/01/2024 – 31/12/2024) e data de atualização
- Segmentação **Subscription Type**
- Cartões de KPI com o total de **EA Play Season Pass** e de **Minecraft Season Pass**, alimentados por fórmulas `GETPIVOTDATA`
- Gráfico de barras com o faturamento por auto renovação

---

## 🎨 Identidade visual

Paleta definida na aba Assets, inspirada no verde do Xbox:

| Cor | Hex | Uso |
|-----|-----|-----|
| Verde Xbox | `#9BC848` | Cor principal |
| Verde Xbox | `#22C55E` | Cor principal |
| Verde menu | `#2AE6B1` | Menus |
| Verde menu | `#5BF6A8` | Menus e barras do gráfico |
| Cinza claro | `#E8E6E9` | Zona negativa / fundo |

---

## ▶️ Como usar

1. Abra o arquivo no **Excel 2010 ou superior** (necessário para a segmentação de dados).
2. Na aba **Dashboard**, clique em um tipo de assinatura na segmentação para filtrar os resultados. Para limpar o filtro, use o ícone de funil com **X** no canto da segmentação.
3. Para ver as abas ocultas: clique com o botão direito em qualquer aba → **Reexibir**.
4. Se alterar ou acrescentar dados na aba **Bases**, atualize tudo em **Dados → Atualizar Tudo**.

---

## 🛠️ Ferramentas

- Microsoft Excel
  - Tabelas (`Tabela1`)
  - Tabelas Dinâmicas
  - Segmentação de Dados
  - Função `GETPIVOTDATA`
  - Gráficos de barras
  - Formas, imagens e ícones para o layout

---

## 🔍 Pontos de atenção e melhorias

- **`Total Value` está como valor fixo**, não como fórmula. Transformá-lo em coluna calculada (regra acima) evita divergências quando a base crescer.
- **`EA Play Season Pass Price` usa `-` (texto)** quando não há contratação. Trocar por `0` deixa a coluna 100% numérica.
- **O preço não varia com o tipo de assinatura:** o mesmo plano custa o mesmo valor em Monthly, Quarterly e Annual. Por isso o faturamento "anual" não é 12× o mensal. Vale revisar se isso reflete a regra de negócio desejada.
- **Nomes repetidos com IDs diferentes:** há 15 linhas com nome repetido (por exemplo, *Geraldo Ribeiro*, IDs 3324 e 3349). Confirme se são clientes distintos ou duplicidade.
- **Período do cabeçalho:** o dashboard informa 01/01/2024 – 31/12/2024, mas a última assinatura da base é de 16/12/2024.
- **Próximos passos possíveis:** evolução mensal de assinaturas, ticket médio por plano, taxa de contratação dos complementos e uso do valor dos cupons.

---
