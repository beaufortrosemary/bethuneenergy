# Bethune Energy website

Static site for bethuneenergy.com. Netlify hosts the site and deploys it automatically from this GitHub repository. The newsroom is data driven: entries live in `news/news.json`, PDFs live in `downloads/press/`, and `js/news.js` renders the list on the home page and on `news.html`. Dylan edits the newsroom through Pages CMS (pagescms.org), a friendly editing layer on top of this repository. There is no build step; the repository is served as-is.

## One-time setup

1. Create the GitHub repository.
   - Create a public repository named `bethuneenergy`.
   - Push these files to the `main` branch.
2. Connect the repository to the existing Netlify site.
   - In Netlify, open the current bethuneenergy.com site, go to Site configuration, then Build & deploy, then Continuous deployment, and link this GitHub repository.
   - Leave the build command empty and set the publish directory to the repository root. The site is served as-is, so there is no build step.
   - Netlify keeps the custom domain and HTTPS that are already set up for the site. From now on, every push to `main` deploys automatically, usually within a minute or two.
3. Connect the repository to Pages CMS.
   - Go to pagescms.org and sign in with GitHub.
   - Authorize Pages CMS for the `bethuneenergy` repository and open it. The `.pages.yml` file in this repository defines the News editor and the PDF media library (`downloads/press/`).
4. Invite Dylan.
   - In Pages CMS, open this project and invite Dylan by his email address. He receives a passwordless magic-link login and can edit the newsroom without a GitHub account. (Per-user permissions for email-invited editors are limited for now, so he gets broad edit access to the project's content, which is fine here.)
   - Alternative: add him as a GitHub collaborator with the Write role under the repo's Settings, then Collaborators. He then signs in to Pages CMS with his own GitHub account.

## How Dylan adds an item

1. Open the Pages CMS link and sign in with GitHub.
2. Open the News entry and click Add item under News items.
3. Fill in the date, category, title, and summary.
4. Choose the link type and add the link:
   - **PDF file** (the usual case): in the **PDF file** field, click and upload the PDF. It is stored in `downloads/press/` automatically, so there are no paths to type.
   - **Link to another website**: leave the PDF field empty and paste the full address, starting with `https`, into the **Website link** field.
5. Save. Pages CMS commits the change to the repository, and Netlify redeploys the site within a minute or two.

## The news.json entry format

To publish something yourself without Pages CMS, edit `news/news.json` directly and add the PDF to `downloads/press/`. The entry format:

```json
{
  "date": "2026-08-15",
  "category": "Press release",
  "title": "Headline here",
  "summary": "One or two sentences.",
  "link_type": "pdf",
  "url": "downloads/press/2026-08-15-headline-here.pdf"
}
```

For an external story, set `link_type` to `external`, leave `url` empty, and put the full address in an `external_url` field:

```json
{
  "date": "2026-08-15",
  "category": "In the news",
  "title": "Headline here",
  "summary": "One or two sentences.",
  "link_type": "external",
  "external_url": "https://www.example.com/story"
}
```

The site shows "See the story" for external items and "Download PDF" for uploads.

## Fixing or removing a published item

Edit `news/news.json`: correct the fields, or delete the entry's block, and commit. Remove the PDF from `downloads/press/` if it should not remain available.

## Notes

- `news/news.json` currently holds a single clearly labeled DEMO item with a working sample PDF (`downloads/press/demo-news-item.pdf`), there so you can test the layout and the Download PDF button. Delete it (and its PDF) before launch, then add the real releases.
- If every item is removed, the newsroom simply shows "No announcements have been posted yet." An empty or `{ "items": [] }` file is handled gracefully.
- Category options live in `.pages.yml` (the News editor). The site itself renders whatever category string an entry carries.
