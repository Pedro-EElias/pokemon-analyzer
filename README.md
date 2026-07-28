# Pokémon Analyzer

Pipeline automatizado de ETL que coleta dados de todos os Pokémon via [PokéAPI](https://pokeapi.co/) e consolida em uma base estruturada (`.xlsx`), atualizada automaticamente toda semana via GitHub Actions.

## O que o projeto faz

1. **Extração**: consulta a PokéAPI para cada Pokémon (ID 1 a 1025), cobrindo todas as gerações — de Kanto a Paldea.
2. **Transformação**: classifica cada Pokémon por região/geração e organiza atributos base (HP, Ataque, Defesa, Ataque Especial, Defesa Especial, Velocidade) e tipos (Tipo 1 / Tipo 2).
3. **Carga**: exporta o resultado para `data/base_pokemon.xlsx`.
4. **Automação**: um workflow de GitHub Actions (`.github/workflows/`) roda o coletor automaticamente a cada push na `main` e semanalmente (domingo, meia-noite), commitando a base atualizada de volta no repositório.

## Estrutura

```
pokemon-analyzer/
├── scripts/
│   └── colector.py       # Script de extração/transformação (PokéAPI → DataFrame → Excel)
├── data/
│   └── base_pokemon.xlsx # Base de dados gerada automaticamente
├── requirements.txt
└── .github/workflows/    # Pipeline de CI/CD que roda o coletor periodicamente
```

## Tecnologias

- **Python** (pandas, requests, openpyxl)
- **PokéAPI** como fonte de dados
- **GitHub Actions** para agendamento e automação (cron semanal + trigger em push)

## Como rodar localmente

```bash
pip install -r requirements.txt
python scripts/colector.py
```

Por padrão, coleta todas as regiões. Para uma região específica, edite a chamada de `extract_data()` no `colector.py` passando um dos valores de `regions_limits` (ex: `"Kanto"`, `"Hoenn"`).

## Roadmap / próximos passos

- [ ] Adicionar testes automatizados para validar integridade dos dados coletados
- [ ] Tratar falhas de request (retry / rate limit da PokéAPI)
- [ ] Camada de análise exploratória sobre a base gerada (distribuição de tipos, stats por geração, etc.)
- [ ] Versionar a base em formato mais leve para diffs (ex: Parquet ou CSV) além do Excel
