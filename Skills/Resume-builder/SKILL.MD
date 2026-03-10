---
name: resume-builder
description: "Professional resume and cover letter builder that personalizes your application for each job. Use this skill whenever the user wants to: tailor or customize a resume for a specific job posting, write a cover letter for a job application, analyze a job description to understand requirements, research a company before applying, prepare application materials, or optimize their resume for ATS (applicant tracking systems). Trigger whenever the user mentions 'resume', 'CV', 'cover letter', 'job application', 'apply for a job', 'tailor my resume', or shares a job posting/description and wants help applying. Also trigger when the user asks to analyze a job posting, compare their experience to job requirements, or wants a job fit assessment. Even casual mentions like 'I found this job I want to apply for' or 'can you help me with my application' should trigger this skill."
---

# Resume & Cover Letter Builder

Build personalized, job-targeted resumes and cover letters by analyzing the role, researching the company, and intelligently tailoring your existing materials.

## Philosophy

The best applications feel like they were written *for* this specific role at this specific company — not like a generic resume blasted to 50 postings. This skill achieves that through a structured pipeline: understand the job, understand the company, understand the candidate, then synthesize everything into polished application materials.

The tailoring approach is **light but strategic**: optimize keywords, adjust emphasis, and refine phrasing to align with the role — while preserving the candidate's authentic voice and original content structure. Think of it as a skilled editor, not a ghostwriter.

## Workflow Overview

The process has five phases. Each one builds on the last, so follow them in order.

### Phase 1: Gather Inputs

You need two things to start: the candidate's base resume and the job they're targeting.

**Base resume:**
- Check if the user has provided or previously uploaded a resume file (PDF, DOCX, or plain text)
- If no resume is available, ask the user to provide one — either by uploading a file or pasting their experience
- Extract all content: contact info, summary/objective, work experience (titles, companies, dates, bullet points), education, skills, certifications, projects, and any other sections
- Preserve the original structure and section ordering — this is the candidate's preferred layout

**Job description:**
- Accept either a URL (fetch and extract the posting) or pasted text
- Extract and organize: job title, company name, location, required qualifications, preferred qualifications, responsibilities, salary range (if listed), and any other notable details
- Identify the seniority level and department/function from context clues

### Phase 2: Company Research

Use web search to build a rich picture of the employer. This research directly informs both the resume tailoring and the cover letter tone.

Research these areas:
- **Company overview**: What they do, their market position, size, and stage (startup vs. enterprise)
- **Culture and values**: Mission statement, stated values, Glassdoor/LinkedIn sentiment, work style (remote, hybrid, etc.)
- **Recent news**: Product launches, funding rounds, leadership changes, partnerships, press coverage from the last 6 months
- **Industry context**: Competitive landscape, market trends affecting the company, growth trajectory
- **Tech stack or tools** (if relevant): What technologies, platforms, or methodologies they use

Compile findings into a concise company brief — you'll reference this throughout the rest of the process.

### Phase 3: Job Fit Analysis

This is the strategic core. Compare the candidate's background against the job requirements and produce a clear-eyed assessment.

**Present this analysis to the user** before proceeding to document creation. It serves two purposes: it helps the candidate understand their positioning, and it lets them course-correct before you write anything.

Structure the analysis as:

**Requirements Match**
- Go through each required and preferred qualification
- For each one, note whether the candidate meets it (strong match), partially meets it (transferable experience), or doesn't meet it (gap)
- Be specific — cite which resume bullet points or experiences map to which requirements

**Strategic Recommendations**
- Which existing bullet points to emphasize or move to a more prominent position
- Keywords from the job description that should appear in the resume (for ATS optimization)
- How to frame transferable experience to address any gaps
- What to highlight in the cover letter to compensate for weak areas in the resume

**Overall Assessment**
- A candid summary of fit strength (strong/moderate/stretch)
- The 2-3 most compelling selling points for this specific role
- Any red flags or areas that need careful framing

After presenting the analysis, ask the user if they'd like to adjust anything before you proceed to creating the documents.

### Phase 4: Tailor the Resume

