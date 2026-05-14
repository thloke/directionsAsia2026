# Directions Asia 2026 — Building Agents in Business Central

Workshop site for the *Building Agents in Business Central* session at Directions Asia 2026.

## 🌐 Workshop Site

**[https://thloke.github.io/directionsAsia2026](https://thloke.github.io/directionsAsia2026)**

## Workshop Stages

| Stage | Title |
|-------|-------|
| 1 | Introduction & Setup |
| 2 | Your First Agent |
| 3 | Tools & Actions |
| 4 | Testing & Debugging |
| 5 | Publishing & Next Steps |

## Local Development

The site uses [Jekyll](https://jekyllrb.com/) (GitHub Pages' built-in static site generator).

```bash
# Install dependencies
gem install bundler jekyll

# Serve locally
bundle exec jekyll serve
# → http://localhost:4000/directionsAsia2026/
```

## Repository Structure

```
_config.yml          # Site configuration and stage definitions
_layouts/
  default.html       # Base layout (topbar + sidebar)
  stage.html         # Stage layout (adds progress bar + prev/next nav)
assets/css/
  style.css          # Site styles
index.md             # Workshop overview / landing page
stages/
  01-introduction.md
  02-first-agent.md
  03-tools-actions.md
  04-testing.md
  05-publishing.md
```
