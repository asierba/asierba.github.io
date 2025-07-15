# Asier's Blog

A Jekyll blog built with Gulp and BrowserSync for local development.

## Prerequisites

- **Ruby** (version 3.0+)
- **Node.js** (version 16+)
- **Homebrew** (for macOS)

## Setup Instructions

### 1. Install Ruby Version Manager (rbenv)

```bash
# Install rbenv
brew install rbenv

# Add rbenv to your shell profile (for zsh)
echo 'eval "$(rbenv init -)"' >> ~/.zshrc
source ~/.zshrc

# Install latest Ruby
rbenv install 3.4.4
rbenv global 3.4.4
rbenv rehash
```

### 2. Install Jekyll

```bash
gem install jekyll bundler
```

### 3. Install Node.js Dependencies

```bash
npm install
```

### 4. Run the Development Server

```bash
npx gulp
```

This will:

- Build the Jekyll site with drafts included
- Start a local server at `http://localhost:3000`
- Enable live reloading when files change
- Provide a BrowserSync UI at `http://localhost:3001`

### Alternative: Without Gulp

If you prefer to use Jekyll directly:

```bash
jekyll serve --drafts
```

The site will be available at `http://localhost:4000`.

## Project Structure

- `_posts/` - Published blog posts
- `_drafts/` - Draft posts (included in development)
- `_layouts/` - HTML templates
- `_includes/` - Reusable HTML components
- `_sass/` - Sass stylesheets
- `assets/` - Images and other assets
- `_config.yml` - Jekyll configuration

## Development

- Posts are written in Markdown
- Drafts are automatically included when running `npx gulp`
- The site rebuilds automatically when files change
- BrowserSync provides live reloading across devices
