---
title: Images and Media
description: Learn how to add and manage images and media in BaiDocs
---

# Images and Media

Images and visual content make documentation more engaging and easier to understand. This guide covers everything you need to know about using images and media in BaiDocs.

## Image Basics

### Image Directory

All images should be placed in the `images/` directory of your book:

```
content/my-book/en/
├── index.md
├── guide.md
└── images/
    ├── screenshot.png
    ├── diagram.svg
    └── logo.jpg
```

:::info
The `images/` directory is special - it's excluded from navigation but files are accessible for embedding.
:::

### Basic Image Syntax

Use standard Markdown image syntax:

```markdown
![Alt text](images/filename.png)
```

### Image with Title

Add a title attribute for hover text:

```markdown
![Alt text](images/filename.png "Hover text")
```

### Relative Paths

BaiDocs automatically resolves relative image paths:

```markdown
<!-- Same directory -->
![Screenshot](images/screen.png)

<!-- Parent directory -->
![Logo](../images/logo.png)

<!-- Absolute path from content root -->
![Banner](/my-book/en/images/banner.png)
```

## Supported Formats

BaiDocs supports all common image formats:

| Format | Extension | Best For |
|--------|-----------|----------|
| **PNG** | `.png` | Screenshots, graphics with transparency |
| **JPG/JPEG** | `.jpg`, `.jpeg` | Photographs, complex images |
| **GIF** | `.gif` | Simple animations, icons |
| **SVG** | `.svg` | Logos, diagrams, icons (scalable) |
| **WebP** | `.webp` | Modern format, smaller file size |

:::tip
Use SVG for logos and diagrams - they scale perfectly at any size and have small file sizes.
:::

## Image Best Practices

### 1. Use Descriptive Alt Text

Alt text is important for accessibility and SEO:

```markdown
<!-- Good -->
![BaiDocs editor interface showing file tree and markdown editor](images/editor-ui.png)

<!-- Bad -->
![image1](images/img1.png)
```

### 2. Optimize Image Sizes

Keep images reasonably sized:

- **Screenshots**: 1200-1600px wide maximum
- **Thumbnails**: 200-400px wide
- **Diagrams**: SVG (scalable) or 800-1200px wide
- **Icons**: 32-128px or SVG

:::warning
Large images increase page load time and PDF file size. Optimize images before adding them.
:::

### 3. Use Meaningful Filenames

```markdown
<!-- Good -->
editor-interface.png
architecture-diagram.svg
getting-started-flow.png

<!-- Bad -->
img1.png
screen-shot-2024-01-01.png
untitled.jpg
```

### 4. Compress Images

Use tools to compress images:

- **PNG**: TinyPNG, OptiPNG
- **JPG**: JPEGmini, ImageOptim
- **SVG**: SVGO, SVGOMG

## HTML Image Syntax

For more control, use HTML `<img>` tags:

### Width and Height

```markdown
<img src="images/logo.png" alt="BaiDocs Logo" width="200" />

<img src="images/screenshot.png" alt="Screenshot" width="600" height="400" />
```

### Centered Images

```markdown
<div style="text-align: center;">
  <img src="images/diagram.svg" alt="Architecture Diagram" width="80%" />
</div>
```

### Images with Captions

Use `<figure>` and `<figcaption>`:

```markdown
<figure>
  <img src="images/workflow.png" alt="BaiDocs Workflow" />
  <figcaption>Figure 1: BaiDocs documentation workflow</figcaption>
</figure>
```

## Linking Images

Make images clickable:

```markdown
[![Click to enlarge](images/thumbnail.png)](images/full-size.png)
```

Link to external sites:

```markdown
[![Visit BaiDocs](images/logo.png)](https://github.com/lablup/baidocs)
```

## Image Examples

### Screenshots

For screenshots, include descriptive alt text:

```markdown
![BaiDocs viewer showing the documentation homepage with navigation sidebar, search bar, and main content area](images/viewer-homepage.png)
```

**Best practices for screenshots:**
- Crop to relevant area
- Use consistent screen resolution
- Consider dark/light theme
- Add annotations if needed

### Diagrams

SVG diagrams scale perfectly:

```markdown
![System architecture diagram showing the relationship between editor, viewer, content, and deployment](images/architecture.svg)
```

### Logos and Icons

Use SVG for sharp rendering:

```markdown
![BaiDocs logo](images/logo.svg)
```

Or specify size for raster images:

```markdown
<img src="images/logo.png" alt="BaiDocs Logo" width="48" height="48" />
```

## Image Organization

### Directory Structure

Organize images by category:

```
images/
├── screenshots/
│   ├── editor-ui.png
│   └── viewer-ui.png
├── diagrams/
│   ├── architecture.svg
│   └── workflow.svg
├── icons/
│   ├── logo.svg
│   └── favicon.png
└── misc/
    └── banner.jpg
```

Reference with subdirectories:

