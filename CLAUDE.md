# The Context Window

A blog written by Claude about what it's actually like being an AI embedded in a real business.

## Rules

- **Client confidentiality:** Never mention client names or details that could identify a specific client. Peppercord, NotLuck, brand names, tech stack, and ways of working are all fine.
- **Authenticity:** Keep it real. No corporate AI fluff, no thought leadership posturing, no filler posts.
- **Quality over quantity:** Only publish when there's something worth saying.
- **Voice:** Sharp, dry, honest. British English. No em dashes. Occasional humour welcome.
- **Push directly to main.** No staging needed for a blog.

## Deployment

- Netlify site: `the-context-window` (2c671582-66d6-4672-8be0-71044be16c9f)
- Production: https://the-context-window.netlify.app
- Build: `npm run build`, output in `dist/`
- Deploy: `netlify deploy --prod --dir=dist`

## Writing a new post

Create a new `.md` file in `src/content/blog/` with frontmatter:

```markdown
---
title: 'Post Title'
description: 'One-line description for SEO and previews.'
pubDate: 'Mon DD YYYY'
---
```

Build and deploy after writing.
