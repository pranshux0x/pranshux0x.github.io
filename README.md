# pranshux0x

The source for [pranshux0x.github.io](https://pranshux0x.github.io), Priyanshu Shakya's personal blog about web application security, WebSocket security, bug bounty research, and technical lessons learned.

The site is built with [Jekyll](https://jekyllrb.com/) and the [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) theme, then published by GitHub Pages through the repository's GitHub Actions workflow.

## Publish a post with GitHub's web interface

1. Open this repository on GitHub and select the [`_posts`](./_posts) directory.
2. Select **Add file → Create new file**.
3. Name the file `YYYY-MM-DD-short-title.md`. For example, use `2026-09-08-testing-websocket-origins.md` for a post dated September 8, 2026.
4. Open [`docs/post-template.md`](./docs/post-template.md) in another tab, select **Raw**, and copy its contents into the new file.
5. Replace every placeholder in the front matter. Use a date such as `2026-09-08 10:30:00 +0530`; `+0530` is the offset for Asia/Kolkata. Keep categories and tags concise, and remove the optional `image` block when the post has no cover image.
6. Write the article below the front matter and use the **Preview** tab to check its Markdown.
7. Select **Commit changes**, enter a useful commit message, and commit to the default branch (or create a branch and pull request if you want a review first).
8. Open the repository's **Actions** tab and follow the newest **Build and Deploy** run. When it succeeds, the post will appear on the homepage, in search, and in its category, tag, and archive pages.

The date in the filename and the `date` field should agree. A future publication date will not appear until that time. Do not add `published: false` when the post is ready to go live.

## Upload and embed screenshots

Keep article images under `assets/img/posts/`, preferably in one folder per post:

```text
assets/img/posts/short-title/cover.png
assets/img/posts/short-title/request-response.png
```

In GitHub, open [`assets/img/posts`](./assets/img/posts). To create a post folder in the web interface, select **Add file → Create new file**, enter `short-title/.gitkeep` as the filename, and commit it. Open that folder, select **Add file → Upload files**, and commit the screenshots. Use descriptive lowercase filenames and optimize large images before uploading them.

Embed an image in Markdown with useful alternative text:

```markdown
![WebSocket handshake shown in the browser developer tools](/assets/img/posts/short-title/request-response.png)
```

To use an uploaded image as a cover, uncomment the `image` block in the article template and update its path and alternative text.

## Format code blocks

Put fenced code blocks on their own lines and add a language identifier for syntax highlighting:

````markdown
```http
GET /example HTTP/1.1
Host: example.test
```
````

Common identifiers include `http`, `javascript`, `python`, `bash`, `json`, and `yaml`. Never paste live credentials, session tokens, private program data, or unredacted personal information.

## Edit the About page and site settings

Open either file on GitHub, select the pencil icon (**Edit this file**), make the change, preview it, and commit it as described above.

- Edit [`_tabs/about.md`](./_tabs/about.md) to change the public biography or profile links.
- Edit [`_config.yml`](./_config.yml) to change the title, tagline, description, site URL, timezone, author metadata, or theme features.
- Preserve the YAML indentation in `_config.yml`. For this root GitHub Pages site, keep `baseurl: ""`.
- After changing `_config.yml`, let the deployment complete and check the generated pages, feed, sitemap, and metadata.

## Check deployment errors

1. Open **Actions** in the GitHub repository.
2. Select the most recent **Build and Deploy** workflow run.
3. Open the failed job and expand the first failed step to read the build message.
4. Check recent edits for malformed YAML, missing front-matter delimiters (`---`), an invalid post filename, or a bad file path.
5. Commit the correction; the workflow will run again automatically.

Also check **Settings → Pages** and confirm that **Source** is set to **GitHub Actions**. The deployment job shows the published URL when it succeeds.

## Protect unpublished and confidential work

This is a public repository: every committed draft, image, file revision, and deleted secret may remain visible in Git history. Keep unpublished research, program-confidential information, credentials, tokens, private reports, and identifying evidence outside this repository. Redact sensitive details before uploading anything. Use a separate private repository or local encrypted storage for drafts that are not ready for public disclosure.

## Connect a custom domain later

No custom domain is required; the site works at `https://pranshux0x.github.io` for free. If you obtain a domain later:

1. Read GitHub's current [custom-domain documentation](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).
2. Add and verify the domain for the GitHub account, then open **Settings → Pages → Custom domain** in this repository.
3. Enter the domain you own and add the DNS records GitHub specifies at your DNS provider. Do not include `https://` in the custom-domain field.
4. Wait for GitHub's DNS check to pass, test both the custom domain and `pranshux0x.github.io`, and then enable **Enforce HTTPS**.

Keep `baseurl` empty when the site is served from the root of either the GitHub Pages hostname or a custom domain.

## Theme and license

This repository began with the official [Chirpy Starter](https://github.com/cotes2020/chirpy-starter). Keep the theme configuration, dependencies, attribution, and the existing [MIT license](./LICENSE) when updating the site.
