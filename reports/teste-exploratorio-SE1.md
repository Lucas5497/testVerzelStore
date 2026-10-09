# Teste Exploratório

**ID da sessão:** SE-01  
**Título:** Exploração integrada de cupons e recálculo do carrinho  
**Tempo planejado:** 30 minutos  
| **Ambiente** https://verzel-store.qa-test-verzel-store.workers.dev/ · Chrome _139.0.7258.128_ · Windows · 07/10/2026   
**Abordagem:** Testes exploratórios de UI, com consulta à API quando necessário.  
**Objetivo:** Investigar comportamentos complementares aos 38 testes estruturados, priorizando entradas atípicas, transições de estado e recálculo dos valores do carrinho.

## Escopo da exploração

- Tratamento de entradas atípicas de cupom.
- Aplicação, remoção, reaplicação e substituição de cupons.
- Recálculo do carrinho após alterações nos produtos e nas quantidades.
- Consistência de subtotal, desconto, frete e total.
- Mensagens de validação e erros inesperados na interface.

## Checklist de execução

- [x] **EXP-01 — Cupom vazio:** tentar aplicar o cupom sem preencher o campo.
- [x] **EXP-02 — Cupom com espaços:** testar uma entrada contendo somente espaços.
- [x] **EXP-03 — Caracteres especiais:** informar caracteres como `@@@###` e observar o tratamento da entrada.
- [x] **EXP-04 — Variação de caixa:** testar um cupom válido com letras minúsculas ou combinação de maiúsculas e minúsculas.
- [x] **EXP-05 — Remoção e reaplicação:** aplicar um cupom válido, removê-lo e aplicá-lo novamente.
- [x] **EXP-06 — Substituição de cupom:** tentar aplicar outro cupom com o primeiro ativo e, em seguida, repetir a operação após removê-lo.
- [x] **EXP-07 — Alteração de quantidade:** modificar a quantidade de um produto com um cupom ativo e verificar o recálculo dos valores.
- [x] **EXP-08 — Inclusão e remoção de produtos:** adicionar ou remover produtos com o cupom ativo e conferir os valores atualizados.
- [x] **EXP-09 — Cruzamento do limite de frete:** quando possível, alterar o carrinho de um subtotal acima de R$ 200,00 para abaixo desse limite e vice-versa.
- [x] **EXP-10 — Consistência e mensagens:** conferir subtotal, desconto, frete, total, mensagens exibidas e possíveis erros no Console do navegador.

### Exploração complementar — Finalização da compra (Checkout)

- [x] **EXP-11 — Envio com campos vazios:** clicar em `Confirmar pedido` sem preencher nenhum campo e observar as validações apresentadas.
- [x] **EXP-12 — Nome completo:** testar campo vazio, somente espaços e nome com caracteres válidos. Verificar se entradas inadequadas são rejeitadas.
- [x] **EXP-13 — E-mail inválido:** testar formatos como `lucas`, `lucas@` e `lucas@exemplo`. Registrar como a aplicação valida o endereço.
- [x] **EXP-14 — CEP inválido:** testar campo vazio, letras, quantidade insuficiente de números e formato incorreto.

Aberto o bug para a EXP-14 do teste exploratório SE-001 BUG-003 | [`bugs/BUG-003/`](../bugs/BUG-003.md)
