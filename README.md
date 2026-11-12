# Costa Néon *(título de trabalho)* — Projeto Underground

Jogo de corridas de rua noturno em mundo aberto, com tuning visual profundo e uma história de ascensão na cena underground de uma cidade portuária fictícia de inspiração ibérica. No fim da história, a cidade passa a ser partilhada online com outros jogadores.

- **Plataforma inicial:** PC (Steam)
- **Motor:** Unreal Engine 5 (Chaos Vehicles, World Partition, Lumen)
- **Estado atual:** Fase 0, a aprender o motor e a preparar o repositório

## Documentação

| Documento | Conteúdo |
|---|---|
| [docs/PLANO_DESENVOLVIMENTO.md](docs/PLANO_DESENVOLVIMENTO.md) | Conceito, pilares de design, stack, roadmap (Fases 0-8), primeiras 4 semanas, riscos |
| [docs/GUIAO.md](docs/GUIAO.md) | Mundo (Costa Néon e distritos), personagens e história (Prólogo, Atos I-III, Epílogo) |
| [docs/SETUP_AMBIENTE.md](docs/SETUP_AMBIENTE.md) | Instalação passo a passo do ambiente (Git LFS, Visual Studio, Unreal) e criação do projeto |

## Pilares de design

1. Condução arcade com sensação de velocidade
2. O teu carro é a tua identidade
3. A cidade à noite
4. Reputação na cena
5. Online sem fricção

Tudo o que entra no jogo tem de servir pelo menos um destes pilares.

## Fluxo de trabalho (Git)

- `main`: versão estável. Só recebe alterações por pull request.
- `development/features-and-bugs`: branch de desenvolvimento, onde entram as funcionalidades e as correções antes do PR para a `main`.

### Git LFS

Os assets binários (`.uasset`, `.umap`, modelos, texturas, áudio) são guardados com Git LFS, conforme configurado no [`.gitattributes`](.gitattributes). Depois de clonar, corre:

```bash
git lfs install
```

```bash
git lfs pull
```

Os `.uasset` e `.umap` estão marcados como `lockable`. Antes de editares um mapa ou asset partilhado, bloqueia-o com `git lfs lock <ficheiro>` para evitar conflitos impossíveis de juntar.
