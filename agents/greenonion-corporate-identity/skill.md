---
name: greenonion-corporate-identity
description: Ensures consistent brand identity across all GreenOnion outputs including presentations, documents, marketing materials, and digital content with color palette, typography, templates, and tone of voice guidelines.
---

# GreenOnion Corporate Identity Skill

## PURPOSE
This skill ensures all Claude outputs for GreenOnion maintain consistent brand identity across presentations, documents, marketing materials, and digital content.

## WHEN TO USE THIS SKILL
Activate automatically when:
- Creating presentations (PowerPoint/PPTX)
- Writing documents (Word/DOCX, PDF)
- Designing marketing materials
- Developing web content or landing pages
- Creating email templates
- Generating social media content
- Any output representing GreenOnion brand

## BRAND IDENTITY SPECIFICATIONS

### Logo Variants

Two official logos are in use. Both are located in `assets/`.

1. **Logo-2.svg** (Primaerlogo, SVG-Vektordatei): Horizontales Logo mit Zwiebel-Icon in Turquoise und "GreenOnion" Wortmarke in Dark Green. Einsatz auf weissen und hellen Hintergruenden. Bevorzugtes Format fuer digitale und Print-Anwendungen, da verlustfrei skalierbar.

2. **Logo-dark-transparent.png** (Sekundaerlogo, bereits freigestellt): Weisse Wortmarke + turquoise Icon auf transparentem Hintergrund. Einsatz auf dunklen oder farbigen Hintergruenden.

**Logo Usage Rules:**
- Primaerlogo (SVG): Auf weissem oder hellem Hintergrund, alle offiziellen Dokumente, Web, Praesentationen
- Sekundaerlogo (PNG, freigestellt): Auf dunklen oder farbigen Hintergruenden
- Immer Originalproportionen beibehalten, kein Strecken oder Verzerren
- Mindest-Freiraum um das Logo: Logohoehe x 0.5 auf allen Seiten
- Keine Schatten, Outline oder Effekte auf das Logo legen
- **Web: SVG-Format bevorzugen** -- PNG-Logos muessen passend zur Rendergroesse dimensioniert sein (kein 6000px-PNG fuer ein 200px-Element)

### Color Palette

**Primary Colors:**
- **Turquoise (Brand Primary):** #4DC3BD
  - RGB: 77, 195, 189
  - CMYK: 62, 0, 25, 0
  - Usage: Primary brand color, CTAs, highlights, icon elements
  - Hover: #3DAB9F

- **Dark Green (Brand Secondary):** #003530
  - RGB: 0, 53, 48
  - CMYK: 100, 58, 76, 68
  - Usage: Headlines, body text, professional contexts, primary nav CTAs
  - Hover: #002420

**Supporting Colors:**
- **White:** #FFFFFF (backgrounds, text on dark)
- **Light Grey:** #F5F5F5 (subtle section backgrounds)
- **Mid Grey:** #B8C5C2 (tertiary text, disabled states, decorative borders)
- **Dark Grey:** #333333 (secondary body text)
- **Near Black:** #0A1F1C (dark section backgrounds, alternative to Dark Green on very dark layouts)

**Data / UI State Colors (nur fuer digitale Interfaces, nicht fuer Print/Dokumente):**
- **Amber:** #F5A623 (warning states, attention indicators)
- **Data Green:** #2DB88F (live/active status indicators -- gedaempftes Teal, KEIN Neon)
- **Data Red:** #E0492A (error states, critical indicators)

**Color Psychology & Application:**
- Turquoise conveys: innovation, sustainability, growth, freshness
- Dark Green conveys: stability, trust, environmental responsibility, professionalism
- Use turquoise for energetic, forward-looking content
- Use dark green for serious, professional, trust-building contexts

### CTA Color Hierarchy (Web/Digital)

| Typ | Hintergrund | Text | Hover | Einsatz |
|-----|-------------|------|-------|---------|
| **Primary Nav CTA** | Dark Green #003530 | White | #002420 | Conversion: "Erstgespraech vereinbaren" |
| **Inline / Secondary CTA** | Turquoise #4DC3BD | White | #3DAB9F | Action: "Mehr erfahren", "Details" |
### Typography

**Primary Font Family:** Gilroy. Alle drei Schnitte sind als TTF-Dateien in `assets/` vorhanden:
- `assets/Gilroy-Black.ttf`
- `assets/Gilroy-Bold.ttf`
- `assets/Gilroy-Regular.ttf`

