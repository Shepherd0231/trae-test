---
name: "image-prompts"
description: "Generates AI image generation prompts for website visuals and manages Cloudflare R2 image storage workflow. Invoke when user needs hero images, product photos, or setting up responsive image delivery."
---

# Image Prompt Generator & R2 Storage Workflow

## Cloudflare R2 三端图片策略

### 为什么要用 R2？
- ✅ **免费额度**: 每月 10GB 存储 + 1000 万次请求
- ✅ **无出口费用**: 不像 S3 收取高额流量费
- ✅ **全球 CDN**: 自动分发到全球节点
- ✅ **兼容 S3 API**: 易于迁移和使用

### 三端图片规格

为不同设备预先生成三种尺寸的图片：

| 设备类型 | 分辨率 | 用途 | 文件名后缀 |
|---------|--------|------|-----------|
| **Mobile** | 640px 宽 | 手机端 | `-mobile` |
| **Tablet** | 1024px 宽 | 平板端 | `-tablet` |
| **Desktop** | 1920px 宽 | 桌面端 | `-desktop` |

### 图片命名规范
```
hero-solar-mobile.jpg    (640×360)
hero-solar-tablet.jpg    (1024×576)
hero-solar-desktop.jpg   (1920×1080)

expertise-1-mobile.jpg   (400×533)
expertise-1-tablet.jpg   (600×800)
expertise-1-desktop.jpg  (800×1067)
```

## Hero Image Prompt

### Solar Energy Hero
```
Professional solar panel installation on modern residential rooftop, 
aerial view, golden hour lighting, clean energy, sustainable future, 
high-end residential architecture, solar panels with red accent lighting, 
modern design, photorealistic, 8K, cinematic lighting, professional photography, 
aspect ratio 16:9
```

### Alternative Hero
```
Modern solar farm at sunset, rows of photovoltaic panels stretching to horizon,
orange and red sky, clean renewable energy, industrial scale, aerial drone view,
professional photography, 8K, dramatic lighting, aspect ratio 16:9
```

## Service Card Prompts

### Solar Solutions (expertise-1)
```
Modern solar farm with rows of photovoltaic panels, blue sky, clean energy, 
sustainable technology, professional photography, wide angle, 8K, 
industrial design, aspect ratio 3:4
```

### Cable Services (expertise-2)
```
High-speed fiber optic cables, network infrastructure, modern technology, 
clean installation, professional setup, blue lighting, 8K, macro photography, 
aspect ratio 3:4
```

### Phone Services (expertise-3)
```
Modern smartphone with connected network lines, digital communication, 
5G technology, clean background, professional product photography, 8K, 
studio lighting, aspect ratio 3:4
```

### Internet Solutions (expertise-4)
```
Modern router with wifi signal visualization, high-speed internet, 
connected devices, clean home office setup, professional photography, 8K, 
minimal design, aspect ratio 3:4
```

## Team Photo Prompt
```
Professional team meeting in modern office, diverse group of energy experts 
discussing solar solutions, modern office with large windows and natural lighting, 
glass conference table, business casual attire, collaborative atmosphere, 
clean minimalist interior design, professional business photography, 8K, 
corporate style, aspect ratio 4:3
```

## About Page Image
```
Professional team of solar energy consultants in modern office environment,
diverse group wearing business casual, confident poses, large windows with natural light,
clean minimalist interior, professional corporate photography, 8K, 
high-end business atmosphere, aspect ratio 16:9
```

## Specifications

### 原始图片规格（用于生成三端版本）
| Type | Resolution | Ratio | Format | Max Size |
|------|-----------|-------|--------|----------|
| Hero | 1920×1080 | 16:9 | PNG/JPG | < 500KB |
| Cards | 1200×1600 | 3:4 | PNG/JPG | < 800KB |
| Team | 1600×1200 | 4:3 | PNG/JPG | < 600KB |
| About | 1920×1280 | 3:2 | PNG/JPG | < 800KB |

### 三端图片规格
| 设备 | Hero | Cards | Team | About |
|------|------|-------|------|-------|
| Mobile | 640×360 | 400×533 | 400×300 | 640×427 |
| Tablet | 1024×576 | 600×800 | 600×450 | 1024×683 |
| Desktop | 1920×1080 | 800×1067 | 800×600 | 1600×1067 |

### 压缩参数
```bash
# 使用 ImageMagick 生成三端版本
magick input.jpg -resize 640x360 -quality 85 output-mobile.jpg
magick input.jpg -resize 1024x576 -quality 85 output-tablet.jpg
magick input.jpg -resize 1920x1080 -quality 90 output-desktop.jpg
```

