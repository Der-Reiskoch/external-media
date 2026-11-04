# External Media

Central media repository for [Der Reiskoch](https://www.der-reiskoch.de/) and related projects. It stores images that are maintained separately from the
website repositories and publishes them through GitHub Pages.

## Delivery

Files on `main` are available below:

```text
https://der-reiskoch.github.io/external-media/<path>
```

For example:

```text
https://der-reiskoch.github.io/external-media/youtube/2hdlc6szjZs.webp
```

The root-level `.nojekyll` file disables Jekyll processing. This is required because YouTube video IDs can start with an underscore.

## Directory structure

| Directory     | Contents                                      | Naming convention                        |
| ------------- | --------------------------------------------- | ---------------------------------------- |
| `youtube/`    | Archived YouTube preview images               | `<video-id>.webp`                        |
| `ahaan-thai/` | Thai dish photography                         | `<letter>/<dish_name>.jpg`               |
| `diary/`      | Food diary images                             | Organized by year                        |
| `flyer/`      | Event and marketing flyers                    | `<year>/<YYYYMMDD_location>.jpg`          |
| `pins/`       | Pinterest images                              | `<category>/<item>/pin.jpg`               |
| `shopify/`    | Shopify product images                        | Descriptive lowercase filename           |

YouTube previews use WebP only. Do not retain the downloaded JPG source. Other directories may use JPG, PNG or WebP according to their existing conventions.

## YouTube previews

Download every missing YouTube preview referenced by the `ahaan-thai` content:

```bash
npm run download:youtube
```

Process one content file only:

```bash
npm run download:youtube -- ../ahaan-thai/content/de/lexikon/g/example.md
```

The script tries YouTube's WebP variants in this order:

1. `sddefault.webp`
2. `maxresdefault.webp`
3. `hqdefault.webp`

The archived files are fallbacks for videos whose preview is no longer available from YouTube. The public API normally requests YouTube first and then tries
GitHub Pages and Raw GitHub:

```text
https://api.koch-reis.de/youtube/v1/?file=<video-id>.webp
```

## Deployment

Deploy all pending media changes:

```bash
npm run deploy
```

This command normalizes `.JPG` and `.jpeg` extensions outside `youtube/`, stages the complete working tree, creates an `auto-deploy: added new media` commit and
pushes it. Under `youtube/`, it removes a JPG only when the matching WebP exists and aborts if the WebP is missing. Review `git status` before running it because
the deployment includes every pending file.

GitHub Pages may take several minutes to publish a large media commit.

## Repository size

Deleting a file removes it from the current checkout and GitHub Pages, but not from Git history. `npm run clear:gitHistory` rewrites the complete repository
history and force-pushes a new root commit. Use it only as an intentional maintenance operation after verifying a complete backup.
