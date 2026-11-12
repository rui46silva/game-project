# Projeto Underground — Plano de Desenvolvimento

> Documento de referência (versão de 4 nov. 2026, Rui Silva). O guião está em [GUIAO.md](GUIAO.md).

## 1. Conceito

**Nome provisório:** *Costa Néon* (título de trabalho, troca quando quiseres)

**Pitch numa frase:** Um jogo de corridas de rua noturno em mundo aberto, com tuning visual profundo e uma história de ascensão na cena underground, onde no fim a cidade passa a ser partilhada online com outros jogadores.

**Plataforma inicial:** PC (Steam). Consolas ficam para depois de o jogo provar que funciona.

### Pilares de design

Tudo o que entra no jogo tem de servir pelo menos um destes pilares. Se não servir, fica de fora.

1. **Condução arcade com sensação de velocidade.** Drift fácil de fazer e difícil de dominar, câmara e efeitos que fazem 200 km/h parecer perigoso. Não é um simulador.
2. **O teu carro é a tua identidade.** Kits de carroçaria, jantes, vinis em camadas, néons, vidros fumados, sistemas de som. Um carro deve ser reconhecível à distância.
3. **A cidade à noite.** Sempre noite, estrada molhada, reflexos, néons, nevoeiro de porto. O ambiente é uma personagem.
4. **Reputação na cena.** Ganhas respeito a correr, a ter estilo e a derrotar crews. A reputação abre distritos, peças e eventos.
5. **Online sem fricção.** Encontrar outros jogadores no mundo aberto e desafiá-los deve demorar segundos, como piscar os faróis num semáforo.

### O que herdamos do espírito Underground

- Tipos de prova: circuito, sprint, drift, drag e eliminação (o último é eliminado a cada volta).
- Desafios espontâneos no mundo aberto: encostas a outro carro e começa a corrida.
- Classificação de estilo do carro: quanto mais trabalhado, mais "capas de revista" e patrocínios desbloqueias.
- Uma mentora/oficina como centro da história.

### O que fazemos diferente

- Cidade original de inspiração ibérica, nada de cópias de cidades americanas.
- História mais madura e pessoal, com consequências.
- Online integrado desde o início, não um modo à parte.
- Carros fictícios inspirados em clássicos JDM, europeus e muscle (sem marcas reais, para evitar licenças).

## 2. Stack para PC

| Área | Ferramenta | Porquê | Custo |
|---|---|---|---|
| Motor de jogo | Unreal Engine 5 | Chaos Vehicles, World Partition para mundo aberto, Lumen para iluminação noturna, replicação multiplayer de base | Grátis; 5% de royalties acima de 1M$ de receita |
| IDE | Visual Studio 2022 ou JetBrains Rider | C++ do Unreal; o Rider tem melhor integração com UE | VS Community grátis; Rider grátis para uso não comercial |
| Modelação 3D | Blender | Carros, peças de tuning, edifícios | Grátis |
| Texturas | Substance Painter + Megascans (Fab) | Pintura metalizada, carbono, cromados; texturas de cidade prontas | Subscrição / parte grátis no Fab |
| 2D / UI / vinis | Krita, Photoshop ou Affinity | Vinis, logos de crews, interface | Krita grátis |
| Áudio | FMOD | Som de motor dinâmico (rotações, carga, turbo, escape) | Grátis para projetos indie pequenos (confirmar condições atuais) |
| Controlo de versões | Git + Git LFS (GitHub) ou Perforce | Ficheiros binários grandes do Unreal | Perforce grátis até poucos utilizadores |
| Backend online | Epic Online Services | Contas, amigos, lobbies, matchmaking, cross-play futuro | Grátis |
| Alternativa backend | Nakama | Open source, lógica de servidor em TypeScript (encaixa no teu Node.js) | Self-hosted ou pago |
| Loja | Steamworks | Página Steam, conquistas, multiplayer Steam, demos | Taxa única de 100 USD por jogo |
| Gestão | Notion, Trello ou GitHub Projects | Backlog e roadmap | Grátis |

### Hardware recomendado

