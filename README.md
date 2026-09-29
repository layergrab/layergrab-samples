# LayerGrab samples: real image-to-layers results

Six images split into layers by [LayerGrab](https://layergrab.com), saved exactly as the model returned them: the original, the filled-in background, every layer as a transparent image, and a JSON manifest with each layer's name, stacking order and position. They are among the samples on the LayerGrab home page, published here so anyone comparing layer-decomposition models, or building something that reads layered results, has real data to look at.

| Sample | Image | Layers | Split in |
|---|---|---|---|
| `coffee-poster` | Coffee shop poster, 1760×2368 | 10 | 100 s |
| `headphones-banner` | Product banner, 2720×1536 | 7 | 66 s |
| `skincare-social-post` | Instagram post, 2048×2048 | 7 | 80 s |
| `ai-desk-illustration` | Isometric desk illustration, 2352×1760 | 9 | 100 s |
| `growth-report-slide` | Presentation slide, 2560×1600 | 11 | 97 s |
| `sportswear-flat-lay` | Flat-lay photo, 2416×1616 | 15 | 148 s |

Layer counts exclude the filled background. `index.json` lists the same table with the source credit of every sample.

## Layout of a sample

```
samples/<id>/
  sample.json      title, size, seconds, optional prompt, credit, and the layer list
  original.webp    the flat input image
  base.webp        the background with every layer removed and the hidden areas filled in
  layer-01.webp …  one transparent image per layer, numbered by z-index (1 = bottom)
```

`sample.json`:

```json
{
  "id": "skincare-social-post",
  "title": "Skincare Instagram post",
  "width": 2048, "height": 2048, "seconds": 80,
  "layers": [
    { "file": "layer-07.webp", "name": "Skincare Serum Bottle", "description": "…", "zIndex": 7, "box": [x1, y1, x2, y2] }
  ]
}
```

`box` is in the base image's pixel coordinates; a layer file is the size of its box, not of the whole image. To rebuild the original, draw `base.webp`, then each layer at its box in ascending `zIndex`.

## What to look at

- **Completed elements.** In `coffee-poster`, the cup behind the pumpkin comes back as a whole cup; in `ai-desk-illustration` the desk under everything is its own whole layer.
- **Filled backgrounds.** Every `base.webp` has the elements painted out, not cut out.
- **Targeted splits.** A sample split with a one-sentence instruction ("only the person on the right") will be added; the tool takes such an instruction on any image.
- **Names.** Layer names come from the model, in English, as returned.

## Licence and credits

Split results and the images made for this demo: [CC BY 4.0](LICENSE), attribution "LayerGrab, https://layergrab.com". `sportswear-flat-lay/original.webp` is a photo by [mr lee on Unsplash](https://unsplash.com/photos/888HU1GauzY) under the Unsplash License; `coffee-poster` and `headphones-banner` were generated with gpt-image-2, `ai-desk-illustration` and `skincare-social-post` with Seedream 5.0; `growth-report-slide` is a mock slide with illustrative figures.

Made with [LayerGrab](https://layergrab.com), which splits images like these in the browser or inside Photoshop, Figma, Sketch and Chrome.