### Style Requirements
- **Style**: Modern, professional, high-end
- **Lighting**: Natural or cinematic
- **Colors**: Match brand (#EE0000 red accents)
- **Mood**: Professional, trustworthy, innovative
- **Quality**: Photorealistic, 8K, professional photography

## R2 存储配置

### 1. 创建 R2 Bucket
```bash
# 使用 wrangler CLI
npx wrangler r2 bucket create solarco-images
```

### 2. 上传图片脚本
```javascript
// scripts/upload-images.js
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3';
import { readFileSync } from 'fs';
import { readdirSync } from 'fs';
import { join } from 'path';

const S3 = new S3Client({
  region: 'auto',
  endpoint: `https://${ACCOUNT_ID}.r2.cloudflarestorage.com`,
  credentials: {
    accessKeyId: process.env.R2_ACCESS_KEY_ID,
    secretAccessKey: process.env.R2_SECRET_ACCESS_KEY,
  },
});

async function uploadImages() {
  const imagesDir = './public/images';
  const files = readdirSync(imagesDir);
  
  for (const file of files) {
    const filePath = join(imagesDir, file);
    const fileContent = readFileSync(filePath);
    
    await S3.send(new PutObjectCommand({
      Bucket: 'solarco-images',
      Key: file,
      Body: fileContent,
      ContentType: 'image/jpeg',
    }));
    
    console.log(`Uploaded: ${file}`);
  }
}

uploadImages();
```

### 3. 环境变量 (.env)
```
R2_ACCESS_KEY_ID=your_access_key
R2_SECRET_ACCESS_KEY=your_secret_key
R2_BUCKET_NAME=solarco-images
R2_PUBLIC_URL=https://pub-xxx.r2.dev
```

## 组件中使用 R2 图片

### HTML Picture 元素（推荐）
```astro
---
const { src, alt, className = '' } = Astro.props;
const baseName = src.replace(/\.[^/.]+$/, '');
---

<picture>
  <!-- Desktop -->
  <source 
    media="(min-width: 1024px)" 
    srcset={`${baseName}-desktop.jpg`}
    type="image/jpeg"
  >
  <!-- Tablet -->
  <source 
    media="(min-width: 768px)" 
    srcset={`${baseName}-tablet.jpg`}
    type="image/jpeg"
  >
  <!-- Mobile (fallback) -->
  <img 
    src={`${baseName}-mobile.jpg`}
    alt={alt}
    class={className}
    loading="lazy"
    decoding="async"
  >
</picture>
```

### 使用 R2 公共 URL
```astro
---
const R2_BASE_URL = import.meta.env.R2_PUBLIC_URL;
---

<img 
  src={`${R2_BASE_URL}/hero-solar-mobile.jpg`}
  srcset={`
    ${R2_BASE_URL}/hero-solar-mobile.jpg 640w,
    ${R2_BASE_URL}/hero-solar-tablet.jpg 1024w,
    ${R2_BASE_URL}/hero-solar-desktop.jpg 1920w
  `}
  sizes="(max-width: 768px) 640px, (max-width: 1024px) 1024px, 1920px"
  alt="Solar panel installation"
  loading="lazy"
/>
```

## AI Tools

### Recommended Tools
1. **Gemini** - https://gemini.google.com
   - Free, good quality
   - 15 images/day limit

2. **Midjourney** - https://midjourney.com
   - Highest quality
   - Requires subscription

3. **DALL-E 3** - https://openai.com/dall-e-3
   - Good quality
   - Requires OpenAI account

4. **豆包** - https://www.doubao.com
   - Chinese interface
   - Free tier available

### Prompt Tips
- Be specific about lighting and atmosphere
- Include aspect ratio in prompt
- Mention professional photography style
- Specify color palette if needed
- Use 8K or photorealistic for quality

## 完整工作流程

### 1. 生成原始图片
使用 AI 工具生成高分辨率原始图片（1920px 宽）

### 2. 生成三端版本
```bash
# 创建输出目录
mkdir -p output/mobile output/tablet output/desktop

# 批量处理
for img in *.jpg; do
  base=$(basename "$img" .jpg)
  magick "$img" -resize 640x -quality 85 "output/mobile/${base}.jpg"
  magick "$img" -resize 1024x -quality 85 "output/tablet/${base}.jpg"
  magick "$img" -resize 1920x -quality 90 "output/desktop/${base}.jpg"
done
```

### 3. 上传到 R2
```bash
npm run upload-images
```

### 4. 更新组件代码
使用 `<picture>` 元素或 `srcset` 加载响应式图片

### 5. 验证
- 检查不同设备上的图片加载
- 验证 R2 CDN 缓存
- 测试图片压缩质量
