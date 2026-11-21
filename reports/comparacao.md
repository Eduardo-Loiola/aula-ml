# Comparação entre Modelos (`r1` vs. `r2`) e Evidência de Restauração

## 1. Resumo das Versões

| Atributo | Modelo `r1` (`chamados-r1`) | Modelo `r2` (`chamados-r2`) |
| :--- | :--- | :--- |
| **Tag Git** | `modelo-r1` | `modelo-r2` |
| **Dataset** | `chamados-v1` (`dados-v1`) | `chamados-v2` (`dados-v2`) |
| **Alteração nos Dados** | Snapshot inicial (ID `006` rotulado como `comum`) | Correção do ID `006` de `comum` para `urgente` |
| **SHA-256 Dataset (`data/chamados.csv`)** | `bc3fd86a3a5f598841a58839a9726d901fa7e8361ffe79c5b43873ad8a1328ac` | `4ca2e29cb930a9161c2a851b3c71f5529dbb812db607ee4a5ea95d2223baa14d` |
| **SHA-256 Partição (`data/split.csv`)** | `2ef820171c1a4625fe216e6da67c7c8dcf9d97a08288c0239cd23f1a6961d981` | `2ef820171c1a4625fe216e6da67c7c8dcf9d97a08288c0239cd23f1a6961d981` |
| **SHA-256 Modelo (`models/model.json`)** | `72a3a67936139a3473838a6dcfa7aae1acb4c23467be1ef1056d51d485b94840` | `7f9d94ca8dd86ccc8ad90b0839613c61a33e2168bd23bf083a4bbe1375f908ac` |

## 2. Comparação de Desempenho (Partição de Teste Fixa)

| Métrica | `chamados-r1` | `chamados-r2` | Variação |
| :--- | :--- | :--- | :--- |
| **Acurácia Geral** | `0.75` (3/4) | `1.00` (4/4) | `+0.25` |
| **Recall — Classe `comum`** | `1.00` (2/2) | `1.00` (2/2) | `0.00` |
| **Recall — Classe `urgente`** | `0.50` (1/2) | `1.00` (2/2) | `+0.50` |

### Análise da Evolução
- **Integridade da avaliação:** O hash `split_sha256` permaneceu idêntico entre `r1` e `r2`, assegurando que ambos os modelos foram avaliados exatamente sobre os mesmos 4 exemplos de teste.
- **Efeito da correção de rótulo:** Em `chamados-v1`, o chamado `006` (`bloqueio de acesso`) estava rotulado incorretamente como `comum`, gerando empate na contagem do token `bloqueio` no treino e fazendo `r1` classificar o chamado de teste `010` (`bloqueio de conta`) como `comum`. Após a correção em `chamados-v2`, o token `bloqueio` passou a associar-se exclusivamente a `urgente`, elevando o *recall* dessa classe de `0.50` para `1.00` e a acurácia global de `0.75` para `1.00`.

## 3. Evidência de Restauração em Clone Limpo (`modelo-r1`)

O repositório foi clonado em um diretório limpo (`aula-ml-restauracao`), posicionado na tag `modelo-r1` (commit `7526d288c8ba38057a8ce2457706c0a83d3cc89d`) e restaurado via `dvc pull`. A verificação criptográfica confirmou correspondência integral com o manifesto original de `r1`:

    OK data/chamados.csv bc3fd86a3a5f598841a58839a9726d901fa7e8361ffe79c5b43873ad8a1328ac
    OK data/split.csv 2ef820171c1a4625fe216e6da67c7c8dcf9d97a08288c0239cd23f1a6961d981
    OK models/model.json 72a3a67936139a3473838a6dcfa7aae1acb4c23467be1ef1056d51d485b94840
