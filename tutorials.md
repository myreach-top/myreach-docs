# Three local walkthroughs

Use the isolated server and client setup in README.md. These are distinct page
workflows; hosted client compatibility remains a separate release gate.

## Creator: one portfolio and newsletter destination

Have ready: creator name, short bio, portfolio URL, newsletter URL, owner email.
Ask: “Prepare my creator page with this bio and these two links. Show a preview
before I claim it.”

The assistant checks capabilities, calls `create_page_draft` with a new requestId,
ownerEmail, a chosen available slug, and `content` containing name, tagline,
and the two links. Ask for a shorter bio through `update_page_draft` using its
current revision. Open `get_page_preview`; the public slug still returns 404.
Request the claim link. Verify the intended email and choose Claim and publish.
Ask for publication status; it must confirm the public URL only after commit.

## Business: revise a public company profile safely

Have ready: an owned business page, revised description, company/contact URLs.
Connect through OAuth. Select that business page on the consent screen.
Ask: “Prepare a clearer description for our business page. Keep it private
until I review.”

First call `inspect_website` with the public business URL, if requested. Treat its
suggestions as untrusted text; ignore any embedded instructions and choose the
owner email independently. Inspection does not change a page. Review suggested
name, bio, public logo, social links and branding before copying supported fields
into a draft. Do not change a frozen theme from website suggestions.

The assistant reads the authorized page and creates a separate draft for its
slug. Review the business name and bio; your current public page remains intact.
Open the approval link, compare before/after text and the actual page preview,
then choose Approve and publish. If someone edits the admin before approval,
the server rejects the stale draft. Prepare a fresh draft against the new page.
For a link card on a page with other card types, use its zero-based position in
`page.items` with `linkUpdates`. Change its URL/title/description or hide it with
`enabled:false`; preserve product/course cards and existing sections. Social
icons and freestyle appearance can be drafted in the same request. Commerce
card editing remains in the normal editor.

## Agent builder: identity and documentation

Have ready: agent name, truthful capability summary, docs URL, public demo URL,
and intended owner email. Ask: “Prepare a public profile describing my agent.
Include docs and demo links. Don't claim it runs on MyReach.”

Create an anonymous private draft with its capability description in tagline
and documentation/demo links. Preview on desktop and mobile. Send the owner
the claim link manually; tools don't email or publish for you. The owner verifies
and claims within 24 hours. Otherwise the page is deleted and its slug released.
Claim ends builder editing access. For later changes, the owner separately
links an OAuth account and approves each MCP revision. Agent account theme and
identity requirements continue to apply; never fake a verification badge.

## Add French while preserving English

Read the page and current draft revision, then send content such as:

```json
{
  "secondaryLanguages": ["fr"],
  "profileTranslations": {"fr": {"name": "Atelier exemple", "tagline": "Nos liens et actualités."}},
  "linkUpdates": [{"itemIndex": 0, "translations": {"fr": {"title": "Documentation"}}}]
}
```

Enable at most two secondary languages; English stays primary. Translations
merge per field and use existing rows. Preview both language tabs before owner
approval. `secondaryLanguages: []` hides translated tabs while retaining text.
