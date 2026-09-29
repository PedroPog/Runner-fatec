# 🏃‍♂️ Runner — Jogo Endless Runner 2D (GDevelop 5)

Um jogo no estilo **Endless Runner vertical** (inspirado em *Subway Surfers*) desenvolvido no **GDevelop 5** como projeto acadêmico (FATEC). O jogador corre por uma pista infinita de 3 faixas com aceleração progressiva e precisa reagir rapidamente para saltar, deslizar ou desviar de diferentes tipos de obstáculos.

---

## 🎮 Mecânicas Principais

- **Sistema de 3 Faixas (Lanes):** Movimentação lateral suave utilizando interpolação (`Tween easeOutQuad`) calculada dinamicamente a partir do centro da tela (`BaseX`) e da largura da faixa (`LaneWidth = 160px`).
- **Estados do Jogador (`PlayerState`):**
  - `IDLE`: Aguardando início da partida ao pressionar qualquer tecla.
  - `RUNNING`: Corrida contínua na pista com animação em loop.
  - `JUMPING`: Salto com efeito de escala (`Tween Scale 1.3x`) e transição para animação de queda (`fall`), permitindo ultrapassar obstáculos rasteiros.
  - `SLIDING`: Deslize/abaixada com compressão de escala (`Tween Scale Y 0.5x`), permitindo passar por baixo de obstáculos suspensos.
  - `DEAD`: Estado de Game Over acionado em colisões, trazendo o personagem para o topo da camada (`Z-Order = 5`) e interrompendo a rolagem da pista.
- **Arquitetura Hitbox + Skin Separada:**
  - O objeto `Player` (`128x128`) atua como **Hitbox invisível** com máscara de colisão otimizada (`X: 0..128`, `Y: 24..104`).
  - Os objetos visuais na pasta `Personagens` (`Personagem`, `Red`) sincronizam posição (`X/Y`), escala, `Z-Order` e animações em tempo real com o `Player`, facilitando a troca de skins/personagens sem afetar a física de colisão.
- **Pista Infinita e Dificuldade Progressiva:**
  - Rolagem contínua via `Tiled Sprite` (`Floor`) utilizando `TrackSpeed * TimeDelta()`.
  - Aumento automático de velocidade (`+30 px/s` a cada 5 segundos via `TimerScale`, até o limite de `900 px/s`).
- **Geração Procedural de 3 Tipos de Obstáculos (`TimerSpawn`):**
  - **Obstáculo Baixo (`ObsBaixo` - `Z-Order: 1`):** Exige que o jogador **salte** (`JUMPING`).
  - **Obstáculo Alto (`ObsAlto` - `Z-Order: 3`):** Exige que o jogador **abaixe/deslize** (`SLIDING`).
  - **Obstáculo de Bloqueio (`ObsBloqueio` - `Z-Order: 2`):** Bloqueio total que não permite pular nem abaixar, exigindo **troca de faixa**.
  - Todos os obstáculos ocupam automaticamente **85% da largura da faixa (`136px`)** e são reciclados/destruídos ao saírem da tela (`Y > 1350`).

---

## 🕹️ Controles

| Tecla | Ação |
| :---: | :--- |
| **Qualquer Tecla** | Inicia a partida (sai do modo `IDLE`) |
| **Seta para Esquerda (`Left`)** | Move para a faixa da esquerda |
| **Seta para Direita (`Right`)** | Move para a faixa da direita |
| **Seta para Cima (`Up`)** | Salta sobre obstáculos baixos (`ObsBaixo`) |
| **Seta para Baixo (`Down`)** | Desliza sob obstáculos altos (`ObsAlto`) |

---

## 📁 Estrutura do Projeto

```text
Runner/
├── Runner.json          # Arquivo principal do projeto GDevelop 5 (cenas, objetos, variáveis e eventos)
├── assets/              # Recursos gráficos (Sprites Pixel Art 128x128 e Texturas de Pista)
│   ├── Char_*.png       # Sprites das 8 animações do personagem (Idle, Run, Jump, Fall, Left, Right, Slide, Death)
│   ├── ObsAlto.png      # Sprite do obstáculo suspenso
│   ├── ObsBloqueio.png  # Sprite do obstáculo de bloqueio total
│   └── ...
└── README.md            # Documentação do projeto
```

---

## 🚀 Como Executar e Editar

1. Baixe e instale o [GDevelop 5](https://gdevelop.io/).
2. Abra o GDevelop 5 e clique em **Open a project** (Abrir um projeto).
3. Selecione o arquivo `Runner.json` na raiz deste repositório.
4. Clique no botão **Preview** (Visualizar) no topo da interface para jogar e testar.
