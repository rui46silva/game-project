# Setup do ambiente de desenvolvimento

Guia passo a passo para preparar o PC (Windows) e deixar o repositório pronto para começarmos a programar.

> **Tempo estimado:** 3 a 5 horas, quase tudo em downloads. O Unreal ocupa cerca de 60 a 100 GB e o Visual Studio cerca de 20 a 40 GB.

---

## Passo 0 — Verificar o PC

- [ ] Windows 10 ou 11, 64 bits, atualizado
- [ ] Pelo menos **200 GB livres num SSD NVMe** (motor + Visual Studio + projeto + cache)
- [ ] 32 GB de RAM (o mínimo confortável) e uma GPU com suporte a ray tracing, para o Lumen
- [ ] Drivers da gráfica atualizados (NVIDIA App ou AMD Adrenalin)

Se tiveres dois discos, instala o motor e o projeto no mais rápido.

---

## Passo 1 — Git e Git LFS

1. Instala o **Git for Windows** a partir de <https://git-scm.com/download/win>. O Git LFS já vem incluído: mantém a opção ativa no instalador.
2. Abre o **Git Bash** e corre cada comando:

```bash
git lfs install
```

```bash
git config --global core.longpaths true
```

```bash
git config --global user.name "Rui Silva"
```

```bash
git config --global user.email "o-teu-email@exemplo.com"
```

O `core.longpaths` evita erros com os caminhos muito longos que o Unreal gera.

3. *(Opcional)* Instala o **GitHub Desktop** ou o **Fork**, se preferires um cliente visual. Os dois suportam LFS.

---

## Passo 2 — Visual Studio (compilador de C++)

O Unreal precisa do compilador da Microsoft para compilar o código C++, **mesmo que uses o Rider como editor**.

1. Descarrega o **Visual Studio 2022 Community** (grátis) a partir de <https://visualstudio.microsoft.com/>.
2. No instalador, escolhe estes **workloads**:
   - [ ] **Desenvolvimento de jogos com C++** (Game development with C++)
   - [ ] **Desenvolvimento para ambiente de trabalho com C++** (Desktop development with C++)
3. No painel da direita, dentro de *Game development with C++*, confirma que estão selecionados:
   - [ ] **Unreal Engine installer** e **IDE support for Unreal Engine**
   - [ ] **Windows 11 SDK**, a versão mais recente disponível
   - [ ] **MSVC v143 – VS 2022 C++ x64/x86 build tools**
4. No separador **Componentes individuais**, adiciona o **.NET 8.0 Runtime** (o Unreal Build Tool usa-o), se não vier já marcado.

> **Sobre o Visual Studio 2026:** a documentação da Epic já o refere, mas há relatos de incompatibilidades com o compilador que ele traz por defeito. Para não perderes tempo, usa o **VS 2022 (17.14)**. Se quiseres mesmo o IDE do 2026, instala também as *build tools* do VS 2022 (MSVC v143) para compilar com elas.

### Alternativa: JetBrains Rider

O Rider tem melhor integração com o Unreal (navegação em Blueprints, refactoring, depuração) e é grátis para uso não comercial. Se o escolheres, faz o Passo 2 na mesma, porque o Rider usa o compilador do Visual Studio. Depois instala o Rider a partir de <https://www.jetbrains.com/rider/> e ativa o suporte a Unreal Engine.

---

## Passo 3 — Epic Games Launcher e Unreal Engine 5

1. Cria uma conta Epic em <https://www.epicgames.com/> e instala o **Epic Games Launcher**.
2. No launcher: **Unreal Engine → Library → +** (ao lado de *Engine Versions*) e escolhe a **versão 5.x mais recente** (5.8 neste momento).
3. Antes de instalar, carrega na seta ao lado de **Install → Options** e seleciona:
   - [ ] **Core Components** (obrigatório)
   - [ ] **Starter Content**
   - [ ] **Templates and Feature Packs** (é aqui que vem o template de veículos)
   - [ ] **Engine Source** (útil para depurar e ler o código do Chaos Vehicles)
   - [ ] **Editor symbols for debugging** (opcional; ocupa muito, mas ajuda a depurar *crashes*)
   - [ ] Desmarca as plataformas que não vais usar (Android, iOS, Linux) para poupar espaço.
4. Instala e abre o motor uma vez para confirmar que arranca.

**Anota a versão exata** (por exemplo, 5.8.1). Todos os que trabalharem no projeto têm de usar a mesma.

---

## Passo 4 — Clonar o repositório

Num disco com espaço, **fora** de pastas sincronizadas (OneDrive, Dropbox, Google Drive), corre cada comando:

```bash
cd /d/Dev
```

```bash
git clone https://github.com/rui46silva/game-project.git
```

```bash
cd game-project
```

```bash
git checkout development/features-and-bugs
```

Usa um caminho curto, como `D:\Dev\game-project`. Caminhos longos dão problemas no Unreal.

---

## Passo 5 — Criar o projeto Unreal dentro do repositório

1. Abre o Unreal Engine e escolhe **Games → Vehicle**.
2. Configura assim:
   - **Project type:** **C++** (não Blueprint; isto é essencial para eu poder escrever código)
   - **Target platform:** Desktop
   - **Quality preset:** Maximum
   - **Starter Content:** sim
   - **Raytracing:** sim
3. **Localização:** a pasta *pai* do repositório (por exemplo, `D:\Dev`).
   **Nome do projeto:** `CostaNeon` (sem espaços nem acentos; se mudarmos o título do jogo, este nome técnico pode ficar).
