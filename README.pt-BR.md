# Header Change Mask Decoder

Este projeto explica, passo a passo, como interpretar o campo `header__change_mask`
produzido pelos produtos de integracao de dados da Qlik. O objetivo e mostrar
quais colunas da change table (tabelas `__ct`) foram atualizadas em relacao a
tabela de origem.

## O que o programa faz

1. Le a entrada hexadecimal (`header__change_mask`).
2. Normaliza a entrada (remove `0x` e completa com zero a esquerda para ficar
   com numero par de caracteres).
3. Converte cada byte para binario (8 bits por byte).
4. Aplica a regra do destino Event Hubs:
   - `sim`: usa todos os bits (descarte 0).
   - `nao`: descarta os ultimos N bits (padrao 7).
5. Remove zeros a esquerda (null-trim).
6. Mapeia os bits da direita para a esquerda para as colunas de dados:
   - bit 1 = coluna atualizada
   - bit 0 = coluna nao modificada

## Regras importantes

- A posicao do bit segue o ordinal da coluna na change table.
- Os bits mapeiam apenas colunas de dados; campos `header__...` nao entram na mascara.
- Os bytes sao little-endian e podem ter zeros a esquerda removidos.
- A quantidade de bits descartados equivale ao numero de campos `header__`
  armazenados na tabela `__ct`. O padrao e 7, mas pode mudar por configuracao.
  Esse valor vira 0 quando o destino e Event Hubs.

## Como usar

1. Abra `index.html` ou https://ivanyort.github.io/hcmd/ no navegador.
2. Informe o valor hexadecimal de `header__change_mask`.
3. Escolha se o destino e Event Hubs (`n` ou `y`).
4. Informe quantos bits finais devem ser descartados (padrao 7).
5. Clique em "Decodificar" para ver o resultado e o passo a passo.

## Autor

Ivan Yort <ivan.yort@qlik.com>

## Repositorio

https://github.com/ivanyort/hcmd

