# Attribution

The ideas in this skill are not original. They are assembled from formats that already work, and each
one contributed something specific:

- **mattpocock/skills — `handoff`** (MIT, https://github.com/mattpocock/skills)
  The baseline shape: a small Markdown document, suggested skills listed for the next agent, no
  duplication of artifacts that already exist elsewhere, redact sensitive values. The "keep it to one
  file and do not over-engineer it" stance comes from here.
- **Jordanwei1/jiaojie-skill** (MIT, https://github.com/Jordanwei1/jiaojie-skill)
  The parts that make a handoff actually resumable: rejected and failed paths as first-class content,
  the exact stop point, completion criteria for the next action, staleness/revalidation needs, and the
  rule that a handoff transfers no authority. The multi-format (md/zip/audit) approach was deliberately
  left out.
- **wilbeibi/catchup** (MIT, https://github.com/wilbeibi/catchup)
  Read the real session record rather than the model's recollection of it. CatchUp does this with a CLI
  over each harness's local transcripts; here it is expressed as a rule, so the skill stays
  dependency-free.
- **obra/superpowers** (MIT, https://github.com/obra/superpowers)
  The habit of writing skills that declare their own failure modes, require evidence before a success
  claim, and state what they will not do.

If you maintain one of these projects and want the wording changed, open an issue — attribution is
meant to be accurate, not decorative.