4. Clica em **Create**. O Unreal cria `D:\Dev\CostaNeon\`.
5. Fecha o editor e **move o conteúdo** de `D:\Dev\CostaNeon\` para a raiz de `D:\Dev\game-project\`. O resultado deve ser:

```
game-project/
├── .gitattributes
├── .gitignore
├── README.md
├── docs/
├── CostaNeon.uproject
├── Config/
├── Content/
├── Source/
│   ├── CostaNeon.Target.cs
│   ├── CostaNeonEditor.Target.cs
│   └── CostaNeon/
└── (Binaries/, Intermediate/, Saved/ e DerivedDataCache/ são ignorados pelo Git)
```

6. Clica com o botão direito no `CostaNeon.uproject` e escolhe **Generate Visual Studio project files**.
7. Abre o `CostaNeon.sln` (ou o `.uproject` no Rider), escolhe a configuração **Development Editor | Win64** e compila (**Ctrl+Shift+B**).
8. Corre com **F5**. O editor deve abrir com o mapa do carro. Carrega em **Play** e conduz um pouco.

> Se o editor perguntar "*Would you like to rebuild them now?*" sobre módulos em falta, responde **Yes**. É normal na primeira vez.

---

## Passo 6 — Primeiro commit do projeto

Confirma primeiro que os ficheiros grandes vão para o LFS e que as pastas geradas ficam de fora:

```bash
git status
```

Não devem aparecer `Binaries/`, `Intermediate/`, `Saved/`, `DerivedDataCache/` nem `.sln`. Se aparecerem, avisa-me antes de fazer commit.

```bash
git add .
```

```bash
git lfs status
```

Os `.uasset` e `.umap` devem aparecer como **LFS**. Se aparecerem como *Git*, pára e avisa-me.

```bash
git commit -m "Add CostaNeon Unreal project from Vehicle template (C++)"
```

```bash
git push origin development/features-and-bugs
```

> **Quota do LFS:** o GitHub tem um limite de armazenamento e de transferência LFS no plano grátis. O template de veículos cabe bem, mas confirma o uso em *GitHub → Settings → Billing and plans* à medida que o projeto crescer.

---

## Passo 7 — Ferramentas de arte e áudio (podem esperar)

Estas não são precisas para começar a programar. Instala-as quando chegares às fases respetivas:

| Ferramenta | Quando | Link |
|---|---|---|
| Blender | Fase 2-3 (cidade, peças de tuning) | <https://www.blender.org/> |
| Krita | Fase 3 (vinis, UI) | <https://krita.org/> |
| FMOD Studio + plugin UE | Fase 1 (som do motor) | <https://www.fmod.com/download> |
| Substance Painter | Fase 3 (pinturas) | <https://www.adobe.com/products/substance3d/apps/painter.html> |
| OBS Studio | Semana 4 (gravar clips) | <https://obsproject.com/> |

---

## ✅ O que preciso que tenhas para eu começar a programar

- [ ] O projeto `CostaNeon` (**C++**, template **Vehicle**) na raiz do repositório
- [ ] O projeto compila e abre sem erros no teu PC
- [ ] O commit enviado para `development/features-and-bugs`, com os assets no LFS
- [ ] A versão exata do Unreal Engine que instalaste (ex.: 5.8.1)
- [ ] O IDE que escolheste (Visual Studio 2022 ou Rider)
- [ ] O comando que vais usar para testar (Xbox, PlayStation, outro)

### Como vamos trabalhar a partir daí

O meu ambiente não tem o Unreal, por isso não consigo compilar nem correr o jogo. O ciclo é este:

1. Eu escrevo o código C++ (e a configuração em `Config/`) e faço push para `development/features-and-bugs`.
2. Tu fazes `git pull`, compilas e testas no editor.
3. Dizes-me o que aconteceu: erros de compilação (copia o texto do *Output*), comportamento estranho, ou o que sentiste ao conduzir.
4. Eu ajusto, até ficar bem. Quando uma funcionalidade estiver estável, abrimos o PR para a `main`.

Assets visuais (mapas, materiais, Blueprints) são ficheiros binários que não consigo editar. Nesses casos, dou-te instruções passo a passo para fazeres no editor, ou exponho os valores em C++ para os afinares no editor sem mexer em código.

### Primeiras tarefas que vou programar (Semana 3 / Fase 1)

1. Câmara de perseguição com **FOV dinâmico** em função da velocidade
2. **Drift assistido** (redução de aderência traseira + correção automática de contrabrecagem)
3. **Nitro** com boost de binário e efeito de câmara
4. Afinação arcade do **Chaos Vehicle Movement Component** (centro de massa, curvas de binário, aderência), com os valores expostos para afinares no editor

---

## Problemas comuns

| Problema | Solução |
|---|---|
| "Could not find a valid Visual Studio installation" | Falta o workload *Game development with C++* ou as build tools MSVC v143. Volta ao Passo 2. |
| "The following modules are missing or built with a different engine version" | Responde *Yes* para recompilar. Se falhar, compila no Visual Studio (Passo 5.7) e vê o erro no *Output*. |
| `git push` muito lento ou a falhar | Normal no primeiro push com LFS. Se der erro de quota, avisa-me. |
| Os `.uasset` aparecem como texto no `git diff` | O LFS não foi instalado antes do `git add`. Corre `git lfs install` e depois `git add --renormalize .` |
| O editor está muito lento | Em *Settings → Engine Scalability*, baixa para *High* enquanto desenvolves. |
