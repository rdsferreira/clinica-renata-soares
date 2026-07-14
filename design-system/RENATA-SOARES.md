# Design System — Renata Soares

Base de marca extraída do site vivo (renatasoares.com.br) para reaproveitar em
novos projetos da marca. Fonte da verdade: `src/styles/global.css` e os
componentes em `src/components/`.

## Cores

| Nome | Hex | CSS var | Uso |
|---|---|---|---|
| Roxo Renata | `#502A77` | `--color-accent` | Cards de conteúdo, header, hero, CTA primário |
| Roxo Escuro | `#381D54` | `--color-accent-dark` | Footer, hover de botões roxos |
| Lima | `#B1C346` | `--color-lime` | CTA de maior contraste, eyebrows sobre roxo |
| Laranja | `#F28F3A` | `--color-orange` | Eyebrows e destaques sobre fundo claro |
| WhatsApp verde | `#25D366` | fixo, não é token de marca | Só no botão flutuante — cor do canal |
| Texto | `#201A29` | `--color-text` | Texto principal sobre fundo claro |
| Texto muted | `#5C5468` | `--color-text-muted` | Parágrafos secundários |
| Fundo alt | `#F7F4FA` | `--color-bg-alt` | Blocos claros dentro de seções brancas |
| Borda | `#E3DCEA` | `--color-border` | Divisórias e bordas sutis |

## Tipografia

- **Heading:** Poppins 700–800, sempre uppercase, `letter-spacing: 0.01em`
- **Body:** Montserrat 400–600
- Escala fluida via `clamp()`:
  - `--font-display`: `clamp(2.25rem, 1.6rem + 2.6vw, 3.75rem)`
  - `--font-h2`: `clamp(1.75rem, 1.4rem + 1.4vw, 2.5rem)`
  - `--font-h3`: `clamp(1.25rem, 1.1rem + 0.6vw, 1.5rem)`
  - `--font-body`: `clamp(1rem, 0.95rem + 0.2vw, 1.125rem)`
  - `--font-small`: `0.9375rem`
- No mobile (`max-width: 640px`), display/h2/h3 recebem overrides fixos
  menores (`1.375rem` / `1.125rem` / `1rem`) — clamp puro fica grande demais
  em telas pequenas.

## Espaço & raio

- `--space-section`: `clamp(1rem, 0.75rem + 1.5vw, 2.5rem)`
- `--radius-card`: `clamp(1.5rem, 1.25rem + 1vw, 2.25rem)`
- `--max-width` (container): `80rem` (1280px)

## Botões — Liquid Glass

Toda ação primária usa o material "Liquid Glass" (blur + saturação + brilho
especular + shimmer no hover). A cor de base do vidro muda conforme o fundo:

- **`.btn-primary`** — branco translúcido, para uso sobre fundo roxo
  (`background: rgb(255 255 255 / 0.18)`, `backdrop-filter: blur(24px) saturate(2) brightness(1.1)`)
- **`.btn-secondary`** — glass mais sutil, sobre roxo (texto branco) ou sobre
  branco (texto escuro, dependendo do contexto)
- **Variante roxa sobre fundo branco** (ex.: `.medpartners__cta`) — roxo
  sólido (`background: var(--color-accent)`) com o mesmo tratamento de glass
  (blur + brilho + shimmer) e borda branca translúcida
- **CTA fixo do header** — pill sólida lima (`--color-lime`), texto
  `--color-accent-dark`, sem glass — precisa de contraste máximo, não vidro
- **WhatsApp flutuante** — círculo verde `#25D366` fixo, nunca roxo (é a cor
  do canal, não da marca)

Todos os botões: `border-radius: 999px`, `min-height: 44px` (alvo de toque),
`text-transform: uppercase`, fonte Poppins 700.

## Cards & superfícies

- **`.card-purple`** — wrapper roxo sólido usado em quase toda seção de
  conteúdo (Metodologia, Depoimentos, FAQ, Contato). Dá o ritmo
  branco → roxo → branco ao scroll.
- **`.inset-panel`** — painel branco dentro do card roxo, para destacar um
  item específico (FAQ, depoimento).
- **`.areas__card`** — foto de fundo + overlay gradiente + texto. O overlay
  ancora o **título no topo do bloco de texto** (`justify-content: flex-start`
  no desktop) para alinhar cards de tamanhos de texto diferentes na mesma
  fileira. No mobile (coluna única, sem fileira pra alinhar) volta a ancorar
  embaixo (`flex-end`) pra cobrir menos imagem.
- Gradiente do overlay: 4 estágios suaves terminando em `transparent` no
  topo — nunca cortar bruscamente (gera uma linha reta visível sobre a foto).

## Faixa de CTA intermediária

Padrão para reforçar conversão entre seções longas. Hoje ativo só no
**mobile** (`@media (min-width: 641px) { display: none; }`) — no desktop já
há CTA suficiente (header fixo + hero + seção final).

## Ícones

- Sempre SVG inline, nunca emoji ou biblioteca externa
- `stroke-width` entre 1.5–1.8
- Tamanho 16–20px em contexto de texto, 18–24px em botões

## Fotografia

- Fotos reais da clínica/atendimentos — nunca banco de imagem genérico
- Nunca cortar cabeça, braço ou perna da criança/adolescente no enquadramento
- Formatos: Hero 1:1 · Retrato (Sobre) 3:4 · Card de especialidade 1:1
  (3:4 no mobile) · Galeria em linhas justificadas de altura igual
- Otimização: WebP, 1600px no lado maior, qualidade ~78

## SEO & dados estruturados

Todo projeto novo da marca deve sair com:

- `@astrojs/sitemap` configurado, `site` definido em `astro.config.mjs`
- `robots.txt` apontando pro sitemap
- Open Graph + Twitter Card completos (`og:image` 1200×630)
- `llms.txt` na raiz — resumo estruturado pra buscadores de IA
- Schema JSON-LD tipado ao serviço real (ex.: `PhysicalTherapy`, não o
  genérico `MedicalBusiness`), sempre com `geo`, `priceRange` e `image`
- `FAQPage` schema em toda seção de perguntas frequentes

```json
{
  "@context": "https://schema.org",
  "@type": "PhysicalTherapy",
  "name": "Clínica Renata Soares — Fisioterapia Pediátrica",
  "priceRange": "$$",
  "geo": { "@type": "GeoCoordinates", "latitude": -23.4953194, "longitude": -46.8470664 },
  "openingHoursSpecification": { "opens": "08:00", "closes": "20:00" }
}
```

## Como usar em um novo projeto

1. **Parta do repo `landingbase`** — base Astro já configurada para
   Lighthouse alto (CSS crítico, zero JS por padrão).
2. **Copie os tokens de `global.css`** — cores, fontes, espaçamento e
   `--radius-card` vêm prontos.
3. **Reutilize `.btn-primary`, `.card-purple` e `.areas__overlay`** — são os
   três blocos que carregam a identidade visual mais forte; não redesenhar
   do zero.
4. **Mantenha o ritmo branco → roxo → branco** entre seções.
5. **Adapte o schema.org ao tipo de página/serviço**, mas mantenha `geo`,
   `priceRange` e `image` sempre preenchidos.

---

Referência visual completa (cores, tipografia e componentes renderizados):
ver artifact publicado na conversa, ou reabrir `design-system/RENATA-SOARES.md`
como ponto de partida em texto.
