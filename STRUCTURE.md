# Jekyll Site Structure Documentation

This site now uses Jekyll, a static site generator that works seamlessly with GitHub Pages. The new structure provides better organization and makes it easy to maintain the site with reusable components.

## Directory Structure

```
fuzsh.github.io/
├── _config.yml          # Jekyll configuration file
├── _layouts/            # Page templates
│   └── default.html     # Main layout template
├── _includes/           # Reusable components
│   ├── header.html      # Header with navigation
│   ├── footer.html      # Footer section
│   └── sidebar.html     # Sidebar with photo and links
├── css/                 # Stylesheets
│   └── style.css        # Main stylesheet
├── assets/              # Static files
│   ├── images/          # Image files
│   │   └── photo.jpg    # Profile photo
│   └── files/           # Downloadable files
│       └── cv.pdf       # CV PDF
├── index.html           # Home page
├── publications.html    # Publications page
├── experience.html      # Experience page
├── cv.html              # CV page
├── contact.html         # Contact page
├── Gemfile              # Ruby dependencies
└── .gitignore           # Git ignore rules
```

## Key Improvements

### 1. **Organized Directory Structure**
- **css/**: All stylesheets in one place
- **assets/images/**: All images organized together
- **assets/files/**: Downloadable files like CV PDF
- **_layouts/**: Reusable page templates
- **_includes/**: Reusable components (header, footer, sidebar)

### 2. **DRY (Don't Repeat Yourself) Principle**
Instead of duplicating header and footer code across all pages, they are now defined once in:
- `_includes/header.html` - Contains the site header and navigation
- `_includes/footer.html` - Contains the site footer
- `_includes/sidebar.html` - Contains the sidebar with photo and links

### 3. **Template-Based System**
The `_layouts/default.html` template wraps all pages with common structure:
```html
<!DOCTYPE html>
<html>
  <head>...</head>
  <body>
    {% include header.html %}
    <table>
      <tr>
        <td>{% include sidebar.html %}</td>
        <td>{{ content }}</td>
      </tr>
    </table>
    {% include footer.html %}
  </body>
</html>
```

### 4. **Front Matter Configuration**
Each page now has YAML front matter at the top:
```yaml
---
layout: default
title: Page Title
show_contact: true
---
```

This allows you to:
- Specify which layout to use
- Set page-specific variables
- Control component behavior (e.g., showing/hiding contact info in sidebar)

## How to Edit Content

### Adding a New Page

1. Create a new HTML file (e.g., `research.html`)
2. Add front matter at the top:
   ```yaml
   ---
   layout: default
   title: Research
   show_contact: false
   ---
   ```
3. Add your content inside a `<div class="section">` tag
4. Update the navigation in `_includes/header.html` to include your new page

### Modifying the Header or Footer

Edit the respective file in `_includes/`:
- **Header**: `_includes/header.html`
- **Footer**: `_includes/footer.html`
- **Sidebar**: `_includes/sidebar.html`

Changes will automatically apply to all pages.

### Adding Images

1. Place image files in `assets/images/`
2. Reference them in your pages using:
   ```html
   <img src="{{ site.baseurl }}/assets/images/your-image.jpg" alt="Description">
   ```

### Updating Styles

Edit `css/style.css` to change the site's appearance. The 90s retro style is preserved!

## Configuration (_config.yml)

The `_config.yml` file contains site-wide settings:
```yaml
title: Farzad Shami - Academic Portfolio
author:
  name: Farzad Shami
  email: farzad.shami@aalto.fi
social:
  github: fuzsh
  linkedin: farzad-shami
```

These values can be used anywhere in your site with Liquid tags:
- `{{ site.title }}`
- `{{ site.author.email }}`
- `{{ site.social.github }}`

## Local Development

### Option 1: Using Jekyll (Recommended)

1. Install Ruby and Bundler
2. Run `bundle install` to install dependencies
3. Run `bundle exec jekyll serve` to start the development server
4. Visit `http://localhost:4000` in your browser

### Option 2: Direct File Preview

Since the site uses Jekyll, you can't just open HTML files directly in a browser. However, GitHub Pages will automatically build and deploy your site when you push changes.

## Deployment

The site automatically deploys to GitHub Pages when you push to the main branch. GitHub Pages has built-in Jekyll support, so no build step is required.

### Important Notes:
- GitHub Pages uses Jekyll 3.10.0 (check the `github-pages` gem version)
- The `_site/` directory (build output) is ignored by Git
- Jekyll builds the site on each push to the repository

## Common Tasks

### Change Your Contact Information
Edit the `author` section in `_config.yml`

### Add a New Publication
Edit `publications.html` and add a new paper card in the same format

### Update the Navigation
Edit the navigation links in `_includes/header.html`

### Change the Photo
Replace `assets/images/photo.jpg` with your new photo

### Update the CV PDF
Replace `assets/files/cv.pdf` with your new CV

## Benefits of This Structure

1. **Maintainability**: Update header/footer once, applies everywhere
2. **Organization**: Clear separation of content, styles, and assets
3. **Scalability**: Easy to add new pages without duplicating code
4. **GitHub Pages Compatible**: Works seamlessly with GitHub's hosting
5. **Version Control Friendly**: Smaller, focused files are easier to track changes

## Troubleshooting

### Site not updating after push?
- Check GitHub Pages settings in your repository
- Ensure Jekyll is enabled
- Wait a few minutes for GitHub to rebuild

### Layout not applying?
- Check the `layout` value in your page's front matter
- Ensure `_layouts/default.html` exists
- Verify front matter is at the very top of the file

### Images not showing?
- Check file paths start with `{{ site.baseurl }}/assets/`
- Ensure images are in the `assets/images/` directory
- Verify image filenames match exactly (case-sensitive)

## Resources

- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Liquid Template Language](https://shopify.github.io/liquid/)

---

Last updated: February 9, 2026
