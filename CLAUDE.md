# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Answer only the question the student asks, from the sources they
  name. Don't investigate beyond it or offer a theory about what
  Release 4.2 changed unless they ask for one.
- Read and write only within this directory.
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

Sources: rook-wiki (About Rook, Glossary, Team directory, Releases, Q3 roadmap,
Product briefs) and Priya's handover (`00-rook/company/notes/handoff-from-priya.docx`,
21 Aug 2026). I am the new PM for Rook Dispatch (started Mon 24 Aug 2026,
per a 28 Aug comment).

### Products
- **Rook Dispatch** (flagship, mine): responder coordination. Availability,
  proximity, who gets pinged about a callout, whether they take it. Used by
  handlers (console, desktop-only) and responders (phone).
- **Rook Supply**: gear provisioning. Requisitions, maintenance, field failure
  reports. Used by handlers and quartermasters. Not mine, but shares the
  Responder Availability Record (Dispatch writes it, Supply reads it).
- Customers are independent responders and their handlers, not employees of
  Rook. Revenue is subscription per active responder. Monthly release train;
  point releases are 4.x.
- **Confidentiality:** cover identities are never stored. Never design anything
  that assumes a cover-to-legal identity mapping (Security Policy 4.1), and
  never try to work out who anyone is.

### People (Dispatch team)
| Name | Role | Notes |
| --- | --- | --- |
| Helen Achebe | Director of Product (my director) | Owns roadmap and commitments. Site Aleph |
| Marcus Oyelaran | Engineering Manager | Runs Dispatch engineering. Site Aleph |
| Wen Li | Staff Engineer | Built the ping-ranking logic; no written doc exists. Berlin |
| Sofia Marino | Product Designer | Console and phone app. Ran the September interviews. Site Aleph |
| Ravi Menon | Data Analyst | Weekly report on responders taking pings. Singapore |
| Nadia Hoffmann | Support Lead | Owns tickets. Hears handler complaints first. Berlin |
| Priya Raghunathan | Previous Dispatch PM | Left 21 Aug 2026; owner of every roadmap item |

**Wiki comment roles → people:** "Engineering manager" = Marcus; "Staff
engineer" = Wen Li; "Product designer" = Sofia; "Support lead" = Nadia;
"Priya" = Priya. Handlers named in wiki rows: Aunt Dot (for Vesper), Mr. Ambrose
(Captain Vantage), Halloran (Sgt. Bulwark; also appears in the Supply briefs), Kip
(Meteor Mite and The Gale).

### Vocabulary
- **Callout**: request for a responder to attend an incident; Dispatch's unit of work.
- **Ping**: a callout offered to one responder's phone. **Taken** = yes;
  **turned down** = no; **missed** = no answer before the ping wait ran out.
  Turned down and missed both move the ping to the next responder, but are
  recorded separately.
- **Ping wait**: how long a ping stays before counting as missed. Set per
  release, same for everyone (90s in 4.1, 60s in 4.2).
- **Acceptance rate**: share of pings taken. Headline metric. **Time-to-accept**:
  median seconds from ping sent to taken.
- **Coverage gap**: incident where no available responder had the required
  capability tags. Distinct from low acceptance (nobody *could* go vs. nobody *would*).
- **Routing priority**: score ranking available responders. Inputs: proximity
  (travel-time estimate), availability, capability match, recent acceptance
  history. Turning down or missing a ping lowers the recent-acceptance part.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather,
  aquatic, crowd-management, de-escalation.
- **Mutual aid**: cross-area cover; unsupported, on the Q4 exploration list.
- Supply: **requisition** (handler request, quartermaster approves), **field
  failure report**, **service interval**.

### Release history (Dispatch)
- **4.0** (7 Apr): new console navigation, responder profile redesign, routing override audit log.
- **4.1** (16 Jun): travel-time proximity, bulk callout, phone push reliability.
- **4.2** (12 Aug): proximity weighted up vs. recent acceptance history; ping wait 90s to 60s;
  console filters persist; three defect fixes. Went out clean (no rollback or pages).

