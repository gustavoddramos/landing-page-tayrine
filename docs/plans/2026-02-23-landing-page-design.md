# Design: Landing Page Dra. Tayrine Duarte

**Data**: 2026-02-23
**Status**: Aprovado

## Resumo

Landing page estática para a Dra. Tayrine Duarte, médica com foco em cuidados com pele, cabelos e unhas em Marabá/PA. Site otimizado para SEO e Google Ads, hospedado no GitHub Pages com domínio tayrineduarte.com.br.

**Restrição importante**: Não usar os termos "dermatologista", "dermatologia" ou similar — focar em "cuidados com pele, cabelos e unhas".

## Dados da Profissional

- **Nome**: Dra. Tayrine Duarte
- **CRM**: 17.693/PA
- **Formação**: Medicina (Universidade São Lucas, Porto Velho/RO), Pós-graduação em saúde da pele (ISMD, Belo Horizonte/MG)
- **Endereço**: Av. Nagib Mutran 229, Cidade Nova, Marabá/PA
- **WhatsApp**: (94) 98433-6074
- **Email**: consultorio@tayrineduarte.com.br
- **Instagram**: @tayrineduartee
- **Domínio**: tayrineduarte.com.br

## Arquitetura

- HTML estático + Tailwind CSS (build via CLI)
- Carrossel CSS puro (scroll-snap) + JS mínimo
- GitHub Pages para hospedagem
- GitHub Actions para build automático

### Estrutura de arquivos

```
landing_page_tayrine/
├── index.html
├── src/input.css
├── dist/output.css
├── imagens/ (WebP otimizadas)
├── tailwind.config.js
├── package.json
└── .github/workflows/deploy.yml
```

## Paleta de Cores

| Uso | Hex |
|-----|-----|
| Primária (rose gold) | #C4956A |
| Secundária (blush) | #F5E6E0 |
| Background | #FEFAF8 |
| Texto principal | #3D3530 |
| Texto secundário | #8B7E74 |
| Accent/CTA | #D4A088 |

## Tipografia

- **Headings**: Serif elegante (Playfair Display ou Cormorant Garamond)
- **Body**: Sans-serif limpa (Inter ou Lato)

## Seções da Página (ordem de scroll)

### 1. Header/Nav
- Logo à esquerda
- Links âncora: Sobre, Serviços, Localização
- Botão WhatsApp no nav
- Menu hambúrguer no mobile

### 2. Hero
- Foto de perfil da Dra. Tayrine
- H1: "Cuidados especializados para sua pele, cabelos e unhas"
- Subtítulo: "Atendimento humanizado e personalizado em Marabá/PA"
- CTA: "Agende sua consulta" (link WhatsApp)

### 3. Sobre
- Foto + bio
- Texto: "Sou a Dra. Tayrine Duarte, médica graduada pela Universidade São Lucas (Porto Velho/RO) com pós-graduação em saúde da pele pelo Instituto Superior de Medicina (Belo Horizonte/MG). Minha missão é oferecer um atendimento acolhedor e individualizado, cuidando da saúde da sua pele, cabelos e unhas com dedicação e conhecimento técnico."
- CRM exibido

### 4. Serviços (4 cards)

**Avaliação de Pele e Mapeamento de Sinais**
> Avaliação completa da saúde da sua pele com dermatoscopia digital. Identificamos e acompanhamos pintas, manchas e lesões para prevenção e diagnóstico precoce.

**Tratamento de Acne e Cicatrizes**
> Protocolos personalizados para tratar acne ativa e suavizar cicatrizes. Devolvemos a saúde e a autoestima da sua pele com tratamentos seguros e eficazes.

**Saúde Capilar**
> Cuidamos do seu couro cabeludo e cabelos. Investigamos causas de queda capilar e problemas do couro cabeludo com abordagem clínica completa.

**Cuidados com as Unhas**
> Diagnóstico e tratamento de alterações nas unhas, como micoses, fragilidade e deformidades. Saúde das unhas também é saúde!

### 5. Carrossel de Fotos
- 5 fotos do consultório e atendimento
- Scroll-snap horizontal
- Setas de navegação
- Indicadores de posição

### 6. Localização
- Google Maps embed (iframe, lazy loaded)
- Endereço completo
- Link para abrir no Google Maps

### 7. CTA Final
- "Cuide da saúde da sua pele, cabelos e unhas"
- "Agende sua consulta e receba um atendimento personalizado"
- Botão: "Falar com a Dra. Tayrine pelo WhatsApp"

### 8. Footer
- Logo
- Contatos (WhatsApp, email, Instagram)
- CRM
- Endereço

### Elemento Flutuante
- Botão WhatsApp fixo, canto inferior direito
- Link wa.me/5594984336074
- Animação de pulso sutil

## SEO

### Keywords primárias
- cuidados com a pele marabá
- tratamento de acne marabá
- médica pele cabelos unhas marabá
- saúde da pele marabá PA
- consulta pele marabá
- tratamento queda de cabelo marabá
- mapeamento de sinais marabá

### Tags
- Title: "Dra. Tayrine Duarte | Cuidados com Pele, Cabelos e Unhas em Marabá/PA"
- Meta description otimizada
- Schema.org JSON-LD (MedicalBusiness + Physician)
- Open Graph tags
- Alt text descritivo em todas as imagens
- Canonical URL

## Performance (Lighthouse 90+)

- Imagens WebP otimizadas com fallback JPG
- Lazy loading no carrossel e Maps
- CSS purgado (~5-10kb)
- font-display: swap
- Preload de hero image e fontes críticas
- HTML minificado

## Responsividade

- Mobile-first
- Breakpoints: sm (640px), md (768px), lg (1024px)
- Carrossel horizontal no mobile, grid no desktop
- Menu hambúrguer no mobile
