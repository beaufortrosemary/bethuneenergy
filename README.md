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
   - On GitHub, go to Settings, then Collaborators, and invite Dylan by email so he can edit through Pages CMS.
   - Once he accepts, send him the Pages CMS link for the repository.

The old GitHub issue-form workflow files are still in `.github/` for now, but Pages CMS is the way to edit the newsroom going forward.

## How Dylan adds an item

1. Open the Pages CMS link and sign in with GitHub.
2. Open the News entry and click Add item under News items.
3. Fill in the date, category, title, and summary.
4. Choose the link type:
   - For a PDF, upload the file to the media library (it lands in `downloads/press/`), then set the URL field to that file's path, for example `downloads/press/2026-08-15-headline.pdf`, and set Link type to `pdf`.
   - For a story on another website, set Link type to `external` and paste the full address, starting with `https`, into the URL field.
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

For an external story, set `link_type` to `external` and `url` to the full address. The site shows "See the story" for external items and "Download PDF" for uploads.

## Fixing or removing a published item

Edit `news/news.json`: correct the fields, or delete the entry's block, and commit. Remove the PDF from `downloads/press/` if it should not remain available.

## Notes

- The three entries currently in `news/news.json` are placeholders with no PDFs behind them. Replace them with the real releases before launch, or empty the `items` array to `[]`.
- Category options live in `.pages.yml` (the News editor) and, for the older issue form, in `.github/ISSUE_TEMPLATE/news-item.yml`. The site itself renders whatever category string an entry carries.
