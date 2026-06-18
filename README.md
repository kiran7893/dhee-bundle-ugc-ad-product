# dhee-bundle-ugc-ad-product

A themed **product / UGC video ad** pipeline for
[Dhee](https://github.com/dheeai/dhee-desktop). Give it a real product image, a
short description, and a theme (e.g. *Rainy season*, *Festive tea-time*) and it
produces a short, alive, multi-beat video ad with a designed, multilingual
voiceover. Product-agnostic — all specifics come from the inputs.

Bundle: **`bundles/ugc_ad_product_v2`**.

## The keystone rule

A product's printed label/text must **never** be re-synthesised by a diffusion
model (it re-letters fine print). This bundle honours that two ways:

1. **Keep it far / illegible** — the product is model-composited (boogu) into the
   scene *small and set back*, so the label is sub-readable and any drift is
   invisible by design.
2. **Real pixels for what must read** — the brand **logo** end-card is the actual
   logo, overlaid (never regenerated). Brand recognition rides the logo + the
   voiceover, not the tiny on-pack text.

## Pipeline

```
product image → comfy.matte (SAM 3 concept extract → product on white)
             → comfy.boogu  (composite far/blended into the themed scene)
             → comfy.ltx_director (alive motion)            ── ×3 beats
designed voice (omnivoice_design) → emotive voice acting (omnivoice_clone)
             → ffmpeg.concat → ffmpeg.overlay (logo end-card)
```

## Inputs

`product_description`, `product_image`, `product_concept` (what SAM 3 segments),
`theme`, `language` (English / Kannada / Telugu / Hindi / …), `voice_style`
(e.g. *calm and warm* or *high-energy voice acting*), `brand_logo`, `aspect`.

## Voice

Voice is **designed then acted**: tags (incl. `indian accent`) → a designed
voice → each line acted in it with emotion (`temperature`/`guidance_scale`).
`voice_style` drives the script energy.

## Runners

`llm.generate`, [`comfy.matte`](https://github.com/dheeai/dhee-runner-matte),
`comfy.boogu`, `comfy.tts`, `comfy.ltx_director`, `ffmpeg.*`.

> Sample inputs in `inputs/` are GRL Foods (Mangaluru) marketing assets, used as
> the demo product. Replace them with your own product image, description, and logo.
