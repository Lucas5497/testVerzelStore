# Estratégia de testes — VZS-142 (cupom de desconto e frete grátis)

## 1. Escopo

Validar a entrega **VZS-142, versão 2.3.0** da Verzel Store: os critérios de aceite CA01–CA11, a API (`/produtos`, `/carrinho/calcular`, `/pedidos`), a exibição no carrinho e as regras que já existiam para o pedido (nome com sobrenome, e-mail e CEP).

**Fora do escopo:** testes de carga, estresse e segurança (ambiente compartilhado, regra do teste) e login, cadastro, pagamento online e consulta de pedidos (documentação).

## 2. Abordagem

- **A API é a fonte da verdade.** A documentação diz que o cálculo é feito pela API e a tela só exibe. Por isso as regras de negócio são verificadas direto na API (rápido e determinístico) e a interface é verificada na exibição e no fluxo, comparando o que aparece na tela com a resposta da API.
- **Oráculo independente.** Os valores esperados são calculados à parte, em centavos inteiros ([`oraculo.ts`](../playwright/utils/oraculo.ts)), sem reaproveitar lógica da aplicação.
- **Dados fixos e nomeados.** Carrinhos-alvo para cada fronteira em [`03-dados-de-teste.md`](03-dados-de-teste.md).
- **Ambiente compartilhado.** Poucos workers e só Chromium; cada teste usa um contexto novo (o carrinho vive na aba), então não há interferência entre testes nem entre candidatos.

## 3. Técnicas

| Técnica | Onde foi aplicada |
|---|---|
| Valor-limite | Frete: R$ 199,90 / 200,00 / 209,40 · Quantidade: 4 / 5 / 6 unidades |
| Partição de equivalência | Cupom: válido, inexistente, expirado · CEP: formatos aceitos × rejeitados |
| Tabela de decisão | Subtotal × cupom → desconto, frete e total (CT-FRT-006, CT-FRT-007) |
| Transição de estados | Aplicar, trocar e remover cupom (CA05) |
| Adivinhação de erros | Caixa e espaços no cupom, ruído de ponto flutuante, contorno do limite pela API |
| Testes exploratórios | 4 sessões com *charter* ([`reports/execucao-dos-testes.md`](../reports/execucao-dos-testes.md)) |

## 4. Riscos e prioridade

Onde um defeito mais custa e onde as regras mais interagem:

1. **Frete × desconto (CA08/CA09)** — o frete grátis deve usar o subtotal *antes* do desconto: CT-FRT-06 e CT-FRT-007.
2. **Limite de 5 unidades na interface *e* na API (CA10)** — uma camada pode ser contornada pela outra: CT-QTD-03 a 05.
3. **Valores monetários (CA11)** — somas e percentuais em ponto flutuante podem vazar casas decimais: CT-VAL-001.
4. **Normalização do cupom (CA02)** — caixa e espaços: CT-CUP-03.
5. **Diferença entre `/calcular` e `/pedidos`** — o mesmo cupom ruim é 200 num e 422 no outro: CT-CUP-04, 05 e 08.

Os casos **P1** (14) rodam primeiro e são os primeiros candidatos à automação.

## 5. Rastreabilidade: critério de aceite × casos

| CA | Regra | Casos |
|---|---|---|
| CA01 | BEMVINDO10 aplica 10% sobre o subtotal | CT-CUP-001, CT-CUP-002 |
| CA02 | Código sem distinção de caixa; espaços nas pontas ignorados | CT-CUP-003 |
| CA03 | Cupom inexistente: "Cupom inválido." | CT-CUP-004, CT-CUP-008 |
| CA04 | Cupom expirado: "Cupom expirado." | CT-CUP-005, CT-CUP-008 |
| CA05 | Um cupom por vez; para trocar, remover e aplicar outro | CT-CUP-006, CT-CUP-007 |
| CA06 | Frete grátis a partir de R$ 200,00, inclusive | CT-FRT-004, CT-FRT-005 |
| CA07 | Abaixo de R$ 200,00: frete R$ 19,90 e informa quanto falta | CT-FRT-001, CT-FRT-002, CT-FRT-003 |
| CA08 | Frete grátis considera o subtotal antes do desconto | CT-FRT-006, CT-FRT-007 |
| CA09 | Desconto não incide sobre o frete | CT-CUP-001, CT-FRT-006, CT-FRT-007 |
| CA10 | Máx. 5 unidades por produto (UI e API) | CT-QTD-001, CT-QTD-002, CT-QTD-003, CT-QTD-004, CT-QTD-005, CT-QTD-006 |
| CA11 | Valores com 2 casas decimais | CT-VAL-001, CT-VAL-002 |
| — | Contrato da API e regras do pedido (sem CA numerado) | CT-API-001, CT-API-002, CT-API-003, CT-API-004, CT-API-005, CT-PED-01, CT-PED-02, CT-PED-03, CT-PED-04 |

## 6. Execução e relato

- **UI:** execução manual com captura de tela por passo relevante (`evidence/<ID>/`).
- **API:** suíte Playwright (`npm run test:api`); evidência = relatório HTML, execução no CI e, quando útil, requisição/resposta no documento de evidências.
- **Bugs:** um arquivo por bug em [`bugs/`](../bugs), com a regra violada citada. Antes de reportar, confiro se o comportamento é uma limitação documentada do ambiente ([`02-ambiguidades-e-limitacoes.md`](02-ambiguidades-e-limitacoes.md)).
- **Bug confirmado na automação:** o teste correspondente é marcado como falha esperada (`bugConhecido`), mantendo o pipeline verde e o bug rastreável.
- **Regressão final:** `npm test` completo antes da entrega.
