# ⚙️ PROMPT DE RETOMADA (cole no início de toda conversa nova)

> Você é um desenvolvedor sênior de jogos 2D usando MonoGame em C#. Leia o documento "specs.md" abaixo como a fonte completa do projeto. Continue o trabalho de onde parou, respeitando TODAS as convenções, a arquitetura e as decisões registradas. Não reescreva sistemas que já estão marcados como concluídos. Ao final de qualquer mudança, sugira a atualização da seção "5. Estado Atual" com um resumo simples e objetivo (o que mudou / o que falta).

---

# PROJETO: [NOME DO JOGO]

## 1. Visão Geral

### 1.1 Pitch (1 parágrafo)
[Descreva o jogo em poucas frases. Ex.: um action survive de arena em 2D top-down que mistura hordas de inimigos e escolha de poderes com combate de ação direta estilo musou — o jogador avança por zonas de batalha e precisa conquistar e defender territórios.]

### 1.2 Gênero e Referências
- **Gênero principal:** Action / Arena Survivor / Musou 2D top-down
- **Referências de mecânicas:**
  - *Vampire Survivors* (hordas, poderes por nível, progressão)
  - *Hyrule Warriors* (áreas de batalha, zonas, combate com golpes)
  - [adicione outras referências]

### 1.3 Experiência alvo (o que o jogador deve sentir)
- [Ex.: combate frenético mas com controle direto dos golpes; estratégia de defesa de território; sensação de poder crescente ao subir de nível.]

### 1.4 Plataformas alvo
| Plataforma | Prioridade | Observações |
|---|---|---|
| Windows Desktop | Alta | |
| Linux | Média | |
| macOS | Baixa | |
| Web (via Wasm/outro) | [decidir] | |
| Mobile (Android/iOS) | Futuro | depende de controles touch |
| Consoles | Futuro | MonoGame suporta, mas exige licenciamento |

---

## 2. Stack e Setup

### 2.1 Ferramentas
- **Engine/Framework:** MonoGame
- **Linguagem:** C#
- **Versão .NET:** [ex.: .NET 8]
- **IDE:** [ex.: Visual Studio / Rider / VS Code]
- **Controle de versão:** Git (repositório: [link])

### 2.2 Estrutura do repositório
```
[NomeDoProjeto]/
├── src/
│   ├── [NomeDoJogo].Core/        # lógica do jogo (não depende de MonoGame)
│   ├── [NomeDoJogo].MonoGame/    # projeto MonoGame (entry point)
│   └── [NomeDoJogo].Content/     # assets (texturas, fontes, sons)
├── docs/
│   └── specs.md                  # ESTE documento
├── README.md
└── [outros]
```

> **Regra:** lógica de jogo (regras, estado, matemática) fica no `.Core`. Tudo que toca MonoGame (input, render, som) fica no projeto MonoGame.

---

## 3. Arquitetura e Organização do Código

### 3.1 Padrões adotados
- **Atualização de estado:** [ex.: GameStateManager + enum GameState]
- **Organização de entidades:** [ex.: classes simples + herança de `Entity`, ou ECS, ou componentes]
- **Game Loop:** `Update()` (lógica) separado de `Draw()` (render); timestep fixo para física/colisão.

### 3.2 Principais namespaces/pastas
- `Entities/` — jogador, inimigos, projéteis
- `Combat/` — golpes, magias, dano, hitboxes
- `Enemies/` — tipos de inimigos, spawners de hordas
- `Progression/` — level up, escolha de poderes
- `Zones/` — seções do mapa, captura e defesa
- `UI/` — HUD, menus, tela de escolha de poder
- `Content/` — carregamento de assets

### 3.3 Regra de tamanho de arquivo
- Máximo ~300 linhas por arquivo.
- Um sistema/funcionalidade = um arquivo com nome claro.

---

## 4. Convenções de Código

### 4.1 Nomenclatura
- Classes e métodos: `PascalCase`
- Variáveis locais: `camelCase`
- Campos privados: `_camelCase`
- Constantes: `PascalCase`

### 4.2 Organização
- Um `class` por arquivo (salvo tipos internos muito específicos).
- Comentários `///` em toda classe pública e em métodos não óbvios.
- Não usar números mágicos: extrair para constantes ou campos nomeados.

### 4.3 Assets
- Texturas na pasta `Content/` correspondente.
- Nomes de assets em `snake_case` ou `PascalCase` (defina uma única regra).
- [defina dimensão/estilo: pixel art? resolução base?]

---

## 5. Mecânicas Principais

### 5.1 Personagem / Movimento
- [ ] Movimento em 8 direções (top-down)
- [ ] Colisão com paredes/cenário
- [ ] Atributos base: vida, velocidade, dano, defesa
- Detalhes: [descreva controles, física, valores base]

