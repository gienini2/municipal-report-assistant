# Municipal Report Assistant

AI-assisted drafting of **technical reports for a municipal police service** (Guàrdia
Municipal de Figaró-Montmany). The system turns an officer's field notes into a
structured, properly formatted technical report — infrastructure verifications,
incident technical reports, administrative findings — while keeping **real personal
data out of the AI entirely**.

Built and used in real operational work. This repo is a **methodology + example**
showcase: it contains **no real personal data** (see the privacy section).

## Privacy by design (LOPD / GDPR)

This is the core design decision, not an afterthought.

- **The AI never processes real personal data.** Reports are drafted using
  **placeholder tokens** — `[persona1]`, `[persona2]`, `[vehicle1]`, `[parcela1]`,
  `[adreça]`, `[DNI]`, `[data_naixement]` — instead of real names, plates, IDs or
  addresses.
- **Real identities are substituted manually and locally**, only in the final
  document, by the officer — outside the AI pipeline. So no real personal data is
  ever sent to a model, stored, or logged.
- **Data minimisation in the output too.** Operational metadata that could
  pinpoint private property is deliberately excluded: for example, the sample
  report below records that photo **GPS/EXIF coordinates are kept as internal
  traceability only and are not reproduced** in the report body, by command
  instruction.
- **Roadmap:** the substitution is currently manual. Automating the detection of
  persons/vehicles with NER (e.g. spaCy or Microsoft Presidio) is the natural next
  step — the placeholder convention is already designed for it.

> This is pseudonymisation (reversible only by the officer, locally), combined with
> data minimisation — the practical way to use an LLM on sensitive public-sector
> work without ever exposing citizens' data.

## Grounded in a regulatory knowledge base (no invented law)

The real value of the system is its **curated knowledge base of applicable
regulations**, used as the reference layer to frame **economic and legal
responsibility** for each case: urbanism and administrative-procedure law, traffic
and road regulations, civil-code liability articles, and the municipality's own
ordinances.

The assistant does **not invent legal references**. When a report addresses
liability — a duty of maintenance, concurrent administrative responsibility, the
applicable claim procedure and limitation period — it cites the **real provisions**
(by article and statute), grounded in that corpus. The model drafts and frames the
argument; the norms come from verified references. This grounding is what makes the
output usable in a legal and economic context, and it is the system's
anti-hallucination guarantee.

## How it works

1. **Structured templates** per report type (object, general data, chronology,
   on-site inspection, technical assessment, legal framing, proposed measures,
   conclusions, annexes).
2. The officer provides the facts **with placeholders** for any person, vehicle,
   ID, address or parcel.
3. The assistant drafts the report in formal Catalan, following the template and
   the service's conventions, and separates **directly observed facts** from
   **reported statements** and from **prior documentary sources**.
4. The officer reviews, and only then substitutes the real identities **locally**
   in the final document.

## Technical highlight: photo correlation by metadata

To prove that a point is *recurrently* floodable, photos from different episodes
(months apart) must be tied to the **same physical location**. The system does this
by reading the **EXIF/GPS metadata of each photo** and grouping images by proximity,
associating past-episode photos with the current inspection point — a small forensic
traceability step. Crucially, the geolocation is used **only as internal
traceability**: no coordinate is ever reproduced in the report body (data
minimisation). It's a clean example of using metadata as evidence while still
protecting location data.

## Example

A real verification report, fully free of personal data:

- **[Informe tècnic — Punts inundables (Figaró-Montmany)](exemples/informe-punts-inundables.md)**
  — verification of five recurrently floodable points in the municipality, with a
  photographic annex and a chronology of the on-site inspection. Shows the
  metadata-based photo correlation and the GPS/EXIF data-minimisation note above.
- The original formatted `.docx` is in [`exemples/`](exemples/).

## Note on data

Every example in this repository is free of personal data. Real reports, citizen
data, plates, IDs, addresses and cadastral references are never committed here.

## Author

Built by **Juan Pablo Gienini** — municipal police officer and self-taught
software & AI developer. GitHub: https://github.com/gienini2

## License

MIT
