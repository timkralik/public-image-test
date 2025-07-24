# Image Compression Test Catalog

## Test Images Generated

All images are hosted at: `https://timkralik.github.io/public-image-test/images/`

### Compression Test Results

| Image | Resolution | File Size | Compression Type | URL |
|-------|-----------|-----------|------------------|-----|
| **Geometric Shapes** | 4000×3000 | 319KB | Excellent (solid colors) | [large-geometric-4k.jpg](https://timkralik.github.io/public-image-test/images/large-geometric-4k.jpg) |
| **Text Image** | 6000×4000 | 487KB | Excellent (text/solid areas) | [large-text-6k.jpg](https://timkralik.github.io/public-image-test/images/large-text-6k.jpg) |
| **Gradient Swirl** | 3840×2160 | 873KB | Good (smooth gradients) | [large-gradient-uhd.jpg](https://timkralik.github.io/public-image-test/images/large-gradient-uhd.jpg) |
| **Fractal Plasma** | 5000×4000 | 29MB | Poor (high detail/complexity) | [large-fractal-5k.jpg](https://timkralik.github.io/public-image-test/images/large-fractal-5k.jpg) |
| **Random Noise** | 4096×4096 | 47MB | Worst (random data) | [large-noise-4k-square.jpg](https://timkralik.github.io/public-image-test/images/large-noise-4k-square.jpg) |
| **Sample Landscape** | Varies | 567B | Variable | [sample.jpg](https://timkralik.github.io/public-image-test/images/sample.jpg) |

### Compression Analysis

**Best Compression (Lowest file size to resolution ratio):**
- Geometric shapes and text images compress extremely well due to large areas of solid colors
- JPEG excels at compressing images with smooth color transitions

**Worst Compression (Highest file size):**
- Random noise and fractal patterns resist compression
- High-frequency detail and randomness prevent effective JPEG compression

### Usage Examples

```html
<!-- Direct image embedding -->
<img src="https://timkralik.github.io/public-image-test/images/large-geometric-4k.jpg" alt="4K Geometric Test">

<!-- Responsive image -->
<img src="https://timkralik.github.io/public-image-test/images/large-text-6k.jpg" 
     style="max-width: 100%; height: auto;" alt="6K Text Test">
```

```markdown
![Fractal Test](https://timkralik.github.io/public-image-test/images/large-fractal-5k.jpg)
```