# ntphiep.github.io

A multilingual personal blog built with [Hugo](https://gohugo.io/) and the [Blowfish](https://blowfish.page/) theme. This site features content in English, Spanish, and Vietnamese, focusing on technical topics including data engineering, cloud computing, and software development.

## 🌟 Features

- **Multilingual Support**: Content available in English, Spanish, and Vietnamese
- **Modern Design**: Built with the Blowfish theme for a clean, responsive layout
- **Static Site Generation**: Fast, secure, and SEO-friendly using Hugo
- **Blog Posts**: Technical articles and tutorials on AWS Lambda, DuckDB, Delta Lake, and more
- **Custom Styling**: Enhanced with custom CSS for a personalized look

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- [Hugo](https://gohugo.io/installation/) (Extended version recommended)
- [Git](https://git-scm.com/downloads)
- A terminal/command line interface

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/ntphiep/ntphiep.github.io.git
cd ntphiep.github.io
```

### 2. Load the Theme

The project uses the Blowfish theme as a submodule. Initialize it with:

```bash
make load_theme
# Or manually:
cd themes
git clone https://github.com/nunocoracao/blowfish.git
cd ..
```

### 3. Run the Development Server

Start the Hugo development server:

```bash
hugo server -D
```

The site will be available at `http://localhost:1313/`

### 4. Build for Production

Generate the static site files:

```bash
hugo
```

The built site will be in the `public/` directory.

## 📁 Project Structure

```
├── assets/              # CSS, images, and other assets
├── config/              # Hugo configuration files
│   └── _default/        # Default configuration
├── content/             # Markdown content files
│   ├── about/           # About pages
│   └── posts/           # Blog posts
├── layouts/             # Custom layouts and partials
├── static/              # Static files
├── themes/              # Hugo themes
│   └── blowfish/        # Blowfish theme
└── public/              # Generated site (after build)
```

## ✍️ Creating New Content

### Create a New Blog Post

```bash
hugo new content/posts/your-post-title/index.md
```

### Create Content in Multiple Languages

Create separate files with language codes:
- `index.en.md` - English
- `index.es.md` - Spanish
- `index.vn.md` - Vietnamese

## ⚙️ Configuration

The site configuration is split across multiple files in `config/_default/`:

- `hugo.toml` - Main site configuration
- `params.toml` - Theme parameters
- `menus.*.toml` - Navigation menus for each language
- `languages.*.toml` - Language-specific settings
- `markup.toml` - Markdown rendering configuration

## 🌐 Languages

This site supports three languages:
- **Vietnamese (vn)** - Default language
- **English (en)**
- **Spanish (es)**

Switch languages using the language selector in the navigation menu.

## 🎨 Customization

- **Custom CSS**: Add styles to `assets/css/custom.css`
- **Extend Head**: Modify `layouts/partials/extend-head.html`
- **Extend Footer**: Modify `layouts/partials/extend-footer.html`

## 📝 Available Make Commands

```bash
make load_theme    # Clone the Blowfish theme
```

## 🔗 Useful Links

- [Hugo Documentation](https://gohugo.io/documentation/)
- [Blowfish Theme Documentation](https://blowfish.page/docs/)
- [Hugo Multilingual Mode](https://gohugo.io/content-management/multilingual/)

## 📄 License

This project is open source and available for personal and educational use.

## 👤 Author

**ntphiep**
- GitHub: [@ntphiep](https://github.com/ntphiep)

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!

---

Built with ❤️ using Hugo and Blowfish
