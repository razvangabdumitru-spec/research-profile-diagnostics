# Diagnostics: memory/memory.json

This diagnostics.md lists missing fields, duplicates, conflicting dates/titles, and inconsistent naming found in memory/memory.json. Each issue includes the JSON path(s) where it was observed and minimal questions to resolve it.

1) Inconsistent institution naming for PhD affiliation
- Issue: "HKUST NLP Group" vs "Hong Kong University of Science and Technology" vs "HKUST" — the group and the university are both referenced; memory sometimes uses the group name and other times the full university name.
- JSON paths:
  - entities[0].observations[6] -> "Ph.D. in Computer Science (2024-Present) at Hong Kong University of Science and Technology"
  - entities[1].observations[0] -> "Professor at HKUST NLP Group"
  - entities[2].observations[0] -> "Research group at Hong Kong University of Science and Technology"
  - entities[9].observations[0] -> "University where Junteng Liu is pursuing Ph.D. in Computer Science (2024-Present)"
- Minimal question: Do you want the profile to list your affiliation as "HKUST NLP Group" (group-level) or "Hong Kong University of Science and Technology (HKUST)" (institution-level)? Please confirm preferred canonical name.

2) Duplicate/overlapping education dates and graduation month
- Issue: B.Eng. listed as (2020-2024) and separate observation says "Graduated June 2024" — not a conflict but slightly redundant; verify exact graduation month if necessary.
- JSON paths:
  - entities[0].observations[7] -> "B.Eng. (2020-2024) at Shanghai Jiao Tong University"
  - entities[3].observations[1] -> "Junteng Liu graduated in June 2024"
- Minimal question: Can you confirm the B.Eng. graduation month/year? Is June 2024 correct?

3) Internship date ranges overlap with degree timelines
- Issue: "Research Intern, Tencent WXG (June 2024 - September 2024)" overlaps with B.Eng. end date (2024). Possibly a gap/overlap — could be correct (e.g., internship after graduation), but dates should be clarified.
- JSON paths:
  - entities[0].observations[8] -> "Research Intern at Tencent WXG (June 2024 - September 2024)"
  - entities[0].observations[7] -> "B.Eng. (2020-2024) at Shanghai Jiao Tong University"
- Minimal question: Did the Tencent WXG internship begin after you graduated (i.e., was graduation in June 2024 before that internship), or did you intern while finishing your degree? Please confirm timeline.

4) Multiple contact/name formats
- Issue: GitHub username is recorded as "Vicent0205" but the GitHub contact entity uses the label "GitHub: Vicent0205"; similarly Google Scholar is labeled generically. Not a conflict, but inconsistent naming of contact entities.
- JSON paths:
  - entities[0].observations[19] -> "GitHub: Vicent0205 (https://github.com/Vicent0205)"
  - entities[24].observations -> ["GitHub profile of Junteng Liu","Username: Vicent0205","URL: https://github.com/Vicent0205"]
  - entities[26].observations -> ["URL: https://scholar.google.com/citations?hl=en&user=tbK9jl4AAAAJ&view_op=list_works&sortby=pubdate"]
- Minimal question: Preferred canonical labels for contact links? e.g., GitHub: Vicent0205, Google Scholar: URL, Email: jliugi@connect.ust.hk. Also confirm capitalization of GitHub username (Vicent0205) if that's the canonical username.

5) Publication venues vs arXiv notes
- Issue: Some publications are listed as "published on Arxiv" while others list conference venues. Confirm which should be displayed as "arXiv" vs "conference".
- JSON paths:
  - entities[16].observations[2] -> "Published on Arxiv" (SynLogic)
  - entities[17].observations[2] -> "Published on Arxiv" (Perception Bottleneck)
  - entities[18].observations[2] -> "Published at EMNLP 2024" (Truthfulness Hyperplane)
  - entities[19].observations[1] -> "Published at ICML 2024"
- Minimal question: For each paper, do you want the venue field to show "arXiv" when the paper is only on arXiv, and conference name when accepted? Confirm current status for SynLogic and Perception Bottleneck.

6) Possible duplicate author name forms
- Issue: "Prof. Yu Cheng" vs "Yu Cheng" — two forms for the same person in memory; similarly "Professor Junxian He" vs "Junxian He".
- JSON paths:
  - entities[11].observations[0] -> "Advisor to Junteng Liu during his internship at Shanghai AI Lab" (Prof. Yu Cheng)
  - entities[22].observations[3] -> "Co-authors include ... Yu Cheng" (in On the Universal Truthfulness Hyperplane Inside LLMs)
  - entities[1].observations -> ["Professor at HKUST NLP Group","PhD supervisor of Junteng Liu","Previously advised Junteng Liu during undergraduate studies at SJTU"]
  - entities[...] other mentions of Junxian He across publications
- Minimal question: Preferred name formats for co-authors/advisors (e.g., include titles like Prof. or use plain names)? Confirm canonical forms for Yu Cheng and Junxian He.

7) Missing fields: ORCID, personal website, location
- Issue: Memory contains email, GitHub, Google Scholar, X, but no ORCID, personal website URL, or location/address.
- JSON paths: absence (no entities found for ORCID, website, or location)
- Minimal question: Do you have an ORCID, personal website, or preferred location to include? If yes, please provide.

8) Duplicate/conflicting publication years or ordering
- Issue: Publication years are present (2023, 2024, 2025). No direct conflicts, but verify 2025 arXiv entries are intended to be listed as 2025.
- JSON paths:
  - entities[16].observations[0] -> "2025 publication"
  - entities[17].observations[0] -> "2025 publication"
- Minimal question: Confirm publication years for SynLogic and Perception Bottleneck (are they 2025?).

9) Contact email entity duplication
- Issue: Email appears both as observation in Person entity and as separate Contact Info entity (name equals the email). Slight duplication.
- JSON paths:
  - entities[0].observations[18] -> "Email: jliugi@connect.ust.hk"
  - entities[25].observations[0] -> "Email address of Junteng Liu"
- Minimal question: No action needed, but confirm if you prefer email only in contact section or also listed in personal summary.

10) Missing contribution roles for co-authors
- Issue: For multi-author papers, roles (first author, co-author) are provided inconsistently. Some list "First author: Junteng Liu", others list authors without role.
- JSON paths:
  - entities[16].observations[1] -> "First author: Junteng Liu"
  - entities[19].observations[1] -> "Published at ICML 2024" (authors listed without role)
- Minimal question: For publications where roles are missing, would you like me to assume co-author unless noted first author? Confirm any additional first-author papers.

If you want, I can open a PR with corrected canonical naming and explicit fields after you confirm the answers to the minimal questions above. All diagnostics are based strictly on the content of memory/memory.json; no new facts were added.