### Where things stand
- **4.2 since release (wiki comments):** from 18 Aug, callout tickets ~3x normal.
  26 Aug split: about two thirds "phone never goes off", one third "gone before I
  could answer"; volume flat, not improving. One handler emailed Nadia directly.
  Marcus asked (14 Aug) whether the ranking change was meant to apply to
  responders who turn jobs down; the config doesn't distinguish. Wen Li was away
  14-24 Aug and hadn't answered. Marcus and Nadia held the 4.2 review until I'd
  had a week; Nadia has a ticket breakdown ready.
- **Priya's view (handover, 19 Aug comment):** mostly seasonal (August is soft),
  should recover in September; don't pull the ranking change apart; don't let it
  become a revert conversation. This is her view, not an established finding.
  No numbers from Ravi had been cited in the comments.
- **Q3 roadmap** (last reviewed 30 Jun, all owned by Priya, statuses likely stale):
  4.2 "Committed": change to who gets pinged, Availability Confidence, ping
  timeout tuning. 4.3 "Committed": requisition approval chains. Q4 "Exploring":
  handler phone app, shared cover between responders. Of the 4.2 items, release
  notes show the ping change and the ping wait cut; Availability Confidence is
  not in them. Priya says a couple of items were squeezed out of 4.2 and I need
  to settle with Helen which are still Q3 commitments. Not yet done.
- **Open to-dos from Priya:** write down how pings are ranked (no doc exists);
  console filter tickets are noise; Helen conversation above.
- **Briefs:** Bulk Callout (Jan, says unowned, but 4.1 shipped bulk callout);
  Routing Override Audit Log (Mar, shipped in 4.0); Requisition Approval Chains
  (Jul, Supply; one ask for a second sign-off over $2,000, then scope creep);
  Handler Phone App (8 Sep, Sofia exploring for Q4, notifications/status/one-tap
  contact, not staffed). Briefs may lag shipped work.
- Today per the system date is 2026-11-09; the wiki's newest dated items are
  from September.

### 4.2 release notes vs. handler interviews (2-5 Sep 2026)
Caveats: interviews were console-redesign research, 4 handlers, no
responders; ping experiences are second-hand. Facts only, no cause assigned.
- **Ranking change** ("long requested by wide-area responders"): no interviewee
  covers a wide area or mentions proximity. Kip (quiet Meteor Mite vs. busy The
  Gale, same week and city) and Aunt Dot (quiet stretches) describe a
  quiet-vs-busy split the note doesn't mention.
- **Ping wait 90s to 60s:** consistent with Aunt Dot (Vesper loses the "race,
  phone to stairs"), Ambrose (Vantage "one boot on"; slower replies used to
  still win) and Halloran (Bulwark lost a callout in August). The note lists
  only the benefit.
- **Filters persist:** valued by Ambrose; he reports two resets after updates
  and wants a warning. Not in the note. Kip and Halloran don't use filters much.
- **Three defect fixes** (duplicate push, tag ordering, export time zone): no
  feedback either way.
- **Raised by handlers, not in the note:** quiet responders; late awareness of
  a live callout (Aunt Dot wants to be told too, Ambrose wants it harder to
  miss); per-responder alert sounds (Kip); larger text (Aunt Dot, Ambrose);
  dark mode (Kip).
- **Correction:** Halloran is a handler (Sgt. Bulwark) who also runs the gear cage.

Findings from the Module 1 session (facts only; no cause assigned yet):
- Routing code (`00-rook/code/dispatch-routing`): 4.2 moved proximity weight 0.45 to 0.60 and recent-acceptance 0.40 to 0.25. A decline costs 0.12, an accept earns 0.08. Missed and turned down are scored the same, and the score never decays (`history.py` has an unresolved TODO from 2019).
- Priya's "mostly seasonal" has no numbers behind it; the wiki has no acceptance-rate data and nothing after 8 Sep. Ravi's weekly report is the missing source.
- Open questions: Marcus's 14 Aug question (does the ranking change apply to responders who turn jobs down?) is unanswered; Availability Confidence is committed to 4.2 but unshipped and undocumented; there is no 4.3 row in Releases; "monthly" releases are really 8 to 10 weeks apart.
- `05-super-speed/director-request.txt`: Helen asks for a one-pager and clickable prototype of what we'd build for quiet responders, and refers to a routing-code finding that isn't written up anywhere.
- Briefs vs. evidence: the Requisition brief adds an approval step, but Halloran's interview complains approval is already too slow. Roadmap and brief disagree on who owns the Handler Phone App.
