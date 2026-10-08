# Ambiguidades, interpretações e limitações do ambiente

## 1. Limitações do ambiente (não reportadas como bug)

| # | Simplificação documentada | Efeito nos testes |
|---|---|---|
| L1 | O carrinho fica guardado só na aba do navegador | Outra aba, outro navegador ou janela anônima começam vazios; cada teste automatizado usa contexto novo |
| L2 | Pedidos não são armazenados; o número é fictício | Valido o formato `VZ-000000`, não existe consulta de pedido |
| L3 | Nenhum e-mail é enviado e nenhuma cobrança é feita | Nada a verificar |
| L4 | Produtos, preços e cupons são fixos; não há controle de estoque | Catálogo e cupons usados como dados fixos; estoque não é testado |
| L5 | A API não guarda estado entre chamadas | Não existe "adicionar ao carrinho" na API: o cálculo recebe o carrinho inteiro a cada chamada |
| L6 | Fora de escopo: login, cadastro, pagamento online, consulta de pedidos (e carga, estresse e segurança, pelas regras do teste) | Não testados |

Comportamentos **documentados** que também não são bugs: `/api/carrinho/calcular` responde 200 para cupom inválido/expirado (o motivo vem em `cupom.mensagem`), enquanto `/api/pedidos` responde 422.

## 2. Ambiguidades e interpretações

| ID | O que a documentação não define | Interpretação adotada | Observado na execução |
|---|---|---|---|
| <a id="a1"></a>A1 | O que acontece ao tentar aplicar um segundo cupom **sem** remover o primeiro (CA05 só diz que, para trocar, é preciso remover) | Invariante: nunca dois cupons ativos e descontos nunca somados | <!-- PREENCHER --> |
| <a id="a2"></a>A2 | Mensagem para cupom vazio ou só com espaços | Resposta controlada, sem erro técnico exposto e sem desconto; a mensagem exata não é exigida | <!-- PREENCHER --> |
| <a id="a3"></a>A3 | Método de arredondamento (CA11 diz apenas "2 casas decimais") | Com este catálogo e 10%, o desconto é sempre exato (preços múltiplos de R$ 0,10), então não existe caso de meio-centavo; o único cupom que poderia gerá-lo (VERAO2026, 15%) está expirado. CA11 é verificado como **valores exatos, sem ruído de ponto flutuante e exibidos com 2 casas** | — |
| <a id="a4"></a>A4 | Quais subtotais permitem testar a fronteira do frete | Todos os subtotais são múltiplos de R$ 0,10, então R$ 199,99 e R$ 200,01 **não são alcançáveis**. Uso R$ 199,90 (logo abaixo), R$ 200,00 (limite) e R$ 209,40 (menor valor acima, com até 5 unidades por produto) | — |
| <a id="a5"></a>A5 | Detalhes das regras já existentes do cliente | Nome válido = pelo menos duas palavras. E-mail: testo apenas formatos claramente inválidos (sem `@`, sem domínio). CEP: 8 dígitos, com ou sem hífen em `00000-000`; outras posições de hífen não são testadas | — |

## 3. Como decido entre bug e comportamento esperado

1. Se está em "Sobre este ambiente", é limitação do ambiente, não bug.
2. Se contradiz um critério de aceite, uma regra de cálculo ou um código de erro da documentação, é bug e cito a regra violada.
3. Se a documentação é silenciosa, registro como ambiguidade (tabela acima), a menos que o comportamento seja claramente incorreto (ex.: valor monetário com mais de 2 casas decimais ou erro técnico exposto ao cliente).
