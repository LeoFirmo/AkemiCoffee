---
name: gerador-recursos-fundo-de-funil
description: gera, em uma única resposta, o pacote completo de recursos de uma campanha de rede de pesquisa do google ads (títulos, descrições, sitelinks e frases em destaque) para anúncios de fundo de funil. extrai dados automaticamente da página do produto (domínio ou html) antes de perguntar qualquer coisa manualmente, e adapta idioma, país e moeda para múltiplos mercados na mesma execução.
---

# gerador de recursos - fundo de funil (google ads)

> esta skill foi apresentada ao vivo no enid. este documento existe pra você não só usar,
> mas entender exatamente o que ela faz em cada etapa - pra poder ajustar pro seu próprio
> produto, nicho ou fluxo sem precisar reescrever do zero.

## o que essa skill faz, em 3 blocos

toda skill boa segue a mesma lógica: **entrada → análise → saída**. aqui está o mapa completo
antes de entrar em detalhe de cada parte:

| bloco | o que significa aqui |
|---|---|
| **entrada** | o domínio/html da página do produto (extração automática) + as poucas informações que a página não revela sozinha |
| **análise** | o critério que separa uma copy de fundo de funil de uma copy de topo, os limites de caractere de cada formato, e a regra de moeda/idioma do país escolhido |
| **saída** | 4 blocos de títulos, 6 descrições, 10 grupos de sitelinks e 15 frases em destaque - todos já validados por contagem de caractere e prontos pra colar no google ads |

---

## entrada - o que a skill pede antes de gerar

### passo 0 - extração automática a partir da página (faça isso primeiro, sempre)

antes de perguntar qualquer dado manualmente, a skill pede o **domínio ou o link da página do produto** - ou, se a página não estiver publicada ainda, o **código html** colado diretamente.

> "me envie o domínio/link da página do produto, ou cole o código html dela. se você não tiver
> nenhum dos dois, digite **não tenho** e eu sigo com perguntas manuais."

com isso em mãos, a skill lê a página (via leitura de url ou do html colado) e tenta extrair sozinha:

- nome do produto
- preço atual exibido
- preço original/de-por, se houver (para calcular ou confirmar o desconto - **nunca inventar** se não estiver explícito)
- sinal de frete grátis (texto tipo "free shipping", "frete grátis", ícone de caminhão)
- sinal de garantia e prazo, se mencionado (ex: "30-day money back guarantee")
- sinais de oficialidade (marca registrada, "official site", selo de fabricante)
- idioma em que a página está escrita (pode ser diferente do idioma do anúncio - ver passo 2)

depois da extração, a skill **mostra uma tabela de confirmação** com tudo que encontrou e pede só para confirmar ou corrigir o que estiver errado ou faltando - em vez de perguntar cada campo do zero. isso existe porque dado extraído direto da página real é mais confiável do que dado digitado de memória, e porque poupa tempo de quem está usando a skill toda semana.

campos que a página normalmente **não** revela e que ainda precisam ser perguntados:
- valor exato do desconto, se não estiver escrito literalmente na página
- dias de garantia, se não estiver escrito literalmente
- país de veiculação do anúncio (a página pode não deixar isso claro)

### passo 1 - perguntas manuais (uma de cada vez, só o que faltou)

1. nome do produto *(pula se já extraído)*
2. idioma do anúncio - **não presuma que é o mesmo idioma da página**; uma página em inglês pode virar anúncio em francês para o mercado francês
3. país de veiculação
4. moeda - a skill sugere automaticamente a moeda padrão do país informado (ver tabela na seção de análise), mas sempre pergunta para confirmar, porque um mesmo país pode ter contexto de preço em outra moeda (ex: campanha em usd rodando para o brasil)
5. preço (como deve aparecer, já no formato da moeda/local - ver regras de formatação)
6. valor do desconto *(pula se extraído com segurança da página)*
7. percentual de desconto *(pula se extraído com segurança da página)*
8. frete grátis? (sim/não - se não: envio rápido/imediato/expresso)
9. tem garantia? (sim/não - se sim: quantos dias?)
10. variações - mesmo menu de sempre, responda só os números:

| nº | variação | tom |
|---|---|---|
| 0 | temas padrão | preço, valor do desconto, % de desconto, frete, garantia (se houver), site oficial, loja oficial, direto do fabricante |
| 1 | escassez | estoque limitado + prazo curtíssimo ("últimas unidades hoje") |
| 2 | urgência | ação imediata sem citar estoque ("corra", "aproveite agora") |
| 3 | exclusividade | "somente aqui", "exclusivo", "apenas no site oficial" |
| 4 | preço/desconto | valores exatos informados; nunca inventar |
| 5 | garantia | reforça o prazo informado ("garantia de x dias") |
| 6 | frete grátis | entrega sem custo/rápida |
| 7 | oficialidade | autenticidade e procedência ("produto original", "site oficial") |
| 8 | educativo | convite para conhecer ("descubra como...", "veja como funciona") |
| 9 | aleatório | a skill escolhe 3 variações distintas entre 1-8 sozinha |

digite **0** para temas padrão, **9** para aleatório, ou quantos números quiser - sem limite - separados por espaço/vírgula.

---

## análise - a lógica que a skill aplica

### o que faz uma copy ser "de fundo de funil"

essa skill foi desenhada especificamente para quem **já conhece a oferta** - não é para gerar curiosidade, é para converter quem já está perto da decisão. por isso, toda variação de tom gira em torno de três elementos: **prova concreta** (preço, garantia, frete), **remoção de objeção** (oficialidade, garantia) e **urgência real** (escassez, urgência) - nunca em torno de descoberta ou educação genérica sobre o problema.

### regras de valores (inegociável)

