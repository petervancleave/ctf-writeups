# CTF Writeups Blog

A modern, responsive blog site for sharing CTF (Capture The Flag) challenge writeups. Features a beautiful watercolor theme with ambient animations and markdown support.

## Features

- 🎨 Modern watercolor theme with ambient animations
- 📱 Fully responsive design
- ✍️ Markdown support for blog posts
- 🏷️ Category-based organization
- 🔍 Search functionality
- 🔖 Tag-based filtering
- 📊 Estimated read time calculation
- 🎯 Category-specific images

## Directory Structure

```
.
├── css/
│   ├── styles.css      # Main styles
│   ├── post.css        # Post page styles
│   └── archive.css     # Archive page styles
├── js/
│   ├── watercolor.js   # Watercolor animation
│   └── main.js         # Main functionality
├── posts/
│   ├── index.json      # Posts metadata
│   └── *.md            # Markdown files for posts
├── images/
│   └── categories/     # Category images
├── index.html          # Homepage
├── post.html           # Individual post page
└── archive.html        # Archive/search page
```

## Adding New Posts

1. Create a new markdown file in the `posts/` directory with the format: `YYYY-MM-DD-title.md`
2. Add the post metadata to `posts/index.json` with the following structure:
   ```json
   {
       "filename": "YYYY-MM-DD-title.md",
       "title": "Your Post Title",
       "date": "YYYY-MM-DD",
       "category": "Category Name",
       "tags": ["tag1", "tag2"]
   }
   ```

## Post Categories

- Web Exploitation
- Binary Exploitation
- Reverse Engineering
- Cryptography
- Digital Forensics
- OSINT
- Steganography
- Miscellaneous

## Local Development

1. Clone the repository
2. Set up a local web server (e.g., using Python's `http.server`):
   ```bash
   python -m http.server 8000
   ```
3. Open `http://localhost:8000` in your browser

## Customization

### Adding New Categories

1. Add the category to the select options in `archive.html`
2. Add a corresponding image in `images/categories/`
3. Update the `getCategoryImage` function in `main.js`

### Modifying the Theme

- Main colors and variables are defined in `css/styles.css`
- Watercolor animation can be customized in `js/watercolor.js`

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## License

MIT License - Feel free to use this template for your own CTF writeups blog! 