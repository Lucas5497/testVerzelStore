# Dados de teste

Fonte: documentação da entrega VZS-142 (seção "Dados para teste"). O mesmo catálogo está em [`playwright/fixtures/catalogo.ts`](../playwright/fixtures/catalogo.ts).

## Catálogo

| Id | Produto | Preço |
|---|---|---:|
| P001 | Camiseta Essencial | R$ 59,90 |
| P002 | Calça Jeans Slim | R$ 139,90 |
| P003 | Tênis Casual Urbano | R$ 189,90 |
| P004 | Boné Aba Curva | R$ 49,90 |
| P005 | Mochila Urbana 20L | R$ 100,00 |
| P006 | Kit 3 Pares de Meias | R$ 29,90 |
| P007 | Jaqueta Corta-Vento | R$ 229,90 |
| P008 | Garrafa Térmica 750ml | R$ 50,00 |

## Cupons

| Código | Desconto | Situação | Usado em |
|---|---:|---|---|
| `BEMVINDO10` | 10% | válido | CT-CUP-001 a 003, 006, 007, FRT-006, FRT-007, VAL-001 |
| `VERAO2026` | 15% | expirado em 31/03/2026 | CT-CUP-005, 006, 007, 008 |
| `CUPOMINEXISTENTE` | — | não existe (código inventado) | CT-CUP-004, 008 |

## Carrinhos de teste e valores esperados

Valores calculados à parte, em centavos (`playwright/utils/oraculo.ts`). Frete: R$ 19,90 abaixo de R$ 200,00; grátis a partir de R$ 200,00 (sobre o subtotal **antes** do desconto).

| Carrinho | Itens | Cupom | Subtotal | Desconto | Frete | Faltam p/ grátis | Total |
|---|---|---|---:|---:|---:|---:|---:|
| `r100` | P005×1 | — | R$ 100,00 | R$ 0,00 | R$ 19,90 | R$ 100,00 | R$ 119,90 |
| `r100` | P005×1 | BEMVINDO10 | R$ 100,00 | R$ 10,00 | R$ 19,90 | R$ 100,00 | R$ 109,90 |
| `r139_90` | P002×1 | — | R$ 139,90 | R$ 0,00 | R$ 19,90 | R$ 60,10 | R$ 159,80 |
| `r139_90` | P002×1 | BEMVINDO10 | R$ 139,90 | R$ 13,99 | R$ 19,90 | R$ 60,10 | R$ 145,81 |
| `r199_90` | P004×1 + P008×3 | — | R$ 199,90 | R$ 0,00 | R$ 19,90 | R$ 0,10 | R$ 219,80 |
| `r199_90` | P004×1 + P008×3 | BEMVINDO10 | R$ 199,90 | R$ 19,99 | R$ 19,90 | R$ 0,10 | R$ 199,81 |
| `r200` | P005×2 | — | R$ 200,00 | R$ 0,00 | R$ 0,00 | R$ 0,00 | R$ 200,00 |
| `r200` | P005×2 | BEMVINDO10 | R$ 200,00 | R$ 20,00 | R$ 0,00 | R$ 0,00 | R$ 180,00 |
| `r209_40` | P001×1 + P006×5 | — | R$ 209,40 | R$ 0,00 | R$ 0,00 | R$ 0,00 | R$ 209,40 |
| `r209_40` | P001×1 + P006×5 | BEMVINDO10 | R$ 209,40 | R$ 20,94 | R$ 0,00 | R$ 0,00 | R$ 188,46 |
| `exemploDoc` | P002×1 + P004×2 | — | R$ 239,70 | R$ 0,00 | R$ 0,00 | R$ 0,00 | R$ 239,70 |
| `exemploDoc` | P002×1 + P004×2 | BEMVINDO10 | R$ 239,70 | R$ 23,97 | R$ 0,00 | R$ 0,00 | R$ 215,73 |

### Por que não existem R$ 199,99 nem R$ 200,01

Todos os preços do catálogo são múltiplos de R$ 0,10; logo **todo subtotal também é**. As fronteiras alcançáveis do frete grátis são **R$ 199,90** (logo abaixo), **R$ 200,00** (limite) e **R$ 209,40** (menor valor acima do limite, com até 5 unidades por produto). Ver [`02-ambiguidades-e-limitacoes.md`](02-ambiguidades-e-limitacoes.md#a4).

### Carrinhos de risco de ponto flutuante (CA11)

Em aritmética de ponto flutuante "ingênua", estas contas geram ruído (ex.: `3 × 139,90 = 419.70000000000005`). Se a API não arredondar, o valor vaza na resposta.

| Carrinho | Subtotal exato | Total exato com BEMVINDO10 |
|---|---:|---:|
| P006×1 | R$ 29,90 | R$ 46,81 |
| P002×2 | R$ 279,80 | R$ 251,82 |
| P002×3 | R$ 419,70 | R$ 377,73 |
| P006×3 | R$ 89,70 | R$ 100,63 |

## API

| Método e rota | Sucesso | Observação |
|---|---|---|
| `GET /api/produtos` | 200 | lista o catálogo |
| `GET /api/produtos/{id}` | 200 | 404 `PRODUTO_NAO_ENCONTRADO` se não existir |
| `POST /api/carrinho/calcular` | 200 | cupom inválido/expirado **não** gera erro: 200 sem desconto e motivo em `cupom.mensagem` |
| `POST /api/pedidos` | 201 | cupom inválido/expirado gera **422** (`CUPOM_INVALIDO` / `CUPOM_EXPIRADO`) |

Erros seguem `{ "erro": { "codigo", "mensagem", "campo" } }` com os códigos 400, 404, 405 e 422 descritos na documentação.

Exemplo (equivale a CT-CUP-01):

```bash
curl -s -X POST https://verzel-store.qa-test-verzel-store.workers.dev/api/carrinho/calcular \
  -H "Content-Type: application/json" \
  -d '{"itens":[{"produtoId":"P005","quantidade":1}],"cupom":"BEMVINDO10"}'
```

Cliente válido usado nos pedidos: `{ "nome": "Maria Silva", "email": "maria@exemplo.com", "cep": "01310-100" }`.
