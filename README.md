# Versionamento de Dados e Modelos com DVC e Git (`aula-ml`)

Projeto da Atividade Prática 07 demonstrando o controle de versão reprodutível de datasets, partições de treino/teste, modelos e métricas utilizando **Git** e **DVC**.

## Estrutura do Projeto

- `data/chamados.csv` (rastreado via `data/chamados.csv.dvc`): dataset sintético de classificação de chamados (`comum` vs. `urgente`).
- `data/split.csv` (rastreado via `data/split.csv.dvc`): partição fixa entre `treino` e `teste`.
- `data/manifesto.json`: metadados do dataset, esquema, regras de rotulagem e histórico de alterações.
- `params.json`: hiperparâmetros do tokenizador (`primeira_palavra_minuscula`).
- `src/train.py`: script de treinamento, avaliação e geração de manifestos rastreáveis.
- `models/model.json` (rastreado via `models/model.json.dvc`): artefato do modelo treinado.
- `reports/metrics.json` e `reports/modelo.json`: métricas de avaliação e manifesto de linhagem do modelo.
- `reports/comparacao.md`: comparativo entre as versões `r1` (`chamados-v1`) e `r2` (`chamados-v2`) e evidência de restauração.

## Tags e Versões

- `dados-v1`: snapshot inicial do dataset (`chamados-v1`) com erro intencional no rótulo do ID `006`.
- `modelo-r1`: primeiro modelo (`chamados-r1`) treinado sobre `dados-v1`.
- `dados-v2`: dataset corrigido (`chamados-v2`) após ajuste do rótulo do ID `006` para `urgente`.
- `modelo-r2`: segundo modelo (`chamados-r2`) treinado sobre `dados-v2` mantendo a mesma partição de teste.

## Como Reproduzir / Restaurar uma Versão

1. Clonar o repositório e alternar para a tag desejada (exemplo com `modelo-r1`):

       git clone https://github.com/Eduardo-Loiola/aula-ml.git
       cd aula-ml
       git switch --detach modelo-r1

2. Configurar o armazenamento remoto DVC e baixar os dados e o artefato do modelo:

       dvc pull

3. Treinar novamente a partir dos dados restaurados:

       python src/train.py chamados-r1
