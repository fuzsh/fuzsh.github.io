# fuzsh.github.io - 90s Style Academic Portfolio

A nostalgic, 90s-style academic portfolio website featuring classic web design elements from the golden age of the internet!

## 🎨 Features

- **90s Aesthetic**: Classic grey backgrounds, blue links, purple visited links, and table-based layouts
- **Marquee Header**: Scrolling text that was all the rage in the 90s
- **Sections Include**:
  - About Me
  - News & Updates
  - Publications (Conference Papers, Journal Articles, Preprints)
  - Academic Services (PC Member, Reviewer, Organizing roles)
  - CV/Resume (Education, Experience, Awards, Teaching)
  - Contact Information
- **Sidebar**: Profile photo, quick contact info, and navigation links
- **Authentic 90s Elements**: Visitor counter placeholder, "Best viewed in Netscape" badges, classic fonts

## 📝 How to Update Your Portfolio

### Basic Information

Edit `index.html` and replace the placeholder text:

1. **Your Name**: Search for `[Your Name]` and replace with your actual name
2. **Position & Institution**: Replace `[Position]` and `[University/Institution]`
3. **Research Interests**: Update the research areas in the About section

### Profile Photo

Add your photo to the repository:
- Save your photo as `photo.jpg` in the root directory
- Or update the `src="photo.jpg"` in the HTML to point to your image file

### Publications

In the Publications section (`#papers`), update:
- Paper titles
- Author lists
- Conference/Journal names
- Years
- Add links to PDF files, BibTeX, code, etc.

### News & Updates

Update the table in the News section with your latest updates:
```html
<tr>
    <td><strong>Month Year</strong></td>
    <td>🎉 Your news item here</td>
</tr>
```

### Academic Services

Update the lists under Academic Services section:
- Program Committee memberships
- Reviewer positions
- Organizing committee roles
- Departmental service

### CV/Resume

Update the CV section with:
- Your education background
- Professional experience
- Honors & awards
- Teaching experience

Add your CV files (`cv.pdf`, `cv.doc`) to the repository.

### Contact Information

Update the contact table with:
- Your email address
- Office location
- Mailing address
- Phone number

## 🚀 Publishing to GitHub Pages

1. Push your changes to the `main` or `master` branch
2. Go to your repository Settings → Pages
3. Select the branch to deploy (usually `main`)
4. Your site will be live at `https://fuzsh.github.io`

## 🎯 Customization

### Colors

Edit `style.css` to change colors:
- `#000080` - Dark blue (headers)
- `#c0c0c0` - Classic grey (background)
- `#ffff00` - Yellow (accents)

### Layout

The site uses a classic table-based layout:
- Left column (30%): Sidebar with photo and quick links
- Right column (70%): Main content sections

### Adding New Sections

To add a new section, copy this template:
```html
<div id="newsection" class="section">
    <h2>🆕 New Section</h2>
    <hr>
    <p>Your content here...</p>
</div>
```

And add a link in the navigation:
```html
<a href="#newsection">New Section</a>
```

## 📱 Mobile Friendly

While maintaining the 90s aesthetic, the site includes basic responsive design for mobile devices.

## 🌟 Tips

- Keep the 90s aesthetic by using simple HTML and minimal JavaScript
- Use emojis sparingly to add visual interest (modern browsers support them well)
- Update the "Last updated" dates when you make changes
- Consider adding animated GIFs for that authentic 90s feel

## 📄 License

Feel free to use and modify this template for your own academic portfolio!

---

*Best viewed in Netscape Navigator 4.0 at 800x600 resolution* 😄