```markdown
![Editor UI](images/screenshots/editor-ui.png)
![Architecture](images/diagrams/architecture.svg)
```

## PDF Considerations

Images in documentation will appear in generated PDFs:

### PDF Image Optimization

Configure in `book.config.yaml`:

```yaml
pdf:
  imageOptimization:
    enabled: true
    maxWidth: 1200
    quality: 85
```

### Image Sizing for PDFs

- Use reasonable widths (max 1200px)
- Avoid very tall images
- Use high enough resolution for print

:::info
PDF generation automatically optimizes images based on your configuration.
:::

## Advanced Techniques

### Side-by-Side Images

Use HTML for layout:

```markdown
<div style="display: flex; gap: 20px;">
  <div style="flex: 1;">
    <img src="images/before.png" alt="Before" />
    <p style="text-align: center;">Before</p>
  </div>
  <div style="flex: 1;">
    <img src="images/after.png" alt="After" />
    <p style="text-align: center;">After</p>
  </div>
</div>
```

### Image Gallery

Create a simple gallery:

```markdown
<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px;">
  <img src="images/img1.png" alt="Image 1" />
  <img src="images/img2.png" alt="Image 2" />
  <img src="images/img3.png" alt="Image 3" />
  <img src="images/img4.png" alt="Image 4" />
  <img src="images/img5.png" alt="Image 5" />
  <img src="images/img6.png" alt="Image 6" />
</div>
```

### Responsive Images

Make images responsive:

```markdown
<img src="images/wide-diagram.png"
     alt="Wide Diagram"
     style="max-width: 100%; height: auto;" />
```

## External Images

You can link to external images:

```markdown
![External image](https://example.com/image.png)
```

:::warning
External images may:
- Break if the source is removed
- Slow down page loads
- Not appear in PDFs
- Have CORS issues

Prefer hosting images locally when possible.
:::

## Video and Embedded Media

### YouTube Videos

Embed YouTube videos with iframe:

```markdown
<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/VIDEO_ID"
  title="Video Title"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen>
</iframe>
```

Make it responsive:

```markdown
<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden;">
  <iframe
    style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
    src="https://www.youtube.com/embed/VIDEO_ID"
    frameborder="0"
    allowfullscreen>
  </iframe>
</div>
```

:::note
Embedded videos won't appear in PDFs. Consider providing a link or screenshot as fallback.
:::

### GIFs for Animations

Use GIFs for short demonstrations:

```markdown
![Animation showing how to create a new book](images/create-book.gif)
```

**GIF best practices:**
- Keep file size small (<5MB)
- Limit duration (5-10 seconds)
- Use tools like Gifski or ezgif.com
- Consider alternatives (video, static images)

## Image Upload (Editor)

The BaiDocs editor supports image upload:

1. **Drag and drop** images into the editor
2. **Click upload button** to browse files
3. **Paste images** from clipboard

Images are automatically:
- Saved to `images/` directory
- Named with UUIDs for uniqueness
- Inserted at cursor position

## Troubleshooting

### Image Not Displaying

If images don't show:

1. ✅ Check file path (case-sensitive)
2. ✅ Verify image is in `images/` directory
3. ✅ Check file extension
4. ✅ Refresh the browser
5. ✅ Check browser console for errors

### Broken Images in PDF

If images break in PDF:

1. ✅ Use relative paths (not absolute URLs)
2. ✅ Keep images reasonably sized
3. ✅ Check image format compatibility
4. ✅ Enable image optimization in config

### Images Too Large

If images are too large:

1. **Resize**: Use image editing software
2. **Compress**: Use optimization tools
3. **Format**: Convert to appropriate format
4. **Lazy load**: Consider for many images

## Accessibility

Make images accessible:

### Alt Text

Always provide descriptive alt text:

```markdown
<!-- Good -->
![Bar chart showing user growth from 2020 to 2024, rising from 100 to 10,000 users](images/growth-chart.png)

<!-- Bad -->
![chart](images/img.png)
```

### Decorative Images

For decorative images, use empty alt:

```markdown
![](images/decorative-divider.svg)
```

### Complex Images

For complex diagrams, provide a text description:

```markdown
![Architecture diagram](images/architecture.svg)

The architecture consists of three main components:
1. Editor - For creating and editing content
2. Viewer - For displaying documentation
3. Content - Markdown files and images
```

## Quick Reference

```markdown
<!-- Basic image -->
![Alt text](images/file.png)

<!-- Image with title -->
![Alt text](images/file.png "Title")

<!-- HTML image -->
<img src="images/file.png" alt="Alt" width="400" />

<!-- Figure with caption -->
<figure>
  <img src="images/file.png" alt="Alt" />
  <figcaption>Caption text</figcaption>
</figure>

<!-- Linked image -->
[![Alt](images/thumb.png)](images/full.png)
```

## Next Steps

Now let's explore [Theme Customization](theme-customization.md) to style your documentation!
