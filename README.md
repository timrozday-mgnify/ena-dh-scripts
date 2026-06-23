# ena-dh-scripts

ENA sample/study submission scripts: build a manifest from sample/study
records, validate it against ENA's XSDs, and submit it via the Webin REST
API v2.

Used by [mimicc-ena-submission-assistant](https://github.com/timrozday-mgnify/mimicc-ena-submission-assistant)
(`server/ena_service.py`), pinned as a git dependency in its
`requirements.txt`.

This is a slimmed extraction of the submission-scripts surface from
[ena-submission-dataharmonizer](https://github.com/timrozday-mgnify/ena-submission-dataharmonizer)
— that repo also contains LinkML schema-authoring tooling
(`dh_schema.py`, `edit_linkml.py`), a webin-cli read-submission CLI
(`submit_reads.py`, `ena_cli.py`), and the MIMICC schema/planning material,
none of which is needed at runtime by consumers of these scripts. Those
stay in the larger repo; this one is just the four modules below plus their
tests.

## Layout

- `ena_common.py` — shared utilities: credentials, LinkML/XSD validation,
  XML serialisation, CSV/JSON/TSV loaders, duplicate detection, sample
  attribute unit normalisation.
- `submit_sample.py` / `submit_study.py` — build an ENA XML manifest from
  sample/study records, validate it against the bundled ENA/SRA XSDs, and
  submit it via `ena_api.WebinClient`. Usable as a library
  (`build_manifest`, `validate_manifest`, `submit_batch`/`submit_studies`)
  or as a `typer` CLI.
- `prepare_dh_output.py` — renames a DataHarmonizer JSON export's keys from
  slot titles to LinkML `annotations.id` values (`prepare_data()`).
- `assets/ena_schema/*.xsd` — ENA/SRA XSDs, used only as test fixtures here
  (callers pass their own `xsd_dir` at runtime — see
  mimicc-ena-submission-assistant's own committed copy of these files for
  its runtime use).
- `tests/` — pytest suite covering the four modules above.

## Install

```bash
pip install "ena-dh-scripts @ git+https://github.com/timrozday-mgnify/ena-dh-scripts.git@v0.1.0"
```

## Develop

```bash
python -m venv .venv && . .venv/bin/activate
pip install -e .
pip install pytest
pytest
```
