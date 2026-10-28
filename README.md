# FIS 2021 REDCap charts

## Purpose

This public auxiliary repository hosts anonymized interactive Gantt charts
displayed in the FIS 2021 REDCap project. It contains published chart outputs
and their supporting assets. Data-management and research source material is
maintained separately in the private
[Data_Management repository](https://github.com/CansecoRibas/Data_Management).

## Open the charts

The [chart index](https://cansecoribas.github.io/FIS-2021-Charts/) lists the
available charts. Each individual chart uses this URL pattern:

```text
https://cansecoribas.github.io/FIS-2021-Charts/Gantt_Paciente_<PATIENT_ID>.html
```

For example, the chart for `FIS_FA_1` is:

[Gantt_Paciente_FIS_FA_1.html](https://cansecoribas.github.io/FIS-2021-Charts/Gantt_Paciente_FIS_FA_1.html)

Use the published `github.io` URL for embedding. A GitHub repository file URL
such as `github.com/.../blob/main/...` opens the source-file interface instead
of the published chart.

## Embed an individual chart in REDCap

Use the following HTML in the existing REDCap descriptive field configured
for the chart:

```html
<iframe
  src="https://cansecoribas.github.io/FIS-2021-Charts/Gantt_Paciente_[ant_id].html"
  width="100%"
  height="900"
  style="border:0;">
</iframe>
```

REDCap pipes the current record's `ant_id` into the URL. For `ant_id = FIS_FA_1`,
the iframe therefore loads `Gantt_Paciente_FIS_FA_1.html`. This embedding has
been confirmed in the FIS 2021 project. If another project uses a different
identifier field, replace `[ant_id]` with that field's piping expression.

The identifier must match the filename exactly, including underscores,
capitalization and leading zeros. Existing filename examples include
`FIS_FA_1`, `FIS_SOC_10` and `LEG01`; do not add an extra `FIS_` prefix to an
identifier that already contains it. Use an individual chart URL for a
record-specific display; embedding the index shows the list of all charts.

## Add or update charts

1. Export the approved anonymized chart HTML from the chart-generation workflow.
2. Place `Gantt_Paciente_<PATIENT_ID>.html` at the repository root. Include its
   matching `Gantt_Paciente_<PATIENT_ID>_files/` folder when the export uses
   external companion assets. Preserve the exported relative paths.
3. Keep the filename unchanged when replacing an existing chart so its REDCap
   URL continues to work. Update `index.html` when adding or removing charts.
4. Commit and push the selected chart files to `main`.
5. Wait for [Deploy to GitHub Pages](https://github.com/CansecoRibas/FIS-2021-Charts/actions/workflows/deploy.yml) to
   succeed, then open the individual published URL and check the REDCap field.

The repository is public. Publish only anonymized chart outputs intended for
public access; patient source extracts and credentials do not belong here.

## Deployment scope

The [deployment workflow](.github/workflows/deploy.yml) runs on pushes to `main`
and can also be started manually from GitHub Actions. It prepares a separate
site folder containing only:

- `index.html`;
- root-level `Gantt_Paciente_*.html` pages;
- `Gantt_Paciente_*_files/` companion asset folders and their contents.

That folder is uploaded to GitHub Pages. The workflow does not publish the
entire checkout: this README and unrelated root files are excluded from the
site artifact. Files committed to this public GitHub repository are still
publicly accessible through GitHub itself.

Deployment stops if the index or all chart pages are missing, or if selected
pages/assets contain symbolic links. The preparation step preserves chart
filenames and relative directory structure.

## Troubleshooting

| Symptom | Check |
| --- | --- |
| Individual chart returns 404 | Confirm the exact filename exists on `main`, the identifier matches it, and the latest deployment succeeded. |
| Every REDCap record shows the same chart | Check that the iframe uses `[ant_id]` rather than a fixed identifier. |
| Chart works directly but not in REDCap | Inspect the configured field's HTML and confirm the identifier is being piped. The example above is the working FIS 2021 configuration. |
| Interactive controls or styling are missing | Check whether the export requires a companion `_files/` folder and whether its relative paths were preserved. |
| An updated chart still looks unchanged | Confirm the deployment finished, then reload the individual chart without the browser cache. |
