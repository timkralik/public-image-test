# Next.js Integration Guide

## Using GitHub Pages Images in Next.js Applications

This guide shows how to configure and use these GitHub Pages hosted images in Next.js applications with proper optimization.

### 1. Next.js Image Configuration

Add the GitHub Pages domain to your `next.config.js` to allow external images. The explicit `port` and `search` properties ensure maximum compatibility:

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'timkralik.github.io',
        port: '',
        pathname: '/public-image-test/**',
        search: '',
      },
    ],
  },
}

module.exports = nextConfig
```

#### Alternative Configuration (Next.js 15.3.0+)
For Next.js 15.3.0 and later, you can also use the cleaner URL object syntax:

```javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      new URL('https://timkralik.github.io/public-image-test/**')
    ],
  },
}

module.exports = nextConfig
```

### 2. Using Next.js Image Component

#### Basic Usage
```jsx
import Image from 'next/image'

export default function Gallery() {
  return (
    <div>
      <Image
        src="https://timkralik.github.io/public-image-test/images/large-geometric-4k.jpg"
        alt="4K Geometric Test Pattern"
        width={800}
        height={600}
        priority // Use for above-the-fold images
      />
    </div>
  )
}
```

#### Responsive Images
```jsx
import Image from 'next/image'

export default function ResponsiveGallery() {
  return (
    <div className="relative w-full h-96">
      <Image
        src="https://timkralik.github.io/public-image-test/images/large-text-6k.jpg"
        alt="6K Text Compression Test"
        fill
        style={{ objectFit: 'cover' }}
        sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
      />
    </div>
  )
}
```

### 3. Image Data Configuration

Create a centralized image configuration:

```typescript
// lib/images.ts
export interface TestImage {
  id: string
  url: string
  alt: string
  width: number
  height: number
  fileSize: string
  compressionType: 'excellent' | 'good' | 'poor' | 'worst'
  description: string
}

export const testImages: TestImage[] = [
  {
    id: 'geometric-4k',
    url: 'https://timkralik.github.io/public-image-test/images/large-geometric-4k.jpg',
    alt: '4K Geometric Shapes',
    width: 4000,
    height: 3000,
    fileSize: '319KB',
    compressionType: 'excellent',
    description: 'Geometric shapes with solid colors - excellent JPEG compression'
  },
  {
    id: 'text-6k',
    url: 'https://timkralik.github.io/public-image-test/images/large-text-6k.jpg',
    alt: '6K Text Image',
    width: 6000,
    height: 4000,
    fileSize: '487KB',
    compressionType: 'excellent',
    description: 'High resolution text - excellent compression due to solid areas'
  },
  {
    id: 'gradient-uhd',
    url: 'https://timkralik.github.io/public-image-test/images/large-gradient-uhd.jpg',
    alt: 'UHD Gradient Swirl',
    width: 3840,
    height: 2160,
    fileSize: '873KB',
    compressionType: 'good',
    description: 'Smooth gradient with swirl effect - good compression'
  },
  {
    id: 'fractal-5k',
    url: 'https://timkralik.github.io/public-image-test/images/large-fractal-5k.jpg',
    alt: '5K Fractal Pattern',
    width: 5000,
    height: 4000,
    fileSize: '29MB',
    compressionType: 'poor',
    description: 'Complex fractal pattern - poor compression due to high detail'
  },
  {
    id: 'noise-4k',
    url: 'https://timkralik.github.io/public-image-test/images/large-noise-4k-square.jpg',
    alt: '4K Random Noise',
    width: 4096,
    height: 4096,
    fileSize: '47MB',
    compressionType: 'worst',
    description: 'Random noise pattern - worst compression case'
  }
]
```

### 4. Dynamic Gallery Component

```tsx
// components/ImageGallery.tsx
import Image from 'next/image'
import { testImages, TestImage } from '@/lib/images'

interface ImageGalleryProps {
  filterByCompression?: TestImage['compressionType']
}