Apply light, strategic tailoring to the base resume. The goal is keyword optimization and emphasis adjustment — not a rewrite.

**What to change:**
- Adjust the professional summary/objective to align with this specific role
- Incorporate key terminology from the job description into existing bullet points where it fits naturally
- Reorder bullet points within each role to lead with the most relevant accomplishments
- If the resume has a skills section, ensure it mirrors the job's required skills (only include skills the candidate actually has)
- Adjust section ordering if a non-standard order would better serve this application (e.g., moving a "Projects" section higher if the role emphasizes project work)

**What NOT to change:**
- Don't fabricate experience, skills, or accomplishments
- Don't dramatically rewrite bullet points — adjust wording, don't replace content
- Don't change the fundamental structure or design unless the user asks
- Don't remove sections unless they're clearly irrelevant and the user agrees
- Preserve dates, company names, titles, and factual details exactly

**Output format:**
Generate the tailored resume as both .docx and .pdf. Use the `docx` skill's approach for creating Word documents (JavaScript with the `docx` npm package), then convert to PDF via LibreOffice. The formatting should be clean and professional — readable fonts, clear hierarchy, appropriate whitespace. Match the general style of the original resume where possible.

Read the docx SKILL.md and pdf SKILL.md before creating files to ensure proper document generation. Pay attention to page size (US Letter), margins, and the validation step.

### Phase 5: Write the Cover Letter

Craft a personalized cover letter that complements (not duplicates) the resume. The cover letter should do what a resume can't: tell a story, show personality, and connect the dots between the candidate's background and this specific role at this specific company.

**Structure:**
- **Opening**: A compelling hook that shows genuine interest in the company and role. Reference something specific from the company research — a recent initiative, a value that resonates, a product the candidate admires. Avoid generic openings.
- **Body (1-2 paragraphs)**: Connect the candidate's most relevant experience to the role's key requirements. Use specific examples and results from the resume, but frame them as a narrative. Address any gaps proactively if the fit analysis identified significant ones. Weave in company-specific language and values.
- **Closing**: Clear call to action, enthusiasm for next steps, and a brief reinforcement of the candidate's value proposition for this specific role.

**Tone guidelines:**
- Professional but warm — not stiff or overly formal
- Confident without being arrogant
- Specific and concrete — avoid vague platitudes like "I'm a team player"
- Keep it to one page (roughly 300-400 words)
- Match the company's communication style when possible (a startup cover letter reads differently from a Fortune 500 one)

**Output format:**
Generate as both .docx and .pdf, same as the resume. Clean, professional formatting with proper letter structure (date, addressee if known, salutation, body, sign-off).

## File Generation Notes

When creating the .docx files, read the docx skill (at the standard skill location) for proper document creation patterns. Key reminders:
- Use `npm install -g docx` and create via JavaScript
- Set US Letter page size explicitly (12240 x 15840 DXA)
- Validate after creation with `python scripts/office/validate.py`
- Convert to PDF: `python scripts/office/soffice.py --headless --convert-to pdf document.docx`

For the resume specifically, professional formatting matters:
- Use a clean, modern layout with clear section headers
- Keep font choices readable (Arial, Calibri, or similar)
- Use consistent spacing and alignment
- Ensure the resume fits on 1-2 pages (don't let tailoring bloat it)

For the cover letter:
- Standard business letter format
- Consistent font with the resume for a cohesive application package
- Proper margins and spacing

## Handling Edge Cases

- **No resume provided**: Ask the user for their experience. You can interview them to build a resume from scratch, but note this will take longer and the result will need more review.
- **Vague job posting**: Do your best with what's available. Note which parts of the analysis are less certain due to limited information.
- **Career change scenarios**: Focus extra attention on transferable skills in the fit analysis. The cover letter becomes even more important for bridging the gap.
- **Multiple roles at same company**: If the user wants to apply for several positions, generate separate tailored materials for each. Don't try to create a "one size fits all" version.
- **Non-English applications**: If the job posting is in another language, create the materials in that language. Note this to the user and confirm before proceeding.