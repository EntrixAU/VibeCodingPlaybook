# Vibe Coding Playbook

A comprehensive guide to AI-assisted development that helps you harness the power of AI while maintaining security, governance, and best practices.

**Live Site**: [https://yourusername.github.io/VibeCodingPlaybook](https://yourusername.github.io/VibeCodingPlaybook)

## 📖 About

This documentation site is based on the presentation "Vibe Coding – Let's Do It Right" by Sam Fernando, CTO of Opex Consulting, originally presented at the ACS Forum. It provides practical, actionable guidance for implementing AI-assisted development in enterprise environments.

## 🚀 Quick Start

### Prerequisites

- Ruby 2.7 or higher
- Bundler gem
- Git

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/VibeCodingPlaybook.git
   cd VibeCodingPlaybook
   ```

2. **Install dependencies**
   ```bash
   bundle install
   ```

3. **Run the site locally**
   ```bash
   bundle exec jekyll serve
   ```

4. **Open your browser**
   Navigate to `http://localhost:4000` to view the site.

### GitHub Pages Deployment

This site is configured for automatic deployment to GitHub Pages using GitHub Actions.

#### Setup Instructions

1. **Fork or clone this repository** to your GitHub account

2. **Update configuration**
   Edit `_config.yml` and update these fields:
   ```yaml
   url: "https://yourusername.github.io"  # Replace with your GitHub username
   baseurl: "/VibeCodingPlaybook"         # Replace with your repository name
   ```

3. **Enable GitHub Pages**
   - Go to your repository settings
   - Navigate to "Pages" section
   - Under "Source", select "GitHub Actions"

4. **Push to main branch**
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

5. **Automatic deployment**
   - GitHub Actions will automatically build and deploy your site
   - Check the "Actions" tab to monitor deployment progress
   - Your site will be available at `https://yourusername.github.io/VibeCodingPlaybook`

## 📁 Site Structure

```
VibeCodingPlaybook/
├── _config.yml              # Jekyll configuration
├── _sass/                   # Custom SCSS styles
│   └── custom.scss
├── assets/
│   └── css/
│       └── style.scss       # Main stylesheet
├── .github/
│   └── workflows/
│       └── jekyll.yml       # GitHub Actions deployment
├── index.md                 # Homepage
├── methodology.md           # What is Vibe Coding?
├── evolution.md             # Evolution of Developer Assistance
├── perils.md                # Perils & Pitfalls
├── best-practices.md        # Best Practices & Guardrails
├── governance.md            # Governance Patterns
├── tools.md                 # Tools & Ecosystem
├── adoption.md              # Adoption Framework
├── cheat-sheet.md          # Quick Reference
├── Gemfile                 # Ruby dependencies
└── README.md               # This file
```

## 🎨 Customization

### Styling

The site uses the Minima theme with custom styling. To customize:

1. **Edit colors and fonts** in `_sass/custom.scss`
2. **Modify layout** by overriding Minima templates in `_layouts/`
3. **Add custom components** by creating new SCSS classes

### Content

1. **Update site information** in `_config.yml`
2. **Modify navigation** by editing the `header_pages` list in `_config.yml`
3. **Add new pages** by creating new Markdown files with proper front matter
4. **Update existing content** by editing the respective `.md` files

### Navigation

The site navigation is automatically generated from the `header_pages` list in `_config.yml`. To modify:

```yaml
header_pages:
  - index.md
  - methodology.md
  - evolution.md
  - perils.md
  - best-practices.md
  - governance.md
  - tools.md
  - adoption.md
  - cheat-sheet.md
```

## 🔧 Development

### Running Locally

```bash
# Install dependencies
bundle install

# Serve the site with live reload
bundle exec jekyll serve --livereload

# Build for production
JEKYLL_ENV=production bundle exec jekyll build
```

### Adding Content

1. **Create new page**
   ```markdown
   ---
   layout: page
   title: "Your Page Title"
   permalink: /your-page/
   ---

   # Your Page Title

   Your content here...
   ```

2. **Add to navigation** (optional)
   Add the filename to `header_pages` in `_config.yml`

3. **Test locally**
   ```bash
   bundle exec jekyll serve
   ```

## 📝 Content Guidelines

### Writing Style
- Use clear, actionable language
- Include practical examples and code snippets
- Provide both conceptual explanations and implementation details
- Use consistent formatting and structure

### Code Examples
- Use syntax highlighting with language specification
- Include complete, runnable examples when possible
- Add comments to explain complex logic
- Follow security best practices in all examples

### Security Considerations
- Never include real credentials or sensitive data
- Use placeholder values (e.g., `***MASKED***`)
- Include security warnings where appropriate
- Provide secure alternatives for common patterns

## 🤝 Contributing

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes**
4. **Test locally**
   ```bash
   bundle exec jekyll serve
   ```
5. **Commit and push**
   ```bash
   git add .
   git commit -m "Add your feature description"
   git push origin feature/your-feature-name
   ```
6. **Create a Pull Request**

### Contribution Guidelines
- Follow the existing content structure and style
- Include practical examples with your additions
- Test all code examples before submitting
- Update navigation if adding new pages
- Ensure all links work correctly

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Contact

- **Author**: Sam Fernando
- **Organization**: Opex Consulting
- **Email**: sam@opexconsulting.com.au

## 🙏 Acknowledgments

- Original presentation delivered at ACS Forum
- Jekyll and Minima theme developers
- GitHub Pages for hosting
- The AI development community for insights and best practices

## 🔗 Related Resources

- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [Cursor IDE](https://cursor.sh/)
- [OWASP AI Security Guidelines](https://owasp.org/www-project-ai-security-and-privacy-guide/)
- [Model Context Protocol](https://modelcontextprotocol.io/)

---

**Note**: Replace `yourusername` with your actual GitHub username in URLs and configuration files.
