# Expanded CLI command areas

Observed with CLI 0.4.9 against dev. Discover the target server's tree and nested help before acting; this is a routing guide, not a fixed argument schema.

```text
Account -> event -> shared content -> Event App / website
                -> registration -> participants -> check-in
                -> surveys / campaigns / reports / Lead Capture
```

## Choose the right command area

| Need | Discovery / read commands | Write commands to inspect with help |
|------|---------------------------|-------------------------------------|
| Event lifecycle and copies | `events current`, `events templates list`, `events branding show` | `events duplicate`, `events url`, `events update`, `events cancel`, `events archive`, `events reactivate`, `events delete`, `events branding update` |
| Account team / event team | `accounts members`, `accounts roles`, `events team list` | `events team add`, `invite`, `reinvite`, `remove`, `activate` |
| Eventbrite | `accounts integrations eventbrite status`, `accounts integrations eventbrite catalog`, `events integrations eventbrite status` | Account `connect`, `refresh`, `import`; event `link`, `sync`, `sync_status` |
| Shared speakers | `content speakers list`, `content speakers directories list`, `content speakers categories list` | Speaker, directory and category `create`, `update`, `delete`, `sort`; directory/category `assign` |
| Shared sponsors | `content sponsors list`, `content sponsors tiers list` | Sponsor/tier `create`, `update`, `delete`, `sort`; tier `assign` |
| Agenda | `content schedule settings`, `content schedule days list`, `sessions list`, `tracks list`, `locations list` | `content schedule days` and `content schedule sessions/tracks/locations` CRUD; schedule `update` for JSON arrays |
| Documents | `app documents`, `app folders` | `app create_document`, `update_document`, `delete_document`, `create_folder`, `sort_documents`; also under `content documents` |
| Event App | `app show`, `app settings`, `app modules`, `app contents` | `app update`, `set_modules`, `update_content`, `publish`, `unpublish` |
| App branding and navigation | `app branding show`, `app navigation show`, `app languages show` | `app branding update`, `app navigation update/icon`, `app languages update/translate/label`; `app actions` aliases navigation |
| App pages, photos, streams | `app pages list`, `app photos list`, `app streams list` | Each area's `create`, `update`, `delete`; photos/streams `sort` |
| Maps and custom lists | `app sections maps`, `app sections custom_lists`, `app sections section_access` | `app sections` map/pin/web-view/custom-list commands; `app sections lists` link groups/links; `set_section_access` |
| Live Q&A / polls | `app interaction status`, `settings`, `questions`, `polls`, `templates list` | `create_poll`, `start_poll`, `stop_poll`, `reset_poll`, `export_poll`, question moderation, `comments moderate`, `word-cloud update`, `set_screen`, `set_audience` |
| Website | `site settings`, `site pages list`, `site sections list`, `site branding show` | `site update`, `template`, `publish`, `unpublish`; pages/sections CRUD, source upload/download and sorting; `site faq`, `links`, `button`, `head`, `navbar` |
| Forms | `forms show`, `forms configuration show`, `forms design show`, `forms languages list`, `forms notifications list` | `forms create`, `checkout`, `duplicate`, `delete`; configuration/design/languages/messages/notifications subcommands |
| Ticketing | `registration show`, `ticket-types list`, `orders list`, `registration promos list`, `registration waitlist list` | Registration/ticket/promo updates; waitlist `invite`, `remove`, `bulk_invite`, `bulk_remove` |
| RSVP | `pax show`, `pax rsvp help change`, `pax rsvp help bulk` | Explicit `pax rsvp change` / `bulk`; inspect accepted actions and IDs first |
| Surveys | `surveys list`, `surveys show`, `surveys responses`, `surveys analytics` | Survey/question/section/choice CRUD; `surveys sort`, `update_settings`, `delete_response` |
| Lead Capture | `leads overview`, `leads settings`, `leads exhibitors list`, `leads booths list`, `leads list` | Exhibitor/booth setup, booth `members`, booth `tokens`, `leads exports generate` |
| Reports | `reports types`, `reports list`; `forms reports list`, `registration reports list`, `surveys reports list` | Each area's `generate`, `status`, `download` |
| Plans | `plans current`, `plans list`, `plans usage`, `plans add_ons` | `plans subscribe`, `plans purchase` — explicit purchase approval required |
| Campaign files | `campaigns attachments`, `campaigns preview` | `campaigns attach`, `detach`, `schedule`; confirm sends/schedules |
| Sender health | `accounts email status`, `suppressions list`, `suppressions check` | Suppression `add` / `remove`; do not remove deliverability safeguards without approval |

Table shorthand is relative to its named command area. Run, for example, `micepad app interaction help reset_poll` before supplying arguments.

## Safe workflows and boundaries

- **Shared content first:** speakers, sponsors, schedule and documents can feed multiple surfaces. `site app` only controls the website's Event App promotion block; it does not configure the app. Edit shared sessions under `content schedule`, not `site website_session`.
- **IDs are not interchangeable:** speakers and sponsors use profile IDs; app pages/photos/streams use Content IDs; Lead Capture exhibitors use EventPartner IDs, booths use PartnerBooth IDs. Read the relevant list before writing.
- **Sorting/replacement:** many commands require every item exactly once. Read current scope first, preserve unseen items, and inspect help before sending a full list or JSON replacement. Never sort a paginated subset as the complete set.
- **Reports:** discover supported types/filters, generate once, poll status, download only after completion. Preserve the reported client-storage path. Do not expose signed URLs or export private attendee data into chat.
- **Eventbrite:** account catalog refresh and event sync queue work; a queued response is not completion. Connect uses a browser login/confirmation flow. Never read or print provider credentials.
- **Team access:** event invitation roles are account-wide; removing a pending invitation can cancel it account-wide, and `activate` reactivates membership account-wide. Explain this scope before approval.
- **Lead Capture:** public exhibitor profiles differ from operational partners and physical booths. Token download writes a secret QR credential to client storage; require approval and never print it. Revocation ends active sessions.
- **Live operations:** confirm poll resets, bulk question clearing, deletes, projector/audience changes and public publishing. Resetting a poll deletes votes; clearing questions is permanent.
- **Unsupported features:** `app login_window` reports that the web app disables this feature. Do not claim it can enable login windows.
- **Duplication:** event copies omit orders, payments, attendance and live access tokens. Check copy options and the resulting draft rather than promising a full clone.