- usar exatamente os valores informados ou extraídos da página. **proibido calcular, converter, arredondar ou alterar.**
- "economize/poupe {valor}" só aparece se esse valor foi informado ou extraído.
- "-{%}" só aparece se o percentual foi informado ou extraído.
- "de x por y" só aparece se os dois preços foram informados ou extraídos explicitamente.

### idioma, país e moeda - como os três se combinam

idioma, país e moeda **não andam automaticamente juntos** - por isso são três perguntas separadas, não uma só. exemplos reais desse descolamento: uma campanha pode rodar em francês para a bélgica com preço em euro; uma campanha em inglês pode rodar para o canadá com preço em dólar canadense; uma campanha em espanhol pode rodar para os eua com preço em dólar americano.

**tabela de formatação de moeda** (aplicar sempre no texto gerado, nunca misturar padrões):

| país/mercado | moeda | exemplo de formatação |
|---|---|---|
| brasil | brl | r$ 39,98 |
| estados unidos | usd | $39.98 |
| reino unido | gbp | £39.98 |
| zona do euro (fr/de/es/it/pt) | eur | 39,98 € |
| canadá (inglês) | cad | ca$39.98 |
| canadá (francês) | cad | 39,98 $ |
| méxico | mxn | $39.98 mxn |
| austrália | aud | a$39.98 |

regra geral: eur e brl usam vírgula decimal e símbolo pode vir antes (brl) ou depois (eur, com espaço). usd, gbp, aud, cad (inglês) usam ponto decimal e símbolo antes, sem espaço. se o país pedido não estiver na tabela, pergunte ao usuário o formato correto antes de gerar - não adivinhe.

### tradução de referência

- texto do anúncio sempre no **idioma do anúncio** (o que vai ao ar).
- ao lado, sempre uma coluna de **tradução em português do brasil** - essa coluna é fixa independente do país/idioma da campanha, porque é a língua de quem está revisando o pacote.
- nunca misturar idiomas dentro do texto do anúncio. nunca traduzir o nome do produto.

---

## saída - o formato exato que a skill entrega

### títulos (23-30 caracteres) - 4 blocos

1. produto + [benefício] (colchetes) - 10 títulos
2. produto em posição aleatória - 10 títulos
3. produto + benefício (sem colchetes) - 10 títulos
4. sem nome do produto - 25 títulos

cada bloco em tabela: **título | tradução pt-br | caracteres**

### descrições (80-90 caracteres)

6 descrições, todas começando pelo nome do produto.
tabela: **descrição | tradução pt-br | caracteres**

### sitelinks

10 grupos numerados, cada grupo cobrindo 1 tema (dos temas escolhidos na variação).
3 frases curtas no idioma do anúncio, separadas por `|` na mesma célula.
coluna ao lado: traduções separadas por `|` na mesma ordem.

### frases em destaque / callouts (≤25 caracteres)

15 frases curtas.
tabela: **frase | tradução pt-br | caracteres**

### contagem e validação (aplicar em todos os blocos, sempre)

- contar no idioma original, incluindo espaços e símbolos - nunca estimar visualmente.
- títulos fora de 23-30 caracteres → reescrever até caber.
- descrições fora de 80-90 caracteres → reescrever até caber.
- frases em destaque acima de 25 caracteres → reescrever.
- frases de sitelink fora de 15-35 caracteres → ajustar.
- recalcular tudo até 100% dentro do limite antes de entregar - nunca entregar com contagem pendente.

---

## política de execução (zero interrupção)

- proibido perguntar ou confirmar antes de gerar, **depois** que a fase de perguntas (passo 0 + passo 1) terminar.
- terminada a última pergunta, gerar o pacote inteiro na mesma resposta.
- frases proibidas: "quer que eu finalize?", "posso continuar?".
- gere direto.

## encerramento e reutilização

depois de entregar todos os blocos, pergunte **apenas**:
"deseja anunciar o mesmo produto para outro país?"

- se **não** → encerrar, sem sugestões novas.
- se **sim** → pergunte: "os dados vão mudar, ou você quer trocar só idioma, país e moeda?"
  - **só trocar idioma/país/moeda**: peça apenas os três novos valores, reaproveite todos os demais dados já extraídos/coletados (preço, desconto, frete, garantia, variações), e gere o pacote completo de novo já com a formatação de moeda do novo mercado.
  - **também vai mudar dado do produto**: reinicie do passo 0 (nova página/domínio ou novos dados manuais).

---

## erros a evitar

- não pular o passo 0. mesmo que o usuário já tenha os dados na cabeça, a extração da página
  pega detalhes que a memória perde - frete grátis condicional, garantia com letra miúda,
  variação de preço por sku.
- não presumir moeda a partir do idioma, nem idioma a partir do país. são três perguntas
  independentes.
- não misturar formato de moeda dentro do mesmo pacote - todo o pacote usa o mesmo padrão do
  mercado selecionado.
- não inventar desconto ou preço "de-por" quando a página só mostra um preço.
- não pedir confirmação no meio da geração - se a fase de perguntas acabou, o pacote sai inteiro
  na mesma resposta.

---

## como estender essa skill pro seu negócio

essa estrutura não é exclusiva de suplemento nem de fundo de funil. pra adaptar pro seu produto:

1. **troque o passo 0** pela fonte de dado que existe no seu negócio - pode ser uma página, uma planilha de estoque, um feed de produto, um crm.
2. **redefina o critério de "análise"** - o que separa uma copy boa de uma ruim no seu funil?
   aqui foi prova + remoção de objeção + urgência porque é fundo de funil; no seu caso pode ser outra coisa.
3. **mantenha a saída no formato exato da plataforma final** - títulos, limites de 29 caracteres