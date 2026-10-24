# Legacy v1 files

These are the original v1 files, kept for reference only. They are not maintained.

- `Batman-Laundry-v1-card.yaml` is the original card (formerly `Batman Laundry Picture Elements Card YAML.txt`).

How v1 works, in short: it uses one `switch` entity per machine (`switch.washer`, `switch.dryer`) plus template "job state" and "completion time in minutes" sensors, and it swaps animated images based on those states.

Known problems with v1 (all fixed in v2, see the main [README](../README.md)):

- Image paths started with an IP address placeholder, which breaks outside the home network.
- It stacked an extra empty-bench image element over the base image, which could show as a double image in some views.
- It relied on legacy `platform: template` sensors that current Home Assistant versions reject.

For a new setup, use the v2 card in [`card/`](../card/).