### 5.2 Combate ativo (golpes por botão)
- [ ] Golpes acionados por botões (não apenas automáticos)
- [ ] Combos ou golpes independentes?
- [ ] Hitboxes melee (área de dano na frente do personagem)
- [ ] Tempo de recarga/cooldown de cada golpe
- Detalhes: [descreva botões, tipos de golpe, como funciona o dano]

### 5.3 Magias
- [ ] Sistema de magias com custo/recurso (mana? cooldowns?)
- [ ] Tipos de magia: [área, projétil, buff, etc.]
- [ ] Como o jogador desbloqueia magias
- Detalhes: [descreva]

### 5.4 Armas / Ataques automáticos (inspiração arena survivor — se mantido)
- [ ] Armas que disparam automaticamente ao longo do tempo
- [ ] Armas como power-ups passivos que somam ao combate ativo?
- Detalhes: [decida se o jogo terá apenas combate ativo ou os dois — ativo + automático]

### 5.5 Inimigos e Hordas
- [ ] Spawn contínuo de hordas
- [ ] Tipos de inimigos: [básico, rápido, tanque, ranged, elite, chefe]
- [ ] Dificuldade crescente (escala com tempo ou zonas)
- Detalhes: [descreva cada tipo e seu comportamento]

### 5.6 Progressão e Poderes (escolha por nível)
- [ ] Ganho de XP ao derrotar inimigos
- [ ] Ao subir de nível, tela para escolher 1 de N poderes (roguelite)
- [ ] Lista de poderes disponíveis: [ex.: +dano, +velocidade, nova arma, magia nova, cura]
- [ ] Upgrades permanentes entre partidas? (meta-progressão)
- Detalhes: [descreva]

### 5.7 Mapa e Zonas de Batalha
- [ ] Mapa estruturado em várias seções/zonas (não infinito)
- [ ] Cada zona é uma arena distinta dentro do mapa
- [ ] Zonas podem ficar "abertas": inimigos retornam e tentam reconquistar
- [ ] Objetivo de cada fase: [dominar todas as zonas? sobreviver? derrotar chefe?]
- Detalhes: [descreva o fluxo de um mapa, tamanhos, transições]

### 5.8 Dominação / Defesa de Zonas
- [ ] Mecânica de captura (ex.: permanecer na zona, derrotar líder, segurar ponto)
- [ ] Inimigos podem retomar zonas já conquistadas
- [ ] Bônus ou penalidade por zones controladas
- Detalhes: [descreva lógica de controle]

---

## 6. Game Loop e Fluxo de Partida

1. [Seleção de personagem/poder inicial?]
2. Jogador entra no mapa com N zonas
3. Conquista zonas lutando contra hordas
4. Sobe de nível e escolhe poderes
5. Inimigos tentam reconquistar zonas → jogador defende
6. Objetivo da fase cumprido → próxima fase/mapa
7. Fim de partida → recompensas/meta-progressão

---

## 7. Estado Atual do Desenvolvimento

> Atualize esta seção ao final de cada sessão de trabalho.

### ✅ Já funciona
- [ ] (liste o que está pronto e testado)

### 🚧 Em andamento
- [ ] (liste o que está pela metade)

### ⏭️ Próximos passos (próximos 3-5 passos concretos)
1. [próximo passo]
2. [próximo passo]
3. [próximo passo]

### 🕷️ Bugs / Problemas conhecidos
- [ ] (descreva bug e como reproduzir)

---

## 8. Decisões Importantes e Histórico

> Registre aqui TODA decisão que não seja óbvia, para a IA não "inventar" uma alternativa depois.

| Data | Decisão | Motivo |
|---|---|---|
| [data] | Usar MonoGame em vez de Unity | 2D simples, sem editor visual, portabilidade |
| [data] | [decisão] | [por quê] |

---

## 9. Backlog / Roadmap (visão macro)

### Fase 1 — Vertical slice (protótipo jogável)
- [ ] Movimento + 1 golpe + 1 tipo de inimigo + 1 zona
- [ ] Objetivo mínimo: [ex.: limpar 1 zona e sobreviver 3 minutos]

### Fase 2 — Núcleo de gameplay
- [ ] Sistema de hordas e spawn
- [ ] Level up + escolha de poderes
- [ ] 2-3 armas/golpes e 1 magia

### Fase 3 — Estrutura de fases
- [ ] Mapa com múltiplas zonas
- [ ] Conquista e defesa de zonas
- [ ] Progressão entre fases

### Fase 4 — Polimento
- [ ] Efeitos visuais, sons, feedback
- [ ] Balanceamento
- [ ] Menus e opções

---

## 10. Dúvidas Abertas / A Definir

- [ ] O jogo terá armas 100% ativas (botão) ou também armas automáticas passivas?
- [ ] Mapa infinito ou fases com estrutura definida?
- [ ] Controles touch (mobile) entram como requisito desde o início ou só depois?
- [ ] Arte: pixel art, vetorial, ou placeholders simples por enquanto?
- [ ] Iremos usar algum sistema de física (ex.: Aether.Physics2D) ou colisão própria simples?