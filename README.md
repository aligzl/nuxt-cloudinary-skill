# Nuxt Cloudinary Skill

A lightweight skills.sh integration for Nuxt 3,4 Cloudinary usage guidance.

## What this repo contains
- `nuxt-cloudinary/SKILL.md`: core skill doc for `skills.sh`, with full onboarding and examples.

## GitHub description (repo summary)
Nuxt Cloudinary helper skill for skills.sh: image/video optimization, upload widget, transform syntax and best practices.

## Quick start
1. Install module in Nuxt 3:
   ```bash
   npm install @nuxtjs/cloudinary
   ```
2. Configure in `nuxt.config.ts`:
   ```ts
   export default defineNuxtConfig({
     modules: ['@nuxtjs/cloudinary'],
     cloudinary: {
       cloudName: process.env.CLOUDINARY_CLOUD_NAME,
       uploadPreset: 'my-preset'
     }
   })
   ```
3. Use components in templates:
   - `<CldImage />`
   - `<CldVideoPlayer />`
   - `<CldUploadWidget />`

## What this skill covers
- image & video components 
- URL composables (`useCldImageUrl`, `useCldVideoUrl`)
- signed/unsigned uploads
- transformations, effects, and layout shift prevention

## Policy
- Use environment variables for secrets.
- Prefer Nuxt auto-imported components instead of manual imports.

## Help
See `nuxt-cloudinary/SKILL.md` for full documentation and examples.
