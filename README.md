# Blog-Writing

Source repository for the [xCloud](https://xcloud.host/) blog.

## Structure

```
src/content/blog/          Markdown posts (one file per post)
public/_landing/blog/      Cover images (PNG + WebP)
```

## Post frontmatter

Each post in `src/content/blog/` carries:

| Field | Purpose |
|---|---|
| `title` | H1 / SEO title |
| `slug` | URL slug, no year |
| `description` | Meta description, 140-160 chars |
| `focusKeyword` | Primary target keyword |
| `category` | `Guide`, `Features`, or `Update` |
| `pubDate` | ISO publication date |
| `draft` | `true` hides the post from the index |
| `ogImage` | Path to the 1200x630 cover image |
| `tags` | Topic tags |
| `faq` | List of `question` / `answer` pairs |

## Workflow

Posts are written on a `blog/<slug>` branch and merged into `main` via pull request.

Image placeholders use the format `![Alt text](IMAGE: description of the visual)`
and are replaced with real assets before publication.