export default function ImageGallery({ filterByCompression }: ImageGalleryProps) {
  const filteredImages = filterByCompression 
    ? testImages.filter(img => img.compressionType === filterByCompression)
    : testImages

  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {filteredImages.map((image) => (
        <div key={image.id} className="border rounded-lg overflow-hidden shadow-lg">
          <div className="relative aspect-video">
            <Image
              src={image.url}
              alt={image.alt}
              fill
              style={{ objectFit: 'cover' }}
              sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
              className="transition-transform hover:scale-105"
            />
          </div>
          <div className="p-4">
            <h3 className="font-semibold text-lg">{image.alt}</h3>
            <p className="text-sm text-gray-600 mt-2">{image.description}</p>
            <div className="flex justify-between items-center mt-3 text-xs text-gray-500">
              <span>{image.width}×{image.height}</span>
              <span className={`px-2 py-1 rounded ${
                image.compressionType === 'excellent' ? 'bg-green-100 text-green-800' :
                image.compressionType === 'good' ? 'bg-blue-100 text-blue-800' :
                image.compressionType === 'poor' ? 'bg-yellow-100 text-yellow-800' :
                'bg-red-100 text-red-800'
              }`}>
                {image.fileSize}
              </span>
            </div>
          </div>
        </div>
      ))}
    </div>
  )
}
```

### 5. Performance Considerations

#### Lazy Loading
```jsx
// For images below the fold
<Image
  src="https://timkralik.github.io/public-image-test/images/large-fractal-5k.jpg"
  alt="Large Fractal"
  width={1000}
  height={800}
  loading="lazy" // Default behavior
/>
```

#### Priority Loading
```jsx
// For above-the-fold hero images
<Image
  src="https://timkralik.github.io/public-image-test/images/large-geometric-4k.jpg"
  alt="Hero Image"
  width={1200}
  height={900}
  priority={true}
/>
```

### 6. Error Handling

```tsx
'use client'
import Image from 'next/image'
import { useState } from 'react'

export default function SafeImage({ src, alt, ...props }) {
  const [error, setError] = useState(false)

  if (error) {
    return (
      <div className="flex items-center justify-center bg-gray-200 text-gray-500">
        Failed to load image
      </div>
    )
  }

  return (
    <Image
      src={src}
      alt={alt}
      onError={() => setError(true)}
      onLoad={() => console.log('Image loaded successfully')} // Use onLoad instead of deprecated onLoadingComplete
      {...props}
    />
  )
}
```

### 7. TypeScript Support

Add types to your `next-env.d.ts` or create a `types/images.d.ts`:

```typescript
// types/images.d.ts
declare module '@/lib/images' {
  export interface TestImage {
    id: string
    url: string
    alt: string
    width: number
    height: number
    fileSize: string
    compressionType: 'excellent' | 'good' | 'poor' | 'worst'
    description: string
  }
  
  export const testImages: TestImage[]
}
```

### 8. Usage Examples

#### Basic Page Implementation
```tsx
// app/gallery/page.tsx
import ImageGallery from '@/components/ImageGallery'

export default function GalleryPage() {
  return (
    <div className="container mx-auto px-4 py-8">
      <h1 className="text-3xl font-bold mb-8">Image Compression Tests</h1>
      
      <section className="mb-12">
        <h2 className="text-2xl font-semibold mb-4">Excellent Compression</h2>
        <ImageGallery filterByCompression="excellent" />
      </section>
      
      <section className="mb-12">
        <h2 className="text-2xl font-semibold mb-4">Poor Compression</h2>
        <ImageGallery filterByCompression="poor" />
      </section>
    </div>
  )
}
```

### 9. Environment Variables (Optional)

For easier configuration management:

```bash
# .env.local
NEXT_PUBLIC_IMAGE_BASE_URL=https://timkralik.github.io/public-image-test/images
```

```typescript
// lib/images.ts
const baseUrl = process.env.NEXT_PUBLIC_IMAGE_BASE_URL || 'https://timkralik.github.io/public-image-test/images'

export const getImageUrl = (filename: string) => `${baseUrl}/${filename}`
```

This setup provides a complete integration of the GitHub Pages hosted images with Next.js, including proper optimization, error handling, and TypeScript support.