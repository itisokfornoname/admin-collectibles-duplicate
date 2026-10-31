# ballsdex_duplicates

Adds **`/admin balls duplicates`** (and the text command `b.admin balls duplicates`) to BallsDex V3.

* Finds players who own several copies of the same collectible.
* Optional `user` argument to look at one person only.
* Shows the totals of collectibles and specials in the bot, with the amount owned of each.
* Admin only (Django permissions, same system as the other `/admin` commands).
* Every criterion is editable in the **admin panel** under *Duplicate checker > Duplicate checker settings*.

## Install (development, see the BallsDex wiki "Writing custom packages")

1. Copy this folder to `extra/ballsdex_duplicates` inside your BallsDex folder.
2. Create `config/extra.toml` (or add to it):

   ```toml
   [[ballsdex.packages]]
   location = "/code/extra/ballsdex_duplicates"   # Docker. Use "" without Docker.
   path = "duplicates_app"
   enabled = true
   editable = true
   ```
3. Install it.
   * Docker: `docker compose build`
   * No Docker: `uv pip install -e extra/ballsdex_duplicates`
4. Create the database migration (needed once):
   * Docker: `docker compose run --rm migration python3 -m django makemigrations duplicates_app`
   * No Docker: `python3 -m django makemigrations duplicates_app`
5. Start the bot, then run **`b.admin syncslash`** in your server so Discord learns about the new slash subcommand.

You should see `duplicates_ext` in the "Packages loaded" log line.

## Who can use it?

A Django user linked to the Discord account, with **Is staff** ticked and the permission set in the settings
(default `bd_models.view_ballinstance`). Leave the permission empty in the settings to allow any staff member.

## Settings (admin panel)

| Setting | What it does |
| --- | --- |
| Enabled | Turns the command off/on |
| Required permission | Django permission needed |
| Min count | Copies needed to count as a duplicate (2 or more) |
| Separate by special | Shiny X and normal X count separately (or together) |
| Ignore favorites / Only tradeable | Filter which copies are counted |
| Excluded collectibles / specials | Never reported |
| Rows per page / Max rows / Ephemeral | Display options |

## Pages in the result

1. Overview (bot totals, user totals, criteria, result count)
2. Duplicates (who owns how many)
3. Specials with amounts
4. Collectibles with amounts
