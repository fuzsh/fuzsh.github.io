# Site Structure Comparison

## Before (Old Structure)
```
fuzsh.github.io/
├── index.html          (full page with header/footer)
├── contact.html        (full page with header/footer)
├── cv.html             (full page with header/footer)
├── experience.html     (full page with header/footer)
├── publications.html   (full page with header/footer)
├── style.css           (in root)
└── static/
    ├── photo.jpg
    └── cv.pdf
```

**Issues:**
- ❌ Header and footer duplicated in every page
- ❌ CSS file in root directory (no organization)
- ❌ Mixed file types in root
- ❌ No template system
- ❌ Hard to maintain consistency

## After (New Jekyll Structure)
```
fuzsh.github.io/
├── _config.yml         ← Site configuration
├── _layouts/
│   └── default.html    ← Single template for all pages
├── _includes/
│   ├── header.html     ← Header component (used once)
│   ├── footer.html     ← Footer component (used once)
│   └── sidebar.html    ← Sidebar component (used once)
├── css/
│   └── style.css       ← Organized stylesheets
├── assets/
│   ├── images/
│   │   └── photo.jpg   ← Organized images
│   └── files/
│       └── cv.pdf      ← Organized files
├── index.html          (content only, uses layout)
├── contact.html        (content only, uses layout)
├── cv.html             (content only, uses layout)
├── experience.html     (content only, uses layout)
└── publications.html   (content only, uses layout)
```

**Benefits:**
- ✅ Header and footer defined once in `_includes/`
- ✅ CSS organized in `css/` directory
- ✅ Images in `assets/images/`, files in `assets/files/`
- ✅ Template-based system with layouts
- ✅ Easy to maintain and update
- ✅ Follows Jekyll best practices
- ✅ GitHub Pages compatible
- ✅ Scalable structure for future growth

## Code Reduction Example

### Before: Every page had ~60 lines of repeated code
```html
<!DOCTYPE html>
<html>
<head>...</head>
<body>
    <div class="header">...</div>      <!-- Duplicated -->
    <div class="navbar">...</div>      <!-- Duplicated -->
    <table>
        <tr>
            <td>
                <div class="sidebar">  <!-- Duplicated -->
                    ...
                </div>
            </td>
            <td>
                <!-- ACTUAL CONTENT -->
            </td>
        </tr>
    </table>
    <div class="footer">...</div>      <!-- Duplicated -->
</body>
</html>
```

### After: Pages now only contain content + 5 lines of config
```yaml
---
layout: default
title: Home
show_contact: true
---
<!-- ACTUAL CONTENT -->
```

**Result:** 
- 📉 ~90% reduction in code duplication
- 🎯 Pages now focus only on their unique content
- 🚀 Changes to header/footer apply to all pages instantly
- 🛠️ Easier to maintain and update
