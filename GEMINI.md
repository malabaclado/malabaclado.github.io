# Project Overview: Mark Labaclado's Personal Website

This project is a personal website and blog built with [Jekyll](https://jekyllrb.com/) using the [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy/) theme. It is hosted on GitHub Pages at [malabaclado.github.io](https://malabaclado.github.io).

## Key Technologies
- **Jekyll**: Static site generator.
- **Ruby & RubyGems**: Core environment and dependency management.
- **Chirpy Theme**: A responsive, feature-rich Jekyll theme designed for technical writing.
- **GitHub Actions**: Automated deployment via `.github/workflows/pages-deploy.yml`.

## Directory Structure
- `_config.yml`: Main site configuration (title, social links, theme settings).
- `_posts/`: Blog post content in Markdown (Format: `YYYY-MM-DD-title.md`).
- `_tabs/`: Source files for navigation tabs (Archives, Categories, Projects, Tags).
- `_data/`: Data files for customizing contact links (`contact.yml`) and sharing options (`share.yml`).
- `assets/`: Static assets including images (`img/`) and icons (`favicons/`).
- `tools/`: Utility scripts for development and testing.

## Local Development

### Prerequisites
- Ruby (check `Gemfile` for version compatibility).
- Bundler (`gem install bundler`).

### Commands
| Task | Command |
| :--- | :--- |
| **Install Dependencies** | `bundle install` |
| **Run Locally (Default)** | `bundle exec jekyll serve` |
| **Run Locally (Custom)** | `bash tools/run.sh` (includes live reloading and polling options) |
| **Build for Production** | `JEKYLL_ENV=production bundle exec jekyll build` |
| **Test Site** | `bash tools/test.sh` |

## Development Conventions

### Adding Posts
- Create a new Markdown file in `_posts/` following the `YYYY-MM-DD-title.md` naming convention.
- Ensure the front matter includes `title`, `date`, and `categories`/`tags`.

### Customization
- **Site Metadata**: Update `_config.yml` for title, description, and social links.
- **Contact Links**: Modify `_data/contact.yml` to update sidebar social icons.
- **Styles**: Custom styles should ideally be placed in the theme-appropriate `_sass` or `assets` directories (refer to Chirpy documentation).

### Deployment
- Changes pushed to the `main` branch are automatically built and deployed to GitHub Pages via GitHub Actions.