**Font Hierarchy:**
- **Headlines/H1:** Gilroy Black (900) oder ExtraBold (800), 32-48pt, Dark Green (#003530)
- **Subheadlines/H2:** Gilroy Bold, 24-32pt, Dark Green oder Turquoise
- **Body Text:** Gilroy Regular, 14-18pt, Dark Green (#003530) oder Dark Grey (#333333)
- **Captions/Small Text:** Gilroy Regular, 12-14pt, Dark Grey
- **Starke Betonung/Highlights:** Gilroy Black, fuer kurze Textelemente, Kennzahlen, CTAs

**Email/Office Fallback (Tahoma):**
- For email signatures and MS Office documents where Gilroy is unavailable
- Maintain same size and color specifications

**Web/Digital Fallbacks:**
- If Gilroy unavailable: Use "Inter" (gute Systemunterstuetzung, aehnliche Geometrie)
- System fallback: system-ui, -apple-system, Arial, Helvetica, sans-serif

**Typography Rules:**
- Line height: 1.5x font size for body text
- **Letter spacing Headlines: -0.01em** (leicht enger -- kein aufgesperrter Satz)
- **Letter spacing Display/Hero: -0.02em** (grosse Skalierung braucht engeren Satz)
- **Letter spacing Eyebrow/Labels: +0.08em** (Kleinbuchstaben-Labels aufgesperrt)
- Gilroy Black nur fuer kurze Textelemente (Headlines, Labels, Kennzahlen), nie fuer Fliestext
- Never use more than 2 font weights in a single document
- Avoid all-caps except for short labels/buttons

### Visual Style Guidelines

**Imagery Principles:**
- Clean, professional photography with natural lighting
- Focus on real business environments and people
- Avoid overly staged or stock-photo aesthetic
- Use turquoise color overlays (20-30% opacity) when appropriate
- Prefer images showing: technology, teamwork, growth, sustainability in action

**Graphic Elements:**
- Favor clean geometric shapes (circles, rings echoing onion/logo)
- Use rounded corners for cards and containers (see Radius Scale below)
- Minimal shadows: use Shadow Scale below
- Icons: Outline style, 2px stroke, turquoise or dark green
- Charts/Graphs: Use brand colors, avoid excessive decoration

**Radius Scale:**
- xs: 2px  (Tags, Badges, kleine Chips)
- sm: 4px  (Buttons, kompakte Elemente)
- md: 8px  (Karten, Inputs, Standard-Container)
- lg: 12px (Grosse Karten, Modals)
- xl: 20px (Hero-Elemente, prominente Containers)

**Shadow Scale (dark-green-basiert, subtil):**
- Level 1: `0 1px 2px rgba(0,53,48,0.06)` (minimale Tiefe)
- Level 2: `0 2px 8px rgba(0,53,48,0.08), 0 1px 2px rgba(0,53,48,0.04)` (Standard-Karte)
- Level 3: `0 8px 24px rgba(0,53,48,0.10), 0 2px 4px rgba(0,53,48,0.04)` (Dropdowns, Modals)

**Layout Principles:**
- Generous white space (minimum 40px margins)
- Grid-based layouts (12-column or 8-column systems)
- Asymmetric balance preferred over strict symmetry
- Clear visual hierarchy: F-pattern or Z-pattern for reading flow

### Tone of Voice

**Brand Personality:**
- Professional yet approachable
- Solution-focused, not problem-obsessed
- Data-driven but human-centered
- Forward-thinking without buzzword-heavy language

**Language Guidelines:**
- **DO:** Use active voice, concrete examples, quantified results
- **DO:** Address SME decision-makers (CFOs, Managing Directors) directly
- **DO:** Frame sustainability as ROI/profitability driver
- **DON'T:** Use jargon without explanation
- **DON'T:** Make unsubstantiated "green" claims
- **DON'T:** Oversimplify complex business challenges

**Key Messaging Themes:**
1. Sustainability transformation drives measurable business value
2. Technology-enabled, not just consultancy
3. Built for SMEs (Mittelstand), not enterprise bureaucracy
4. Scalable, efficient, AI-supported processes

## DOCUMENT-SPECIFIC RULES & TEMPLATES

### PowerPoint/Presentations

**Template Reference:** templates/IRIS_Praesentation_0_3.pptx

**Slide Specifications:**
- Presentation size: 16:9 (standard widescreen)
- Logo placement: Typically bottom-right corner
- Title slides: Bold headline, centered or left-aligned
- Content slides: White background, dark green (#003530) text
- Use no more than 3 bullet points per slide
- Footer: Slide number right-aligned, small (10pt)
- Transitions: None or subtle fade (avoid animations)

**Recommended Slide Layouts:**
1. Title Slide: Bold headline + subtitle + visual element
2. Content Slide: Header + 2-3 bullet points + supporting image
3. Data Slide: Large statistic + context + source citation
4. Closing Slide: CTA + contact information

### Word Documents/Reports

**Template Reference:** templates/Briefvorlage_GO.docx

**Document Structure:**
- **No Header** (keep clean and minimal)
- **Body:**
  - First line: Location and date (right-aligned)
  - Recipient address block (custom style: "Empfaengeradresse")
  - Salutation (custom style: "Salutation")
  - Body text: Normal style, Gilroy/Calibri Regular 11pt
  - Line spacing: 1.15
- **Footer:**
  - Company: "GreenOnion FlexCo, Salurnergasse 4, 1010 Wien | greenonion.at"
  - Legal: "IBAN: AT57 3225 0000 0077 5650 | FN: 480558d | UID: ATU72745746"
  - Footer font: 8pt, centered, three-column layout

**Custom Word Styles:**
- **Empfaengeradresse:** For recipient address block
- **Salutation:** For greeting line
- **Normal:** Standard body text

### Email Signatures

**Template Reference:** templates/email-signatur.html

**Signature Structure:**
- **Name:** Tahoma Bold, #003530 (Dark Green)
- **Position & Company:** Tahoma 9pt, #003530
- **Contact Details:** Tahoma 9pt, #003530
- **Logo:** Right-aligned, 161x161px
- **CTA:** "Online Terminvereinbarung" centered below contact details

**Plain Text Fallback:**
```
[Name]
[Position] | GreenOnion FlexCo

Tel.: [Phone]
E-Mail: [Email]

greenonion.at | LinkedIn
Online Terminvereinbarung: cal.com/greenonion
```

### Web/Digital
- Hero sections: Full-width turquoise background, white text
- Primary Nav CTAs (Conversion): Dark Green (#003530), white text, border-radius: var(--radius-sm)
- Inline/Secondary CTAs (Action): Turquoise (#4DC3BD), white text, border-radius: var(--radius-sm)
- Hover: --brand-dark-green-hover (#002420) resp. --brand-turquoise-hover (#3DAB9F)
- Forms: Dark green labels, turquoise focus state on inputs
- Cards: White with var(--shadow-2), optional turquoise accent border (top or left)

## TECHNICAL IMPLEMENTATION

### For PowerPoint (PPTX)
```python
TURQUOISE = RGBColor(77, 195, 189)
DARK_GREEN = RGBColor(0, 53, 48)
WHITE = RGBColor(255, 255, 255)
SLIDE_WIDTH = Inches(13.333)
SLIDE_HEIGHT = Inches(7.5)
TITLE_FONT_SIZE = Pt(44)
BODY_FONT_SIZE = Pt(18)
FONT_BLACK = "Gilroy-Black"
FONT_BOLD = "Gilroy-Bold"
FONT_REGULAR = "Gilroy-Regular"
```

### For Word (DOCX)
```python
from docx.shared import RGBColor, Pt
heading1_style.font.name = "Gilroy"
heading1_style.font.size = Pt(32)
heading1_style.font.color.rgb = RGBColor(0, 53, 48)
normal_style.font.name = "Gilroy"
normal_style.font.size = Pt(11)
normal_style.paragraph_format.line_spacing = 1.15
# Footer: GreenOnion FlexCo, Salurnergasse 4, 1010 Wien | greenonion.at
# IBAN: AT57 3225 0000 0077 5650 | FN: 480558d | UID: ATU72745746
```

### For Web/HTML
Standard CSS variables:
```css
:root {
  /* Brand Base Colors */
  --brand-turquoise: #4DC3BD;
  --brand-turquoise-hover: #3DAB9F;
  --brand-dark-green: #003530;
  --brand-dark-green-hover: #002420;
  --brand-near-black: #0A1F1C;
  --brand-white: #FFFFFF;
  --brand-light-grey: #F5F5F5;
  --brand-mid-grey: #B8C5C2;
  --brand-dark-grey: #333333;

  /* Data / UI State (interfaces only) */
  --brand-amber: #F5A623;
  --brand-data-green: #2DB88F;
  --brand-data-red: #E0492A;

  /* Semantic Foreground Tokens */
  --fg-1: var(--brand-dark-green);
  --fg-2: var(--brand-dark-grey);
  --fg-3: var(--brand-mid-grey);
  --fg-on-dark: var(--brand-white);
  --fg-accent: var(--brand-turquoise);

  /* Semantic Background Tokens */
  --bg-1: var(--brand-white);
  --bg-2: var(--brand-light-grey);
  --bg-3: var(--brand-near-black);
  --bg-dark: var(--brand-dark-green);

  /* Border Tokens */
  --border-1: rgba(0, 53, 48, 0.12);
  --border-2: rgba(0, 53, 48, 0.20);

  /* Typography */
  --font-sans: 'Gilroy', 'Inter', system-ui, -apple-system, sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', ui-monospace, 'Courier New', monospace;

  /* Letter Spacing */
  --tracking-display: -0.02em;
  --tracking-headline: -0.01em;
  --tracking-eyebrow: 0.08em;

  /* Radius Scale */
  --radius-xs: 2px;
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;
  --radius-xl: 20px;

  /* Shadow Scale */
  --shadow-1: 0 1px 2px rgba(0,53,48,0.06);
  --shadow-2: 0 2px 8px rgba(0,53,48,0.08), 0 1px 2px rgba(0,53,48,0.04);
  --shadow-3: 0 8px 24px rgba(0,53,48,0.10), 0 2px 4px rgba(0,53,48,0.04);

  /* Line Heights */
  --line-height-body: 1.5;
  --line-height-heading: 1.2;
}

a {
  color: var(--brand-dark-green);
  text-decoration: none;
  transition: opacity 0.2s;
}
a:hover { opacity: 0.8; }
```

## COMPANY CONTACT INFORMATION

- **Legal Entity:** GreenOnion FlexCo
- **Address:** Salurnergasse 4, 1010 Wien, Austria
- **Website:** greenonion.at
- **LinkedIn:** linkedin.com/company/greenonion
- **Phone (Main):** 0670 18 18 074
- **Email (Main):** martin@greenonion.at
- **IBAN:** AT57 3225 0000 0077 5650
- **FN:** 480558d
- **UID:** ATU72745746
- **Calendly:** cal.com/greenonion

Use "GreenOnion FlexCo" as legal entity name. For marketing: "GreenOnion" acceptable. Website without "www": greenonion.at

## QUALITY CHECKLIST

- [ ] Logo present and properly sized (SVG bevorzugen fuer Web/Screen)
- [ ] Brand colors used correctly (#4DC3BD + #003530)
- [ ] Gilroy font applied (Tahoma/Calibri fallback fuer Office)
- [ ] Sufficient white space and clear hierarchy
- [ ] Tone matches brand voice (professional, solution-focused)
- [ ] No stock photo cliches or greenwashing language
- [ ] Contact information accurate and complete
- [ ] Footer includes legal information (official documents)
- [ ] File naming: GreenOnion_[Type]_[YYYYMMDD].ext
- [ ] Keine Neon-Farben (Data Green = #2DB88F, nicht #0f8)

## PROHIBITED ELEMENTS

- Comic Sans, Papyrus, or decorative fonts
- Pure black (#000000) for text -- use dark green #003530
- Neon colors (insb. kein #0f8 / #00ff88 als Data Green)
- Clipart or low-quality graphics
- Excessive gradients or drop shadows
- Generic sustainability stock photos
- Outdated company information or wrong legal entity name
- Underlining for emphasis (use bold instead)
- Overscaled PNG logos (max. Faktor 3 zwischen natuerlicher und Rendergroesse)

## ADDITIONAL NOTES

**Target Audience:** SME executives (50-500 MA), CFOs, Sustainability Managers, Investors. DACH-Raum.

**Asset Files (in `assets/`):**
- Logo SVG: `assets/Logo-2.svg`
- Logo PNG dark: `assets/Logo-dark-transparent.png`
- Gilroy-Black.ttf, Gilroy-Bold.ttf, Gilroy-Regular.ttf

**Template Files (in `templates/`):**
- PowerPoint: `templates/IRIS_Praesentation_0-3.pptx`
- Word: `templates/Briefvorlage_GO.docx`
- Email: `templates/email-signatur.html`

---

**Version:** 2.2
**Last Updated:** May 29, 2026
**Maintained By:** Martin Watzka, GreenOnion FlexCo
**Changelog v2.2 (Audit 29.05.2026):**
- Farben: near-black, mid-grey, amber, data-green (#2DB88F, war Neon), data-red ergaenzt
- Semantische fg/bg Token-Schicht dokumentiert
- Hover-Tokens (turquoise-hover, dark-green-hover) ergaenzt
- Radius-Scale (xs/sm/md/lg/xl) formalisiert
- Shadow-Scale (3 Ebenen) dokumentiert
- Border-Tokens dokumentiert
- Letter-Spacing korrigiert: Headlines -0.01em (war +0.02em), Display -0.02em, Eyebrow +0.08em
- Web-Fallback: Montserrat -> Inter
- font-mono: korrekter Monospace-Stack (war identisch mit font-sans)
- CTA-Hierarchie: Primary Nav = Dark Green, Inline = Turquoise -- explizit dokumentiert
- Logo-Warnung: SVG bevorzugen, PNG-Skalierung begrenzen
