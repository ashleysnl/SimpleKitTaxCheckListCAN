# Canadian Family Tax Custom Checklist

This repo contains a production-ready static SimpleKit web app for organizing family tax-season tasks, receipts, slips, contacts, and filing follow-ups.

The app preserves the shared SimpleKit shell:

- shared core shell loaded from `https://core.simplekit.app`
- existing Google Analytics head snippet kept in place
- static-site-friendly architecture with no build step required

## What The App Does

The checklist is designed for recurring yearly use:

- starts with a default Canadian family tax checklist
- supports editable categories, items, and nested sub-items
- stores notes and contact details on each item
- auto-saves the active planner in `localStorage`
- saves and loads the planner as a reusable checklist file
- clears checkmarks without deleting custom structure
- supports a blank restart and default-template restore
- includes search, status filtering, confirmation modals, and print styles

## File Structure

```text
/
  index.html
  calculator-spec.yaml
  assets/
    css/
      styles.css
    js/
      app.js
      simplekit-tool-links.js
```

## Data Structure

The planner uses a versioned client-side checklist schema stored as JSON:

```json
{
  "meta": {
    "app": "Canadian Family Tax Custom Checklist",
    "version": 1,
    "savedAt": "ISO timestamp"
  },
  "settings": {
    "familyName": "",
    "taxYearLabel": "2025 Tax Season",
    "notes": ""
  },
  "categories": [
    {
      "id": "cat-...",
      "title": "Medical Receipts",
      "description": "Track providers, receipts, follow-ups, and notes",
      "collapsed": false,
      "items": [
        {
          "id": "item-...",
          "title": "Freshwater Road Pharmacy",
          "checked": false,
          "notes": "",
          "contactName": "",
          "contactEmail": "",
          "contactPhone": "",
          "tags": ["medical"],
          "children": [
            {
              "id": "item-...",
              "title": "Prescription summary requested",
              "checked": false,
              "notes": "",
              "contactName": "",
              "contactEmail": "",
              "contactPhone": "",
              "tags": [],
              "children": []
            }
          ]
        }
      ]
    }
  ]
}
```

## Where Key Logic Lives

- Default checklist seed data: [`assets/js/app.js`](/Users/AshleySkinner/Documents/00_Engineering/04_Code/50_SimpleKit App Factory/workspaces/tax-checklist/assets/js/app.js)
  The default categories are created in `getDefaultCategories()`.
- Planner state, rendering, validation, persistence, and modal logic: [`assets/js/app.js`](/Users/AshleySkinner/Documents/00_Engineering/04_Code/50_SimpleKit App Factory/workspaces/tax-checklist/assets/js/app.js)
- Tool-specific UI layout and print styles: [`assets/css/styles.css`](/Users/AshleySkinner/Documents/00_Engineering/04_Code/50_SimpleKit App Factory/workspaces/tax-checklist/assets/css/styles.css)
- SEO metadata and SimpleKit shell hooks: [`index.html`](/Users/AshleySkinner/Documents/00_Engineering/04_Code/50_SimpleKit App Factory/workspaces/tax-checklist/index.html)

## Save And Load Checklist Files

- Save Checklist uses a client-side Blob download and saves the current planner as a `.json` checklist file.
- Load Checklist reads a local `.json` checklist file in the browser, validates the schema, normalizes missing IDs when possible, and asks for confirmation before replacing the active planner.
- Invalid checklist files fail gracefully with a friendly message shown in the app.

## LocalStorage Behavior

- The active planner auto-saves to `localStorage` under `simplekit.taxChecklist.planner.v1`.
- Saved checklist files are separate from browser storage and are not required for normal use.
- If saved local storage is corrupted, the app falls back safely to the default checklist, shows a warning banner, and offers a reset action.

## Maintenance Notes

- Keep the Google Analytics snippet in `index.html`.
- Keep the `window.SimpleKitPage` setup and shared core asset loading intact.
- Keep the app client-side only so it remains deployable as a static site.
