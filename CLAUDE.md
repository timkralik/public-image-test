# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a GitHub Pages static image hosting repository designed for compression testing and demonstrations. It serves high-quality test images with different compression characteristics via `https://timkralik.github.io/public-image-test/images/[filename]`.

## Key Commands

### Initial Setup
```bash
# Run the setup script to create a new GitHub Pages image host
./setup-github-pages-image-host.sh
```

### Adding New Images
```bash
# Navigate to the working directory
cd gh-pages-imgs

# Add images to the images/ directory
cp new-image.jpg images/

# Generate large test images using ImageMagick (if available)
convert -size 4000x3000 xc:none -fill "rgb(255,100,50)" -draw "circle 2000,1500 2000,800" -quality 100 images/new-geometric.jpg

# Update documentation and commit
git add .
git commit -m "Add new test images"
git push
```

### Documentation Updates
```bash
# Update the image catalog with new compression analysis
edit md/image-catalog.md

# Update sitemap with new resources
edit sitemap.xml

# Update main README with new image links
edit README.md
```

## Architecture

This repository follows a **pure static hosting pattern** with no build system or server-side processing:

```
Static Site Structure:
├── images/           # Test images with different compression characteristics
├── md/               # Markdown documentation (served as static files)
├── sitemap.xml       # SEO sitemap for GitHub Pages
├── README.md         # Main documentation (GitHub Pages index)
└── setup-*.sh        # Automation scripts (not deployed)
```

### Image Categorization System

Images are organized by **compression effectiveness**:
- **Excellent** (sub-500KB): Geometric shapes, text, solid colors
- **Good** (500KB-1MB): Gradients, simple patterns
- **Poor** (1MB-30MB): Complex fractals, detailed patterns  
- **Worst** (30MB+): Random noise, high-frequency content

### GitHub Pages Integration

The repository is designed to work seamlessly with GitHub Pages automatic deployment:
- All files in `gh-pages-imgs/` are served statically
- Images are accessible via direct HTTPS URLs
- No build process - files are served as-is
- SEO optimized with proper sitemap.xml

### External Integration Architecture

The repository is specifically designed for consumption by external applications:

**Next.js Integration Pattern:**
```javascript
// Required remotePatterns configuration
images: {
  remotePatterns: [
    {
      protocol: 'https',
      hostname: 'timkralik.github.io', 
      pathname: '/public-image-test/**',
    },
  ],
}
```

**TypeScript Integration:**
- Complete type definitions provided in `md/nextjs-integration.md`
- Image metadata includes dimensions, file sizes, and compression types
- Structured data format for programmatic consumption

## Development Workflow

1. **Image Generation**: Use ImageMagick to create test images with specific compression characteristics
2. **Documentation**: Update `md/image-catalog.md` with compression analysis
3. **SEO**: Add new resources to `sitemap.xml` with appropriate priorities
4. **Integration**: Update `md/nextjs-integration.md` with new examples if needed
5. **Deployment**: Commit and push - GitHub Pages auto-deploys changes

## Key Files

- **`setup-github-pages-image-host.sh`**: Automated repository creation and GitHub Pages setup
- **`md/image-catalog.md`**: Compression analysis and performance comparisons
- **`md/nextjs-integration.md`**: Complete Next.js integration guide with TypeScript support
- **`sitemap.xml`**: SEO-optimized resource listing for search engines

## Image Testing Strategy

The repository contains strategically chosen images to demonstrate JPEG compression behavior:
- **Geometric shapes**: Test solid color compression efficiency
- **Text images**: Test high-contrast compression
- **Gradients**: Test smooth transition compression
- **Fractals**: Test high-detail compression challenges
- **Random noise**: Test worst-case compression scenarios

This allows developers to test image optimization strategies across the full spectrum of compression effectiveness.