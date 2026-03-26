---
name: nuxt-cloudinary
description: How to use Nuxt Cloudinary for optimized images, video playing, and uploading in Nuxt 3. Make sure to use this skill whenever the user mentions images, video players, file uploads, image transformations (like blur, crop, generative fill, background removal), Cloudinary, or media optimization in a Nuxt context, even if they don't explicitly ask for 'Cloudinary'.
tags: [nuxt, cloudinary, media, images, video, upload, optimisation]
---

# Nuxt Cloudinary Skill

This skill provides guidelines and patterns for effectively implementing the `@nuxtjs/cloudinary` module for robust media management in Nuxt 3 applications.

## 1. Nuxt Configuration (`nuxt.config.ts`)
To configure the module, use the `cloudinary` key:
```ts
export default defineNuxtConfig({
  modules: ["@nuxtjs/cloudinary"],
  cloudinary: {
    cloudName: process.env.CLOUDINARY_CLOUD_NAME,
    uploadPreset: 'my-preset', // optionally required globally for unsigned uploads
  }
})
```
*Note: Always use `process.env` or Nuxt runtime config for secrets if needed, though `cloudName` automatically defaults to the `CLOUDINARY_CLOUD_NAME` environment variable.*

## 2. Using `CldImage` (Images & Transformations)
`CldImage` is the optimized image representation. It supports standard image attributes plus Cloudinary's AI transformations.
- **Required Props**: `src` (Cloudinary public ID), `width`, `height`, `alt`.
- **Responsive Sizing**: Use `sizes` prop.

**Example:**
```vue
<template>
  <CldImage
    src="products/my-shoe-image"
    width="800"
    height="600"
    alt="Optimized product image"
    sizes="(max-width: 600px) 100vw, 50vw"
    removeBackground="true"
    crop="fill"
    gravity="auto"
  />
</template>
```

## 3. Using `CldVideoPlayer`
Creates an optimized, customizable video player.
```vue
<template>
  <CldVideoPlayer
    src="videos/landing-bg"
    width="1920"
    height="1080"
    :colors="{ base: '#000', text: '#fff', accent: '#f00' }"
    :controls="false"
    autoplay="true"
    loop="true"
  />
</template>
```

## 4. File Uploads (`CldUploadWidget`)
Provides a pre-built Cloudinary widget.
**Unsigned (Client-side):**
Requires `uploadPreset` (configured globally or passed via prop).
```vue
<template>
  <CldUploadWidget v-slot="{ open }" uploadPreset="my-unsigned-preset" @upload="onUpload">
    <button type="button" @click="open">Upload Image</button>
  </CldUploadWidget>
</template>
<script setup lang="ts">
const onUpload = (result: any) => {
  console.log("Uploaded Public ID:", result.info.public_id);
};
</script>
```

**Signed:**
Pass an API endpoint that generates the signature to `signatureEndpoint="/api/sign-cloudinary-params"`.
```vue
<template>
  <CldUploadWidget v-slot="{ open }" signatureEndpoint="/api/sign-cloudinary-params">
    <button type="button" @click="open">Secure Upload</button>
  </CldUploadWidget>
</template>
```

## 5. Composables (`useCldImageUrl` / `useCldVideoUrl`)
Use these when you need the raw URL string (e.g., for `og:image`, inline background styles, or custom components).
```vue
<template>
  <div :style="{ backgroundImage: `url(${url})` }">
    Content inside optimized background
  </div>
</template>

<script setup lang="ts">
const { url } = useCldImageUrl({
  options: {
    src: "my-image",
    width: 1200,
    height: 630,
    crop: "fill",
    blur: "200"
  }
});
</script>
```

## 6. Comprehensive Options & Effects Reference
You can pass these options as props to `<CldImage>` or inside the `options` block of `useCldImageUrl`.

### General Target Options
| Option Name          | Type                 | Example / Default |
|----------------------|----------------------|-------------------|
| `crop`               | string               | `"thumb"`, `"fill"`, `"limit"` (default) |
| `gravity`            | string               | `"auto"`, `"faces"`, `"north"` |
| `format`             | string               | `"webp"`, `"auto"` (default) |
| `quality`            | string               | `"90"`, `"auto"` (default) |
| `removeBackground`   | boolean / string     | `true`, `false` |
| `namedTransformations`| string / array      | `['my-transformation']` |
| `rawTransformations` | array                | `['e_blur:2000']` |
| `overlays`           | array of objects     | `[{ text: { text: "Hello", ... }, position: { gravity: "north" } }]` |
| `underlays`          | array of objects     | `[{ publicId: "bg-image" }]` |
| `zoompan`            | boolean / string     | `true`, `"loop"` |

### Visual Effects
These can be passed directly as props or options.

| Effect Name     | Example Values                               |
|-----------------|----------------------------------------------|
| `art`           | `"al_dente"`, `"audrey"`, `"zorro"`          |
| `blur`          | `true`, `"800"` (strength)                   |
| `blurFaces`     | `true`, `"800"`                              |
| `background`    | `"blue"`, `"rgb:2d0eff99"`                   |
| `border`        | `"5px_solid_purple"`                         |
| `color`         | `"blue"`, `"rgb:ff0000"`                     |
| `colorize`      | `"35,co_darkviolet"`                         |
| `contrast`      | `true`, `"100"`, `"level_-70"`               |
| `grayscale`     | `true`                                       |
| `opacity`       | `40`, `"40"`                                 |
| `outline`       | `true`, `"40"`, `"outer:15:200"`             |
| `pixelate`      | `true`, `"20"`                               |
| `redeye`        | `true`                                       |
| `replaceColor`  | `"saddlebrown"`, `"silver:55:89b8ed"`        |
| `saturation`    | `true`, `"70"`                               |
| `shadow`        | `true`, `"50,x_-15,y_15"`                    |
| `sharpen`       | `true`, `"100"`                              |
| `tint`          | `true`, `"100:red:blue:yellow"`              |

## Core Principles
1. **No Explicit Imports**: Nuxt auto-imports components (`CldImage`, `CldVideoPlayer`, `CldUploadWidget`, `CldUploadButton`) and composables. Do not manually `import { CldImage } from ...`.
2. **Prevent Layout Shift**: ALWAYS provide both `width` and `height` props for `CldImage` to reserve the required space on the page.
3. **Use Options API correctly**: In composables, always pass options inside an object `{ options: { ... } }`.
