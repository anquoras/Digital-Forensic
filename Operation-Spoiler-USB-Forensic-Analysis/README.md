# Operation Spoiler: Expert Witness Statement (COMP6254)

Markdown version of my COMP6254 Digital Forensics coursework, the expert witness statement for **Operation Spoiler** (Case CPD-2024-04-023). It is converted from `COMP6254CW_Expert Witness Statement _qal1g25_ver2 .pdf`.

- **Author:** Quoc Anh Luu (qal1g25), MSc Cybersecurity, University of Southampton
- **Module:** COMP6254 Digital Forensics, 2025/26
- **Exhibit examined:** SM/2024/04/023/001, a red novelty Lego-shaped USB stick (forensic image `SM_2024_04_023_001.dd`, SHA256 `B4F6D02C...B64B294C`)

## Folder Structure

```
Operation-Spoiler-USB-Forensic-Analysis/
├── README.md              This file (overview and lessons learned)
├── report.md              Full expert witness statement in Markdown
└── Data_Image/            Figures used in the report
    ├── A01 to A30         Appendix A: HxD screenshots, carving steps, carved images
    ├── B01 to B18         Appendix B: OOXML exhibits (core.xml, app.xml, document.xml, media)
    ├── C01                Appendix C: Evidence Timeline Diagram
    ├── D01                Appendix D: Evidence Relationship Diagram
    └── source_artefacts/  Original carved JPEGs and the FTK Imager acquisition log
```

The figure prefixes match the appendix they belong to. For example, `B05_document_xml_hidden_signature.png` is Exhibit B-5 in Appendix B.

## Case Summary

More than 500 books in Cyberly bookshops had pages removed in the same way. Dr Taylor Perl Salt was arrested on 23 April 2024, and a USB stick hidden inside a Lego bus model was seized from his home. I was given the disk image and asked to answer four points to prove: who used the USB stick, what is known about the user, who Dr Salt is and how he links to the exhibit, and why the books were damaged.

## Method

All recovery was done by hand in **HxD**, with no automated carving tools.

1. **Cluster size (Task 1a).** I searched for file signatures (`FF D8 FF` for JPEG, `50 4B 03 04` for OOXML) and kept only hits that sat at the start of a 16-byte row, had `00`/`FF` slack before them, and had a valid structure after them. Dividing the surviving offsets by candidate cluster sizes, then checking against the real device size (3880 MB) and Microsoft's FAT32 defaults, gave **4096 bytes**.
2. **JPEG carving (Task 1b).** Two independent JPEGs were carved from header to `FF D9` footer. One showed lions and was unrelated. The other, `carved_image_2.jpeg` (offset 3269828608, 790,339 bytes), showed library and bookshop interiors.
3. **OOXML carving (Task 1c).** Two OOXML candidates were checked by finding their End of Central Directory record (`50 4B 05 06`). One was a decoy holding six telephone sound files. The real document ran from offset 5160960 to 210440344 (about 180 MB).
4. **Document analysis (Task 2).** The `.docx` was renamed to `.zip` and its XML read in XML Marker. This revealed the metadata, white-on-white hidden text (`FEFFFF`), files with fake extensions, and the "operation penguinontheloose" plan image.
5. **External sources (Task 3).** News articles, the police interview and press releases were cross-referenced with the document findings.

## Key Findings

- `core.xml` records **Taylor Salt** as last modifier, at 13:19 UTC on the day of arrest. Hidden white text spells "Dr Taylor Salt Was Here".
- The hidden plan was to find 42 libraries, pick 42 books in empty corridors, and remove **page 42** from each one.
- News Article 3 explains the motive: a fiction contest worth **10 million Cyberly Pounds** that requires exactly 42 pages.
- The portrait `image13.png` (blond hair, blue jumper) fits the eyewitness description and Dr Salt's own comment about a blue jumper he had just bought.
- Overall opinion: the evidence supports the prosecution's case more strongly than the defence's.

## Lessons Learned

### Technical

- **Signature search alone is not enough.** The first search gave 854 JPEG hits and over 5,400 OOXML hits. Most were embedded thumbnails or parts of other archives. What made carving work was a fixed set of filters: row alignment, the slack bytes before the header, and the structure after it. Writing those rules down before looking at the hits stopped me from picking the result I hoped for.
- **Cluster size needs outside context.** Every offset divided cleanly by 512, 1024, 2048, 4096 and 8192, so the maths alone could not choose a size. The answer came from the acquisition log (sector count times 512 bytes is 3880 MB, not the 8 GB from OSINT) and the FAT32 default table. Checking what a device really holds against what is claimed for it is part of the job.
- **The footer decides whether a carve is valid.** For ZIP-based files, the first EOCD found is often not the right one. Two belonged to embedded objects, one to a Python project and one to a web backup. Reading the file count field and the file names near each EOCD, and checking for zero padding after it, was what identified the real end of the document.
- **Decoys exist.** One OOXML candidate looked perfect and held nothing but six `.wav` files. Opening and checking the contents of each carved file, and not stopping at "it extracts without errors", is what exposed it.
- **Do not trust file extensions.** Two images in `word/media/` had fake extensions so that tools would skip them. Checking each file's header against its extension should be routine.
- **Look at the raw XML.** The most important evidence (the hidden signature, the plan text and the hidden table) was invisible in Word. It only appeared when I read `document.xml` and searched for the `FEFFFF` colour value.
- **Always work on copies.** Every carved block was written to a new file, and the SHA256 in the FTK log was checked first. This keeps the original evidence untouched and the results repeatable.

### Reporting and Professional Practice

- **Normalise time zones before building a timeline.** The document times are UTC, but the interview time is written as "14:00 hours" with no zone, and I put it in the timeline as UTC. If it was local time during British Summer Time (UTC+1), the interview began at 13:00 UTC, which is before the 13:17 print event. Every timestamp should be converted to one stated zone, and any uncertainty should be written down.
- **Check copied tables and numbers.** In the Candidate B cluster check, I reused the Candidate A table, so it still shows offset 4726784. Candidate B's end offset also appears as both 3270618944 and 3270618946. An exhibit list also skips B-12. Small mistakes like these give the other side an easy way to question the whole report, so a final pass to check each number against its screenshot is worth the time.
- **Keep fact and inference apart.** Some points, such as the balloon zoo link, B.B. Leaks being Dr Salt, and what Dr Salt knew about the news article before printing, are reasonable but not proven. Labelling them clearly as contextual inferences made the report more balanced and easier to defend.
- **Give the defence a fair hearing.** Listing the points that help the defence (Marsha Lowe created the document, nobody saw the act, he denied everything) made the final opinion stronger, not weaker. An expert's duty is to the court, not to either side.
- **Take notes while working.** Writing the Appendix A notes and screenshots as I went made it possible to explain each carving decision later. Notes written afterwards from memory would have missed the reasons for rejecting each candidate.
- **Know your limits.** This was my first formal expert report, and I said so in the qualifications section. Being honest about experience is part of the declaration of truth the statement opens with.

