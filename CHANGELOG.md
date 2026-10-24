# Changelog

## 2.0.0 - 2026-10-24

A full redesign of the card plus fixes for the issues reported on v1.

### Added
- New design: two permanent bottom slots with circular progress rings (percent, name, stage, "N min left"). Dryer is blue, washer is red.
- Smart slot layout: a single running machine takes the left slot, the idle machine shows READY on the right while the other runs, and a small controls icon remains in the corner when both are off.
- Play, pause and stop control rows for every running machine, stacked when both run.
- Paused (amber), finished (green check) and alarm (red `!`) states.
- Browser Mod pop-ups with controls, settings and maintenance entities for each machine, plus a combined pop-up when both are idle.
- Redesigned 400 x 288 artwork and a cleaned-up idle image (`Batman_Laundry_Empty.png`).
- `images/transparent.png`, a full-card transparent overlay used as a tap target.
- Entity-based setup for Samsung / SmartThings style washers and dryers using two entity prefixes that are easy to find and replace.
- New README with install steps, entity-mapping table, other-brand guidance, customization tips, troubleshooting and a state gallery.
- `configuration/input_text.yaml` for the `input_text.laundry_label` helper.
- `docs/legacy-template-sensors.yaml` with modern template sensors that turn a completion timestamp into minutes left.

### Fixed
- Cropped or offset card and double image (issue #1): images are now served from `/local/batman-laundry-card/...` instead of an IP-based URL, no `panel` option is used, and the extra stacked base image is gone.
- Template error on current Home Assistant versions (issue #2): the legacy `platform: template` sensors were replaced by modern `template:` syntax, with a safe fallback when the source sensor is unknown.
- The dryer "finished" art referenced `Batman_Laundry_Done.gif`, which does not exist in the repo. It now uses `Batmn_Laundry_Done.gif`.

### Changed
- The v1 card moved to `legacy/Batman-Laundry-v1-card.yaml`.
- The old `Config YAML.txt` and `Batman Laundry Picture Elements Card YAML.txt` were removed from the repo root, and the stray `images/newfile` was deleted.
- Requirements: `custom:button-card` and Browser Mod (both from HACS).

## 1.0.0 - 2025-04-04

- First release: Batman-themed `picture-elements` card for a washer and dryer.
- Animated GIFs for wash, rinse, spin and done, plus PNGs for empty and clothes.
- Text overlays for job state and minutes remaining, driven by `switch` entities and template sensors.
