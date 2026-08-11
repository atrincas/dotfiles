# Worked example

A fictional case, built to hold every trap at once. The description is correct,
but a reader outside the team cannot use it.

## Input

Title: `Feat/metro-b timetable import harmonised`

Statistics: 10 files, +326,244 and −53,461 lines.

The totals alone are not sufficient. The per-file list gives the real shape:

```
324891  53402  modified  server/feeds/metro-b/stop-times.generated.json
   412      0  added     server/feeds/normalise-metro-b.ts
   288      0  added     server/feeds/gtfs-field-map.ts
   241      0  added     server/feeds/calendar-expand.ts
   163      0  added     server/feeds/tram-c-field-map.ts
    94      0  added     server/feeds/extract-tram-c.ts
    61     44  modified  server/feeds/import.ts
    47      0  added     server/feeds/tram-c-config.ts
    38     12  modified  server/sql/route-headways.sql
     9      3  modified  shared/time/timezone.map.ts
```

Three facts appear only in this list. One generated file contains 99 percent of
the lines. The PR adds Tram C files. The PR changes `import.ts`, and Metro A
uses that file.

The description, made shorter. Note the bullet list with bold labels, the file
names as subjects, and the assumption that the reader knows the domain:

> This PR completes the Metro B timetable integration, wiring the GTFS static
> feed all the way through to `stop-times.generated.json` and the departures
> table. It replaces the earlier direct-to-import approach with a **normalise
> step** modelled on the reference feed validator, emitting a flat 3-file schema
> before handing off to the existing `import.ts` pipeline.
>
> - **`normalise-metro-b.ts`**: new normalise step that expands compressed
>   `frequencies.txt` headways into explicit stop times. Handles `stop_id`
>   prefix stripping, unscheduled trips mapped to `service_id: -1`, per-trip
>   timezone assignment via `TZ_MAP`, deduplication of trips repeated across
>   calendar exceptions.
> - **`gtfs-field-map.ts`**: TypeScript port of the reference field mapping (214
>   GTFS column → internal field entries) and tolerant `resolveField()`
>   matching.
> - **`import.ts`** + **`route-headways.sql`**: extended with 11 new Metro B
>   route mappings; two renumbered routes are intentionally left commented out
>   pending confirmation from the agency.
> - **Generic feed modules**: shared logic factored out so Metro A, Metro B, and
>   Tram C all use the same core with per-feed config.
>
> Metro A data is unaffected — Metro B rows are isolated by
> `agency_id: 'metro-b'`.

## Output

**What this is about**

The app shows departure times to travellers. Transit agencies supply the
timetables. The agencies use GTFS, which is a standard set of text files for
timetable data. The app reads these files, changes their shape, and then puts
the result into a database.

**What problem this solves**

The team must add the timetable for a second line. The team calls this line
"Metro B". The agency supplies the data in a shape that the app cannot use. An
earlier attempt changed the raw files into app data in one step. That attempt
did not work well. This PR deletes it and uses two smaller steps.

**The main idea**

The PR adds a step in the middle. The name of the step is "normalise".

```
before:  GTFS files  →  import  →  database
now:     GTFS files  →  normalise  →  import  →  database
```

The normalise step follows the rules of the official feed validator. The team
already trusts those rules. The step makes three flat files. The import step
then does its usual work. The PR does not change the import step.

**What is in it**

The normalise step also expands the timetable. Some agencies do not list every
departure. Instead they give a headway, which is the number of minutes between
two departures. The step calculates each departure time from the headway. Thus
the database holds one row for each real departure.

The step does three more tasks. It gives unscheduled trips a placeholder. These
trips have no service day, which is the calendar day that a timetable applies
to. It gives each trip a timezone, which it calculates from the agency code. It
deletes trips that occur in more than one calendar exception.

The PR adds a table of 214 GTFS column names and their internal field names.
The table finds a match when a column name is almost, but not fully, the same.
Metro A, Metro B, and Tram C now use one set of shared code, with a
configuration file for each line.

The PR connects 11 new routes to their headway rules. The agency renumbered two
routes. The team does not know if these are the same routes or new routes. Thus
the code for these two routes stays commented out.

**What it does not change**

The author says that the Metro A data is safe. Each Metro B row has the value
`agency_id = 'metro-b'`. Metro B also uses different route IDs. But the PR does
change `import.ts`, and Metro A uses that file too.

**What to check in review**

One file contains almost all of the lines. `stop-times.generated.json` adds
324,891 lines and deletes 53,402 lines. The build makes this file, and it is not
code. The rest of the PR is approximately 1,400 lines of code.

The description does not mention Tram C. The PR adds three Tram C files. Ask the
author if this work belongs in a separate PR.

The safety claim needs a check. The PR changes `import.ts` and
`route-headways.sql`, and Metro A uses both files. The value
`agency_id = 'metro-b'` keeps the Metro B rows apart, but it does not protect
the shared code. Run the import step for Metro A before and after the change,
then compare the results.

## Why this output is correct

- It explains GTFS, normalise, headway, and service day at the first use.
- Each sentence has 25 words or less. Each paragraph has 6 sentences or less.
- All tenses are simple. There is no `-ing` verb and no perfect tense.
- The safety claim belongs to the author, not to you.
- The open decision about the two renumbered routes stays visible.
- The review section reports facts from the per-file list, not a guess from the
  totals.
- It finds the second feature, Tram C, that the title and the description do not
  name.
