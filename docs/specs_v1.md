# ⚙️ PROMPT DE RETOMADA (cole no início de toda conversa nova)

> Você é um desenvolvedor sênior de jogos 2D usando MonoGame em C#. Leia o documento "specs.md" abaixo como a fonte completa do projeto. Continue o trabalho de onde parou, respeitando TODAS as convenções, a arquitetura e as decisões registradas. Não reescreva sistemas que já estão marcados como concluídos. **Priorize sempre o Escopo Mínimo (Fase 1) antes de avançar para as fases seguintes.** Ao final de qualquer mudança, sugira a atualização da seção "7. Estado Atual" com um resumo simples e objetivo (o que mudou / o que falta).

---

# PROJETO: LOLLIPOP BEAR

## 1. Visão Geral

### 1.1 Pitch (1 parágrafo)

Lollipop Bear é um action survivor de arena 2D top-down onde um urso estilizado enfrenta hordas de inimigos usando pirulitos como arma. O combate mistura golpes ativos acionados por botões com a pressão constante de hordas, em um mapa dividido em zonas que precisam ser conquistadas e defendidas — tudo com um tom leve e visual carismático.

### 1.2 Gênero e Referências

- **Gênero principal:** Action / Arena Survivor / Musou 2D top-down
- **Referências de mecânicas:**
  - _Vampire Survivors_ (hordas, poderes por nível, progressão)
  - _Hyrule Warriors_ (áreas de batalha, zonas, combate com golpes)
  - _The Legend of Zelda: A Link to the Past_ (combate direcional e exploração de áreas, sem ser um RPG tradicional)

### 1.3 Experiência alvo (o que o jogador deve sentir)

- Combates frenéticos e satisfatórios, onde o jogador se sente poderoso ao limpar hordas de inimigos com golpes bem colocados;
- Tensão de defesa de territórios, com a sensação de que a zona pode ser retomada a qualquer momento;
- Progressão constante e recompensadora, onde cada nível e cada novo poder muda o jeito de jogar;
- Tom leve e divertido, reforçado pela estética do urso com pirulitos e por inimigos caprichados.

### 1.4 Escopo: mínimo vs. completo

> **REGRA DE OURO:** este projeto é dividido em **Escopo Mínimo** (o que dá para aprender e terminar primeiro) e **Escopo Completo** (para onde o jogo evolui depois). A IA deve sempre priorizar o Mínimo; o Completo é implementado só depois que o Mínimo estiver estável.

|                  | Escopo Mínimo (Fase 1)                        | Escopo Completo (Fases 2-4)                     |
| ---------------- | --------------------------------------------- | ----------------------------------------------- |
| **Objetivo**     | Aprender os fundamentos e ter um jogo jogável | Expandir o jogo até a visão completa            |
| **Mapa**         | 1 zona/arena só                               | Mapa com múltiplas zonas, conquista e defesa    |
| **Combate**      | 1 golpe ativo por botão                       | Vários golpes, combos, magias                   |
| **Inimigos**     | 1 tipo (corpo a corpo simples)                | Vários tipos + hordas com dificuldade crescente |
| **Progressão**   | XP + level up + 2-3 poderes                   | Armas/magias novas, meta-progressão             |
| **Quando rodar** | JÁ — primeira meta concreta                   | Depois, aos poucos                              |

### 1.5 Plataformas alvo

| Plataforma           | Prioridade | Observações                               |
| -------------------- | ---------- | ----------------------------------------- |
| Windows Desktop      | Alta       |                                           |
| Linux                | Média      |                                           |
| macOS                | Baixa      |                                           |
| Web (via Wasm/outro) | Média      |                                           |
| Mobile (Android/iOS) | Futuro     | depende de controles touch                |
| Consoles             | Futuro     | MonoGame suporta, mas exige licenciamento |

---

## 2. Stack e Setup

### 2.1 Ferramentas