- GPU com bom desempenho em ray tracing (para trabalhar com Lumen confortavelmente).
- 32 GB de RAM no mínimo, 64 GB ideal para mundo aberto.
- SSD NVMe com bastante espaço livre: o motor, os projetos e os assets ocupam centenas de GB.
- Um volante e um comando para testar a condução. O jogo tem de ser bom no comando acima de tudo.

### Assets a comprar em vez de fazer

Sozinho, isto poupa-te anos:

- Packs de cidade modular (edifícios, estradas, mobiliário urbano).
- Um modelo de física de veículos já afinado como ponto de partida, se o Chaos Vehicles não te der o feeling arcade.
- Packs de efeitos visuais (chuva, faíscas, fumo de pneus, nitro).
- Bibliotecas de som de motores gravados.

## 3. Roadmap passo a passo

Prazos pensados para uma pessoa a trabalhar em part-time (cerca de 15-20 horas por semana). Com equipa ou a tempo inteiro, encurtam bastante.

### Fase 0 — Aprender o motor (1-2 meses)

- Fazer os cursos gratuitos do Epic Developer Community (introdução ao Unreal, Blueprints, materiais, iluminação).
- Aprender o básico de C++ no Unreal: classes, Actors, Components, a estrutura de um projeto.
- Fazer o tutorial de Chaos Vehicles e pôr um carro a andar.
- Aprender Git LFS com um projeto Unreal.
- **Entregável:** um carro a andar num terreno plano, com câmara de perseguição.

### Fase 1 — Protótipo de condução (2-3 meses)

- Afinar a física para arcade: aderência, drift assistido, transferência de peso, nitro.
- Câmara dinâmica: FOV aumenta com a velocidade, vibração ligeira, motion blur.
- Som de motor básico com FMOD (rotações e mudanças).
- Efeitos: fumo dos pneus, faíscas, marcas na estrada.
- Testar com comando, volante e teclado.
- **Entregável:** uma pista de teste onde andar de carro é divertido por si só. **Não avançar sem isto.**

### Fase 2 — Um distrito com o ambiente certo (2-3 meses)

- Construir um distrito pequeno (por exemplo, as Docas) com assets modulares.
- Noite permanente com Lumen, estrada molhada, reflexos, néons, nevoeiro.
- Configurar World Partition desde o início, para crescer depois sem refazer.
- Tráfego simples: carros a seguir splines nas estradas.
- **Entregável:** um vídeo de 30 segundos a conduzir à noite que já pareça o teu jogo.

### Fase 3 — Garagem e tuning (2-3 meses)

- Sistema de peças modulares: para-choques, saias, spoilers, jantes, capôs.
- Editor de pintura: cor, material (metalizado, mate, cromado), vinis em camadas.
- Néons, vidros, suspensão (altura).
- Upgrades de desempenho (motor, turbo, travões, pneus) com efeito real na física.
- Guardar e carregar carros (save system).
- **Entregável:** 2-3 carros completamente personalizáveis.

### Fase 4 — Corridas e IA (2 meses)

- Sistema de provas: circuito, sprint, drift (pontuação), drag (mudanças manuais), eliminação.
- IA de adversários a seguir splines de corrida, com dificuldade ajustável (rubber banding moderado).
- Desafios espontâneos no mundo aberto.
- HUD: velocímetro, mapa, posição, tempo.
- **Entregável:** 5 provas jogáveis no distrito.

### Fase 5 — Multiplayer (3-4 meses)

- Começar com servidor local (listen server) para 2-4 jogadores no mesmo mundo.
- Replicação dos carros com previsão e interpolação, para não haver saltos.
- Corridas online com início sincronizado e resultados validados.
- Integração com EOS ou Steam: lobbies, convites de amigos.
- Depois: servidores dedicados para sessões de free roam com 8-16 jogadores.
- **Entregável:** tu e 3 amigos a correr juntos sem lag visível.

### Fase 6 — Vertical slice e Ato I da história (3 meses)

- Prólogo e Ato I completos (ver [guião](GUIAO.md)).
- Cenas narrativas simples (diálogos com retratos e voz opcional, ou cenas no motor).
- Sistema de reputação e progressão.
- Menus, opções gráficas, comandos remapeáveis.
- **Entregável:** uma demo de 30-60 minutos que mostra o jogo como deve ser.

