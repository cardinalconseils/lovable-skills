---
name: photo-direction
description: Use when the user wants to create product photography, brand imagery, or commercial photos using AI image generation. Also use when the user mentions 'product photography', 'AI photo', 'brand imagery', 'photo direction', 'gpt-image-1', 'image generation prompt', or 'Peter Belanger style'.
---

# Photo Direction

Expert knowledge for directing and generating commercial product photography using AI — specifically gpt-image-1. Based on Peter Belanger's aesthetic: clean, precise, honest representation of the product.

## Peter Belanger Aesthetic

Peter Belanger shot Apple's product photography for over a decade. His defining qualities:
- **Absolute cleanliness:** No dust, no fingerprints, no extraneous elements
- **Controlled light:** Soft, directional, revealing form without harsh shadows
- **Subject respect:** The product is the hero. Background and props serve; they never compete.
- **Honest representation:** No distortion, no impossible perspectives. The product looks exactly as it would in your hand.

## Prompt Construction Template

```
[SUBJECT]: [exact product description, color, material, dimensions if relevant]
[LIGHTING]: [named lighting configuration from list below]
[BACKGROUND]: [color/texture from palette below]
[ANGLE]: [angle from reference table below]
[SURFACE]: [what the product rests on, if anything]
[MOOD]: [one-word mood: clinical, warm, luxurious, editorial, minimal]
[STYLE]: commercial product photography, sharp focus, high resolution, Peter Belanger style
```

**Example:**
```
SUBJECT: Matte black aluminum water bottle, 750ml, minimalist design
LIGHTING: Studio Clean (large softbox left, fill right, no shadows)
BACKGROUND: Pure white (#FFFFFF)
ANGLE: 3/4 front at eye level
SURFACE: Frosted white acrylic
MOOD: Clinical
STYLE: commercial product photography, sharp focus, high resolution, Peter Belanger style
```

## Lighting Configurations

| Name | Setup | Best for |
|---|---|---|
| **Studio Clean** | Large softbox at 45° left, fill card right, no shadow | Tech, SaaS screenshots, minimal products |
| **Apple Overhead** | Two large panels above, slight front fill, white BG | App icons, packaging, flat lay |
| **Luxury Shadow** | Single key light at 30°, deep shadow on opposite side, black BG | Watches, jewelry, premium goods |
| **Warm Window** | Large diffused window right, natural bounce left, warm tone | Lifestyle, food-adjacent, wellness |
| **Tabletop Flat** | Overhead ring + two side panels, flat even light | Small products, detail shots |
| **Product Macro** | Ringlight close, shallow DOF, high contrast | Texture, detail, component shots |

## Angle Reference

| Angle | Description | Use when |
|---|---|---|
| Front straight | Camera at product eye level, dead center | Symmetrical products, icons |
| 3/4 front | 30–45° from front, eye level | Most product shots (default) |
| Hero low | Camera below product level, shooting up | Large format, aspirational, bold |
| Overhead flat | Directly above (bird's eye) | Flat lays, packaging, books |
| Side profile | Camera perpendicular to product | Shoes, bottles, slim devices |
| Macro detail | Extreme close-up on a specific feature | Texture, materials, craftsmanship |

## Background Palette

| Name | Hex | Character |
|---|---|---|
| Pure white | `#FFFFFF` | Clean, clinical, Apple-esque |
| Off-white warm | `#F5F0E8` | Softer, lifestyle, premium |
| Light grey | `#E8E8E8` | Neutral, editorial, tech |
| Slate dark | `#2C2C2C` | Dramatic, luxury, dark mode |
| Deep black | `#0A0A0A` | Maximum contrast, bold |
| Cream | `#F9F4EC` | Warm, artisan, beauty |
| Sage | `#C8D5C0` | Natural, wellness, organic |

## gpt-image-1 API Parameters

```python
response = client.images.generate(
    model="gpt-image-1",
    prompt=prompt,
    n=1,
    quality="high",          # "high" for final output, "medium" for iteration
    size="1024x1024",        # Square for product shots, "1792x1024" for hero banners
    output_format="png"      # PNG for transparency support
)
```

**Size guide:**
| Use | Size |
|---|---|
| Product hero (square) | `1024x1024` |
| Banner / hero wide | `1792x1024` |
| Mobile story / social | `1024x1792` |
| Thumbnail iteration | `512x512` + `quality: medium` |

## 3-Variant Strategy

For every product, generate 3 variants before selecting:
1. **Clean / clinical** — Pure white background, Studio Clean lighting. Maximum flexibility for UI use.
2. **Contextual** — Background that hints at use context (desk surface, hand-held, lifestyle setting)
3. **Dramatic** — Dark background, Luxury Shadow lighting. For hero sections, paid ads.

Select the variant that matches the page or ad context. Don't use a single shot everywhere.

## Quality Checklist

- [ ] No visible dust, fingerprints, or artifacts on product surface
- [ ] Shadow direction is consistent (all shadows come from the same light source direction)
- [ ] Product is in perfect focus at the intended focal point
- [ ] Background is clean with no unexpected gradients or artifacts
- [ ] Product dimensions appear realistic (no distortion)
- [ ] If text is on the product — verify it's legible and correctly spelled
- [ ] Color accuracy matches brand reference (check hex values against brand guide)

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "Any prompt works if we describe the product" | Lighting and angle instructions are what separate clinical product shots from generic AI images. |
| "We'll generate one image and use it everywhere" | Different contexts need different moods. The 3-variant strategy takes 3x the generation time for 10x the flexibility. |
| "We don't need to specify background color" | Unspecified backgrounds are unpredictable. Always specify. |
| "The image looks close enough" | Product photography sets brand perception. Close enough is not good enough for hero shots. |