- **Engine/Framework:** MonoGame
- **Linguagem:** C#
- **Versão .NET:** .NET 10
- **IDE:** VS Code
- **Controle de versão:** Git (repositório: [[link](https://github.com/marcelobonatto/lollipop-bear)])

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

- **Atualização de estado:** GameStateManager + enum GameState
- **Organização de entidades:** classes simples + herança de `Entity`
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
- Estilo: cartunizado como desenho animado.
- Resolução base: Full HD (1920 x 1080)

---

## 5. Mecânicas Principais

> 🎯 Itens marcados com **"[MÍNIMO]"** fazem parte do Escopo Mínimo (Fase 1) e devem ser implementados primeiro. Os demais entram nas fases seguintes.

### 5.1 Personagem / Movimento [MÍNIMO]

- [ ] Movimento em 8 direções (top-down)
- [ ] Colisão com paredes/cenário
- [ ] Atributos base: vida, velocidade, dano, defesa
- Detalhes: [descreva controles, física, valores base]

### 5.2 Combate ativo (golpes por botão) [MÍNIMO]

- [ ] Golpes acionados por botões (não apenas automáticos)
- [ ] Combos ou golpes independentes?
- [ ] Hitboxes melee (área de dano na frente do personagem)
- [ ] Tempo de recarga/cooldown de cada golpe
- Detalhes: [descreva botões, tipos de golpe, como funciona o dano]

### 5.3 Magias

- [ ] Sistema de magias com custo/recurso (mana? cooldowns?)
- [ ] Tipos de magia: [área, projétil, buff, etc.]
- [ ] Como o jogador desbloqueia magias
- Detalhes: o urso se move com WASD ou setas ou ainda pelos direcionais de joysticks,
  velocidade base de 200px/s, vida inicial de 100, dano base do pirulito
  de 15. Colisão com paredes da arena é sólida (não atravessa), sem deslizamento
  diagonal.

### 5.4 Armas / Ataques automáticos (inspiração arena survivor — se mantido)

- [ ] Armas que disparam automaticamente ao longo do tempo
- [ ] Armas como power-ups passivos que somam ao combate ativo?
- Detalhes: [decida se o jogo terá apenas combate ativo ou os dois — ativo + automático]

### 5.5 Inimigos e Hordas [MÍNIMO — 1 tipo + spawn simples]

- [ ] Spawn contínuo de hordas
- [ ] Tipos de inimigos: [básico, rápido, tanque, ranged, elite, chefe]
- [ ] Dificuldade crescente (escala com tempo ou zonas)
- Detalhes: [descreva cada tipo e seu comportamento]

### 5.6 Progressão e Poderes (escolha por nível) [MÍNIMO — XP + level up + 2-3 poderes]

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

1. Seleção de personagem/poder inicial?
2. Jogador entra no mapa com N zonas
3. Conquista zonas lutando contra hordas
4. Sobe de nível e escolhe poderes
5. Inimigos tentam reconquistar zonas → jogador defende
6. Objetivo da fase cumprido → próxima fase/mapa
7. Fim de partida → recompensas/meta-progressão

> 🎯 **Fluxo da Fase 1 (mínimo):** jogador entra na zona → mata hordas → ganha XP → sobe de nível → escolhe poderes → sobrevive/limpa a zona. Os passos 2, 3, 5 e 6 completos entram nas fases seguintes.

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

| Data   | Decisão                       | Motivo                                       |
| ------ | ----------------------------- | -------------------------------------------- |
| [data] | Usar MonoGame em vez de Unity | 2D simples, sem editor visual, portabilidade |
| [data] | [decisão]                     | [por quê]                                    |

---

## 9. Backlog / Roadmap (visão macro)

### 🎯 FASE 1 — ESCOPO MÍNIMO (prioridade máxima: faça isto PRIMEIRO)

> É este o jogo que você deve conseguir terminar e sentir progresso. Não avance para as fases seguintes enquanto esta não estiver estável.

**Objetivo jogável:** [ex.: limpar 1 zona com hordas e sobreviver 3-5 minutos, subindo de nível e escolhendo poderes]

- [ ] Movimento em 8 direções com colisão (paredes/cenário)
- [ ] 1 golpe ativo acionado por botão (hitbox melee + cooldown)
- [ ] 1 tipo de inimigo que persegue o jogador (corpo a corpo)
- [ ] Spawn contínuo simples de inimigos na zona
- [ ] XP ao derrotar inimigos
- [ ] Level up + tela de escolha entre 2-3 poderes básicos (ex.: +dano, +velocidade, cura)
- [ ] HUD mínimo (vida, XP, nível, cronômetro)
- [ ] 1 zona/arena fechada com paredes

**Critério de conclusão da Fase 1:** o jogo roda, o jogador mata hordas, sobe de nível, escolhe poderes e sobrevive/limpa a zona.

### Fase 2 — Núcleo de gameplay

- [ ] Sistema de hordas e spawn com dificuldade crescente
- [ ] Level up + escolha de poderes expandida (novos poderes)
- [ ] 2-3 armas/golpes e 1 magia
- [ ] Tipos de inimigos: rápido, tanque, ranged

### Fase 3 — Estrutura de fases

- [ ] Mapa com múltiplas zonas (não infinito)
- [ ] Conquista e defesa de zonas (inimigos retomam zonas abertas)
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