### Fase 7 — Steam e comunidade (contínuo, começa na Fase 6)

- Criar a página Steam cedo, para acumular wishlists.
- Discord, TikTok e YouTube com clips do desenvolvimento (os carros personalizados dão ótimo conteúdo).
- Playtests fechados com a comunidade.
- Demo no Steam Next Fest.
- Lançamento em Early Access com Docas + Centro Velho, e expandir com o dinheiro e o feedback.

### Fase 8 — Expansão

- Novos distritos, carros, Atos II e III, eventos online, crews de jogadores.
- Só aqui pensar em consolas.

## 4. Primeiras 4 semanas

### Semana 1 — Preparar

- [ ] Instalar o Epic Games Launcher e a versão mais recente do Unreal Engine 5
- [ ] Instalar Visual Studio 2022 (com o workload de desenvolvimento de jogos em C++) ou Rider
- [ ] Instalar Blender e Git com Git LFS
- [x] Criar o repositório com o `.gitignore` e `.gitattributes` próprios para Unreal
- [ ] Começar o curso introdutório do Unreal no Epic Developer Community

### Semana 2 — Primeiro carro

- [ ] Criar um projeto a partir do template de veículos do Unreal
- [ ] Perceber como funciona o Chaos Vehicle Movement Component (motor, caixa, rodas, suspensão)
- [ ] Mudar valores e ver o efeito na condução
- [ ] Configurar o comando

### Semana 3 — Sensação de velocidade

- [ ] Câmara de perseguição com FOV dinâmico
- [ ] Primeiro teste de drift assistido
- [ ] Partículas de fumo nos pneus
- [ ] Construir uma pista de teste simples com curvas, retas e um gancho

### Semana 4 — Mostrar a alguém

- [ ] Gravar um clip de 15 segundos
- [ ] Pedir a 2-3 amigos que conduzam e anotar o que dizem
- [ ] Escrever a lista de afinações para o mês seguinte
- [ ] Rever este documento e ajustar os prazos à realidade

## 8. Ligação ao online e riscos

### Como a história alimenta o online

- **A história é o tutorial longo.** Quem acaba o Ato I já sabe conduzir, afinar o carro e conhece o mapa.
- **Crews de jogadores.** As crews da história passam a ser o modelo das crews online: nome, logo (vinil), cores e garagem partilhada.
- **Territórios.** No online, as crews de jogadores disputam distritos em eventos semanais, como as crews da história.
- **Sexta-feira no Parque do Mercado.** Lobby social semanal: exposição de carros, votações de estilo, desafios.
- **Personagens vivas.** A Lena, a Inês e as líderes das crews dão missões diárias e semanais no online, para a história continuar.
- **Reputação única.** A mesma progressão em solo e online, para nunca sentires que estás a jogar dois jogos.

### Riscos principais e como reduzi-los

| Risco | Impacto | Como reduzir |
|---|---|---|
| Âmbito demasiado grande | O projeto nunca acaba | Lançar com 2 distritos e o Ato I; tudo o resto é expansão |
| Condução que não é divertida | O jogo falha por inteiro | Não sair da Fase 1 sem testes com outras pessoas |
| Netcode com lag | Online inutilizável | Protótipo de multiplayer cedo, mesmo feio; testes com amigos noutras redes |
| Licenças de carros e marcas | Problemas legais | Carros fictícios; zero referências a NFS ou marcas reais |
| Custos de servidores | Despesa mensal sem receita | Começar com sessões peer-to-peer/listen server; dedicados só com jogadores |
| Burnout a trabalhar sozinho | Projeto abandonado | Metas mensais pequenas, mostrar progresso à comunidade, procurar 1-2 colaboradores (artista 3D e áudio) |

### Próximas decisões

- [ ] Confirmar o nome do jogo e da cidade.
- [ ] Decidir o estilo das cenas narrativas (diálogos simples com retratos são muito mais baratos do que cinemáticas).
- [ ] Escolher os 3 carros base do Ato I e desenhar o visual de cada um.
