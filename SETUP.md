# Como instalar isso no seu perfil

## 1. Copiar os arquivos
Copie todo o conteúdo deste pacote para dentro do repositório `GustavoLimaDias/GustavoLimaDias`
(o repositório especial cujo README aparece no topo do seu perfil):

```
GustavoLimaDias/
├── README.md
├── gustavo-ascii.svg
├── neofetch-card.svg
├── contrib-heatmap.svg
├── source-photo.png          (sua foto original — só precisa se for regenerar o ASCII)
├── data/
│   └── contributions.json
├── scripts/
│   ├── requirements.txt
│   ├── prep_photo.py
│   ├── make_ascii_svg.py
│   ├── make_info_card.py
│   ├── fetch_contributions.py
│   └── render_heatmap_svg.py
└── .github/
    └── workflows/
        └── update-profile-art.yml
```

## 2. Commit e push

```
git add .
git commit -m "feat: perfil estilo terminal com ASCII art e heatmap animado"
git push
```

## 3. Ativar a automação
Vá na aba **Actions** do repositório → workflow **"Update profile art"** →
**Run workflow** (botão `workflow_dispatch`) para rodar uma vez manualmente e
confirmar que ele commita um `contrib-heatmap.svg` atualizado. Depois disso,
ele roda sozinho todo dia às ~06:17 UTC.

O workflow só reinstala `requests` e `beautifulsoup4` (rápido) — as libs de
imagem (`pillow`, `numpy`, `opencv`, `rembg`) só são necessárias quando você
for trocar a foto e regerar o ASCII art localmente, não entram na automação
diária.

## 4. Trocar a foto no futuro
```
python -m venv .venv && source .venv/bin/activate
pip install -r scripts/requirements.txt

python scripts/prep_photo.py source-photo.png            # remove fundo + contraste
python scripts/make_ascii_svg.py                          # gera gustavo-ascii.svg
```

Se o fundo da nova foto tiver algo perto do corpo (planta, objeto) que o
`rembg` acabe "grudando" na silhueta, passe um recorte manual como segundo
argumento — `left,top,right,bottom` em pixels:

```
python scripts/prep_photo.py source-photo.png "130,10,345,460"
```

## 5. Editar o card neofetch
As linhas mostradas (`os`, `role`, `prev`, `stack`, etc.) estão na lista
`ROWS` no topo de `scripts/make_info_card.py` — edite o texto ali e rode
`python scripts/make_info_card.py` de novo.

## Notas
- Nenhum script usa token do GitHub. Os dados de contribuição vêm da mesma
  página HTML pública que o seu perfil já usa
  (`github.com/users/<usuário>/contributions`).
- Toda a animação vive dentro dos próprios SVGs (SMIL/CSS), porque o GitHub
  remove `<script>` e a maior parte do CSS inline dos READMEs — mas roda as
  animações internas de SVGs embutidos via `<img>`.
- Os badges de stats/streak/activity no fim do README continuam usando os
  serviços de terceiros que você já tinha (github-readme-stats, streak-stats,
  profile-summary-cards) — mantive porque cobrem métricas que os scripts
  acima não calculam (linguagens mais usadas, ranking, etc.).
