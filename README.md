# CPFL Paulista: manifestações sobre falta de energia

Análise exploratória dos dados públicos da ANEEL sobre manifestações relacionadas à falta de energia na CPFL Paulista em 2025, desenvolvida com Python, pandas e matplotlib.

O projeto conecta engenharia elétrica, estatística e análise de dados. Seu desenvolvimento é gradual, com foco no aprendizado e na interpretação dos resultados.

## Objetivo

O objetivo geral é investigar a relação entre a continuidade do fornecimento de energia e o volume de manifestações dos consumidores.

Nesta primeira etapa, a pergunta é:

> Como variou, ao longo de 2025, o volume de manifestações sobre falta de energia na CPFL Paulista?

A comparação com os indicadores DEC e FEC está prevista para uma próxima etapa e ainda não foi realizada.

## Recorte da análise

| Elemento | Seleção |
|---|---|
| Distribuidora | CPFL Paulista |
| Período | Janeiro a dezembro de 2025 |
| Categoria | Interrupção no Fornecimento - Falta de energia |
| Canais | Nível 1 — SAC e nível 2 — Ouvidoria |
| Abrangência | Registros da distribuidora presentes na base, sem filtro por município |
| Medida analisada | Soma de `QtdManifestacoesRecebidas` por `MesCompetencia` |

Embora Americana (SP) tenha motivado a escolha da distribuidora, os resultados desta etapa abrangem a CPFL Paulista, e não apenas o município.

## Fonte dos dados

Os dados são provenientes do conjunto **Manifestações no 1º e 2º nível da Distribuidora**, publicado pela ANEEL.

- [Página oficial do conjunto de dados](https://dadosabertos.aneel.gov.br/dataset/manifestacoes-no-1o-e-2o-niveis-da-distribuidora)
- [Download do arquivo de 2025 em Parquet](https://dadosabertos.aneel.gov.br/dataset/364859a2-7cb8-45ea-9c88-b4392516a6ba/resource/a9ea21c7-1b2b-4c91-97b4-3d9732c77cec/download/manifestacoes-1-2-niveis-distribuidora-2025.parquet)
- [Dicionário de dados da ANEEL](https://dadosabertos.aneel.gov.br/dataset/364859a2-7cb8-45ea-9c88-b4392516a6ba/resource/928f6910-1a40-41a2-8bf1-7965a5fe4ca7/download/dm-manifestacoes-nos-1-e-2-niveis-da-distribuidora.pdf)

O arquivo utilizado no notebook é `manifestacoes-1-2-niveis-distribuidora-2025.parquet`. A ANEEL pode atualizar os arquivos; uma nova versão pode produzir resultados diferentes dos registrados aqui.

## Ferramentas

- **Python:** linguagem utilizada na análise.
- **pandas:** leitura, seleção, conversão de tipos e agrupamento dos dados.
- **matplotlib:** visualização do volume mensal.
- **PyArrow:** suporte à leitura do formato Parquet.
- **Jupyter no VS Code:** ambiente de desenvolvimento do notebook.

## Etapas realizadas

1. Leitura do arquivo Parquet e exploração das colunas.
2. Seleção dos registros identificados como `CPFL Paulista`.
3. Seleção da categoria `Interrupção no Fornecimento - Falta de energia`.
4. Conversão da quantidade recebida e do mês de competência para valores numéricos.
5. Soma das quantidades por mês e ordenação cronológica.
6. Construção de um gráfico de barras.
7. Cálculo da variação percentual entre agosto e setembro.
8. Conferência de linhas totalmente duplicadas e dos níveis de atendimento presentes.

O filtro da CPFL Paulista retornou **244.185 linhas**. A seleção da categoria de falta de energia retornou **13.904 linhas**. Esses números representam linhas da base agregada, não a quantidade de manifestações.

Na seleção analisada, não foram encontradas linhas totalmente idênticas em todas as colunas. Essa conferência não identifica, por si só, registros diferentes relacionados a uma mesma situação.

## Resultados

| Mês | Manifestações recebidas |
|---|---:|
| Janeiro | 41.526 |
| Fevereiro | 80.913 |
| Março | 95.017 |
| Abril | 63.380 |
| Maio | 53.024 |
| Junho | 63.917 |
| Julho | 61.392 |
| Agosto | 58.806 |
| Setembro | 155.850 |
| Outubro | 114.769 |
| Novembro | 97.969 |
| Dezembro | 119.419 |

Setembro apresentou o maior volume no recorte analisado. Em relação a agosto, houve aumento de **165,02%**.

O cálculo utiliza agosto como base da comparação:

```text
Variação percentual = ((setembro - agosto) / agosto) × 100
                   = ((155.850 - 58.806) / 58.806) × 100
                   ≈ 165,02%
```

Outubro, novembro e dezembro também apresentaram volumes superiores ao de agosto.

## Interpretação e limitações

- Os valores representam **manifestações recebidas**, não consumidores únicos ou quantidade de interrupções.
- A soma reúne SAC e Ouvidoria. Uma mesma situação pode gerar registros nos dois níveis de atendimento.
- O recorte utiliza uma categoria específica. Categorias como `Interrupção Frequente do Fornecimento` e `Interrupção Programada` não foram incluídas.
- Os totais não foram normalizados pela quantidade de unidades consumidoras.
- A análise é descritiva: não foram realizados testes de hipótese, modelos estatísticos ou avaliação de causalidade.
- O aumento observado não permite concluir, isoladamente, que as interrupções cresceram na mesma proporção, nem determinar sua causa.

## Como executar

1. Baixe o notebook `01_exploracao_reclamacoes.ipynb` deste repositório.
2. Baixe o arquivo Parquet de 2025 no link oficial acima e coloque-o na **mesma pasta do notebook**.
3. Abra essa pasta no VS Code, com as extensões Python e Jupyter instaladas.
4. Abra o notebook e selecione seu ambiente Python como kernel.
5. Execute a célula de instalação das bibliotecas:

   ```python
   %pip install pandas matplotlib pyarrow
   ```

6. Execute as demais células na ordem em que aparecem. Caso seja solicitado após a instalação, reinicie o kernel antes de continuar.

A leitura dos dados utiliza o nome do arquivo local:

```python
df = pd.read_parquet("manifestacoes-1-2-niveis-distribuidora-2025.parquet")
```

## Próxima etapa: DEC e FEC

O estudo continuará com a incorporação dos indicadores de continuidade:

- **DEC:** Duração Equivalente de Interrupção por Unidade Consumidora.
- **FEC:** Frequência Equivalente de Interrupção por Unidade Consumidora.

A pergunta será se os meses com maior duração ou frequência de interrupções também apresentam maior volume de manifestações sobre falta de energia.

Antes do cruzamento, será necessário compatibilizar os períodos e a abrangência geográfica das bases, considerando que os indicadores podem ser apresentados por conjuntos de unidades consumidoras.

[Fonte prevista: ANEEL — Indicadores Coletivos de Continuidade (DEC e FEC)](https://dadosabertos.aneel.gov.br/pt_BR/dataset/indicadores-coletivos-de-continuidade-dec-e-fec)

## Status

**Primeira etapa exploratória concluída. Cruzamento com DEC e FEC pendente.**

Estudo independente para aprendizado e portfólio, elaborado a partir de dados públicos.
