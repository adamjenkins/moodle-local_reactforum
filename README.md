# ReactForum

ReactForum is a Moodle local plugin that adds configurable reaction buttons to
the standard **Forum** activity. It lets students respond to discussion posts
with a quick click — similar to "like" or emoji reactions on social media —
instead of having to write a full reply. Reactions are recorded per user and
post, and shown as aggregate counts so teachers and students get immediate
feedback on engagement.

## Features

- **Configurable reaction buttons** — teachers define any number of buttons,
  each rendered as either plain text (e.g. "Got it!", "+1") or an uploaded
  image/icon.
- **Per-forum or per-discussion configuration** — reaction settings can be
  set at the forum level, or delegated so each discussion owner chooses their
  own reaction buttons for their topic.
- **Aggregate counts** — each button shows how many users have reacted, so
  engagement is visible at a glance.
- **Optional reactions on replies** — buttons can appear only on the
  discussion's first post, or on every reply as well.
- **Changeable or single-shot reactions** — optionally allow users to switch
  or remove their reaction, or restrict them to one immutable reaction per
  post.
- **Delayed counter visibility** — optionally hide the count from students
  until they react themselves, to avoid bandwagon effects.
- **Privacy API support** — reaction data is included in Moodle's data
  export/deletion (GDPR) tools.
- **Backup and restore** support for reaction settings and reaction buttons
  as part of course backups.

## How it works

Reaction settings and buttons can be attached either to a forum (applying to
every discussion in it) or, when the forum-level setting is set to
**"Decided by discussion owner"**, to an individual discussion. A teacher (or
discussion owner, where permitted) opens **Reaction settings** for a forum or
discussion to:

- Choose the reaction button type: **None**, **Text**, **Image**, or (forum
  level only) **Decided by discussion owner**.
- Add, edit, reorder, or delete individual reaction buttons.
- Toggle whether reactions apply to replies, whether users can change their
  reaction, and whether counters are delayed.

Students then see the configured buttons under each eligible post and can
click one to react. Reacting, changing a reaction, and fetching the current
reaction state for a discussion are all handled via AJAX web services, so the
forum page updates without a full reload.

## Requirements

- Moodle 4.5+ / Moodle 5.x (`$plugin->requires = 2024042200`).
- The core **Forum** activity (`mod_forum`).

## Installation

1. Copy (or check out) this plugin into your Moodle codebase as
   `local/reactforum`:

   ```bash
   rsync -a --delete moodle-local_reactforum/ /path/to/moodle/local/reactforum/
   ```

2. Visit **Site administration > Notifications** (or run
   `php admin/cli/upgrade.php`) to complete the installation, which creates
   the plugin's database tables.

## Usage

1. Open a forum, then go to **Reaction settings** (via the forum's action
   menu) to configure reaction buttons for the whole forum, or to delegate
   configuration to discussion owners.
2. If delegated, open an individual discussion and use its **Reaction
   settings** link to configure that discussion's buttons.
3. Save the settings — reaction buttons will appear immediately under the
   relevant posts for anyone with access to the forum.

## Permissions

| Capability                        | Description                                  | Default roles                          |
|------------------------------------|-----------------------------------------------|-----------------------------------------|
| `local/reactforum:forumconfig`     | Configure reactions of a forum module         | Teacher, Non-editing teacher, Manager   |

Discussion-level configuration is permitted to whoever can edit that
discussion (e.g. its owner), independent of `local/reactforum:forumconfig`.

## Database tables

- `local_reactforum_settings` — reaction type and options per forum or
  discussion.
- `local_reactforum_reactions` — the reaction buttons defined for a forum or
  discussion.
- `local_reactforum_userreactions` — which reaction each user applied to
  each post.

## Web services

The plugin exposes the following AJAX-enabled external functions, used by
its own JavaScript:

- `local_reactforum_get_discussion_reactions` — fetch reaction settings,
  buttons, and per-post state for a discussion.
- `local_reactforum_react` — toggle a reaction on a forum post.
- `local_reactforum_upload_reaction_image` — move a draft image file into the
  plugin's storage and return its file id, for use as a reaction button
  image.

## Privacy

This plugin implements Moodle's Privacy API. It stores which reaction each
user applied to which post (`local_reactforum_userreactions`) and includes
this data in user data exports and deletion requests.

## License

GNU GPL v3 or later. See https://www.gnu.org/copyleft/gpl.html
