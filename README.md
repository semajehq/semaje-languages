# semaje-languages

One canonical code per language, ISO 639-3 + ISO 15924 script (`swh_Latn`,
`amh_Ethi`, `kik_Latn`), with every alias people actually use resolving to it:
`sw`, `swa`, `swh`, `sw-KE`, `Swahili`, `Kiswahili`. STT, TTS, the API, the web
app, and the extension all resolve through this module, so they always agree.

- `languages.json`: 1,693 languages, generated. Each row has `whisper` (code
  if Whisper supports it), `omnilingual` (bool), `mmsTts`, `piper` voices, `aliases`.
- `tts-voices.json`: the Piper voice catalog with per-voice dataset licenses (`piper_catalog()`).
- `__init__.py`: `registry().resolve(value)`; handles BCP-47 region/script
  subtags (`zh-TW` → `cmn_Hant`), macrolanguages (`swa` → `swh`), legacy codes
  (`iw` → Hebrew), and Omnilingual dialect variants (`ell_Grek_cypr1249`).

## Install

```bash
pip install "semaje-languages @ https://github.com/semajehq/semaje-languages/archive/refs/tags/v1.0.0.tar.gz"
```

```python
from semaje_languages import registry
registry().resolve("Kiswahili").code     # "swh_Latn"
registry().resolve("zh-TW").code         # "cmn_Hant"
```

Used by [semaje-stt](https://github.com/semajehq/semaje-stt) and
[semaje-tts](https://github.com/semajehq/semaje-tts). Part of
[semaje](https://github.com/semajehq/semaje).

## Regenerate

From the upstream sources (Whisper, Omnilingual ASR, MMS-TTS, Piper):

```bash
pip install -e ".[build]"      # needs libsndfile for omnilingual-asr
python -m semaje_languages.build
```

Bump `version` in `pyproject.toml` and `__version__`, tag `vX.Y.Z`, and update
the pin in the consuming services.

Never hand-edit the JSON; change `build.py` and regenerate. Gate tests in
`tests/` pin the contract (unique codes, all 100 Whisper codes mapped, a
low-resource coverage floor).
