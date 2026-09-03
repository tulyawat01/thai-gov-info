# AGENTS.md

## What this repo is

Research corpus, not a software project. No build, test, or lint exists.
Each directory maps one Thai ministry to its agencies as Markdown files.
The corpus supports mapping CKAN organizations on data.go.th to ministries.

## Layout and naming

- One directory per ministry: 18 `กระทรวง*` dirs plus `สำนักนายกรัฐมนตรี`. Names are Thai.
- `00_overview.md` describes the ministry. `NN_<agency>.md` describes one agency.
- Give a new agency file the next free number. Do not renumber existing files.
- Quote Thai paths in shell commands. Spaces become underscores in long ministry names, for example `กระทรวงการอุดมศึกษา_วิทยาศาสตร์_วิจัยและนวัตกรรม`.

## Markdown template (follow it exactly)

Every file ends with `## Keywords สำหรับ AI`. It holds Thai and English search terms. All 232 files have it.

Ministry overview (`00_overview.md`):
- Header: `**ชื่อย่อ:**`, `**เว็บไซต์:**`, `**สายด่วน:**`
- Sections in order: `## ภารกิจหลัก`, `## อำนาจหน้าที่`, `## หน่วยงานในสังกัด` (table: หน่วยงาน | ชื่อย่อ | ประเภท), `## กฎหมายหลัก`, `## Keywords สำหรับ AI`

Agency file (`NN_*.md`):
- Header: `**ชื่อย่อ:**`, `**สังกัด:**`, `**ประเภท:**`, `**เว็บไซต์:**`
- Sections in order: `## ภารกิจหลัก`, `## อำนาจหน้าที่`, `## กลุ่มเป้าหมาย / ผู้รับบริการ`, `## กฎหมายหลักที่เกี่ยวข้อง`, `## Keywords สำหรับ AI`

Keep the overview's `หน่วยงานในสังกัด` table in sync when you add or rename an agency file. The table may list units that have no file.

## Local data files (gitignored)

These stay out of git. They may be missing after a fresh clone.

- `organizations_YYYYMMDD.json`: raw CKAN organization dump from data.go.th. The filename carries the dump date. It holds ~1,537 records. Match agencies by Thai `title`; `name` is the portal slug; `package_count` shows dataset volume. Regenerate it from the data.go.th CKAN organization API when needed.
- `thai_gov_ministry_responsibility.docx`: source reference for ministry missions. Kept locally only.

## Git

Work on `dev` and push to `origin/dev` (remote: github.com/tulyawat01/thai-gov-info). `main` currently points at the same commit.

## Verify changes

- Template check: `grep -h "^## " */[0-9]*.md | sort | uniq -c`. Counts of `ภารกิจหลัก`, `อำนาจหน้าที่`, and `Keywords สำหรับ AI` must equal the Markdown file count (232 today).
- Content file count: `find . -mindepth 2 -name "*.md" | wc -l` (232 today; excludes this AGENTS.md)
