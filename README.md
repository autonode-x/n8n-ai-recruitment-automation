# AI-Assisted Recruitment Intake & Human Review

> **Architecture:** See [ARCHITECTURE.md](ARCHITECTURE.md) for the infographic, detailed flow, and trust boundaries.

An n8n workflow template for collecting a job application, extracting job-relevant resume evidence, preparing a structured **advisory-only** assessment, and routing the application to an authorized human reviewer. The reviewer explicitly chooses **advance**, **reject**, or **hold**. The workflow does not make an autonomous hiring decision.

> **Sensitive-domain notice:** Recruitment data and candidate decisions are high-impact and privacy-sensitive. The supplied public workflow is a sanitized template, not a production-ready hiring system. Its intake webhook is **not authenticated in the source export**; add authentication and complete the safeguards below before any live use.

## Project snapshot

- **Category:** n8n workflow automation, recruitment operations, human-in-the-loop AI
- **Workflow size:** 42 nodes, 37 connection groups in the supplied export
- **AI provider in source:** Google Gemini via n8n LangChain nodes
- **Workflow services:** Google Drive, Sheets, Gmail, Calendar, Slack, and n8n Wait/approval form
- **Purpose:** Reduce repetitive intake and preparation work while preserving human control over candidate decisions
- **Decision boundary:** AI output is advisory; only an authorized human reviewer can advance, reject, or hold an application

## What it does

1. Receives a candidate application containing a name, email, position, application ID, job description, resume URL, and an acknowledgement flag for the privacy notice.
2. Validates basic field formats and lengths, including a Google Drive file URL pattern and `privacy_notice_acknowledged === true`.
3. Checks the application ID in a Google Sheets register for duplicates, records a new application, and acknowledges accepted/duplicate requests.
4. Downloads the resume from Google Drive, extracts text, and asks an AI model to extract job-relevant evidence into a structured schema.
5. Generates an evidence-cited, transparent advisory fit assessment from the position, job description, and extracted evidence. Invalid AI output is diverted to a manual-review status.
6. Records the case as awaiting human review and notifies HR.
7. Presents candidate/job context and the advisory assessment through a reviewer form with **advance**, **reject**, and **hold** choices.
8. On **advance**, generates role-related interview questions, creates a calendar event, sends an interview invitation to the candidate, and updates the record. The current template uses a fixed time slot; it must be replaced with an approved scheduling process before activation.
9. On **reject**, sends a rejection message only after the reviewer explicitly selects reject, then records the human rejection. **Hold** and validation-error paths are recorded for manual handling.

## Input contract

| Field | Required | Validation in source template |
|---|---:|---|
| `candidate_name` | Yes | String, 2–120 characters |
| `candidate_email` | Yes | Basic email format, max 254 characters |
| `position` | Yes | String, 2–120 characters |
| `resume_url` | Yes | HTTPS Google Drive file URL pattern |
| `application_id` | Yes | 8–128 characters, letters/digits/underscore/hyphen |
| `job_description` | Yes | 30–10,000 characters |
| `privacy_notice_acknowledged` | Yes | Must be boolean `true` |

The privacy acknowledgement flag is only a submitted boolean; the workflow does **not** itself prove that valid notice text was presented or that consent is legally sufficient. Implement a genuine candidate notice/consent and retention process appropriate to the jurisdiction.

## Portfolio-safe workflow file

`workflow.json` is a scrubbed n8n export. Credential bindings, webhook IDs, the original n8n instance ID, personal email, Slack channel IDs, Google Sheet identifiers, and calendar ID were removed or replaced with placeholders. The webhook path has been changed to a portfolio-only path, and the node is explicitly labeled for authentication setup.

Importing the template will **not** make it operational. Re-bind your own credentials and private resources in n8n, and do not use real candidate data for portfolio demos. Use synthetic resumes and addresses only.

## Critical pre-production security and fairness checklist

1. **Authenticate the intake webhook before activation.** The export does not show webhook auth configured despite the original node label. Add header authentication, rate limiting, payload-size caps, abuse monitoring, and non-production testing. Restrict allowed callers.
2. **Protect the reviewer form.** Configure its Basic Auth credential, restrict reviewers to authorized hiring staff, and protect the resume URL and review link. Add decision logging, reviewer identity, and a secure audit process.
3. **Replace the fixed interview slot.** The supplied expression schedules the same India-time slot three days after approval. It does not check calendar availability or obtain candidate availability. Use a human-approved slot or a real availability check before sending invitations.
4. **Constrain resume access.** A URL pattern is not proof that the file belongs to the applicant or is safe. Restrict the Google Drive credential to an approved folder, verify file ownership/permissions and MIME type, cap size/pages, scan uploads, and limit text extraction.
5. **Minimize candidate data.** Resumes, names, email addresses, job descriptions, extracted facts, reviewer notes, scores, and calendar details are personal data. Apply access controls, encryption, provider review, retention/deletion policies, and a process for candidate requests.
6. **Review AI provider data handling.** Resume-derived content is sent to the configured model provider. Verify contractual terms, retention/training settings, region, and legal basis before using real applicants' information.
7. **Keep humans accountable.** Do not use the AI fit score or rank as the sole or determinative basis for employment decisions. Review the original materials, validate evidence, document reasons, and provide appropriate accommodations and recourse.
8. **Fairness and validity testing.** Assess job-relatedness, accessibility, disparate impact, language effects, and errors before any use. Exclude protected characteristics and proxies; do not infer age, disability, race, religion, family status, or other sensitive traits. Obtain qualified legal/compliance review for relevant jurisdictions.
9. **Provider credentials and logs.** Use least-privilege Google/Gmail/Calendar/Drive/Slack credentials and n8n's credential manager. Restrict execution history and delete sensitive data on a defined schedule.
10. **Rotate exposed secrets.** Sanitizing this JSON does not revoke credentials or undo previous disclosures. Rotate any credential that was ever public, review repository history, and replace any personal contact details with role-based aliases.

## Known limitations

- Basic validation expressions do not provide authentication, consent proof, file safety, or identity verification.
- The model may omit, misinterpret, or hallucinate resume evidence. A structured schema is not proof of accuracy or fairness.
- The model receives up to a bounded amount of extracted resume text; external model/data handling must be assessed before using actual applicant data.
- Duplicate prevention uses an application-ID sheet lookup; verify concurrency/idempotency behavior under parallel submissions.
- The current interview slot is a template default, not a production scheduling policy.
- This project is an automation prototype, not a compliance certification or legal/HR recommendation.

## Suggested freelancing portfolio description

> Designed an n8n recruitment-intake prototype with validated submissions, application-ID deduplication, resume evidence extraction, structured AI-assisted job-fit notes, and an explicit human review gate. Human reviewers choose advance/reject/hold; AI output is advisory only. The advance path prepares interview questions, calendar scheduling, and a candidate invitation. The project includes a sanitized public export and documents authentication, candidate privacy, fairness, and scheduling hardening needed before real-world deployment.

## License

Publish only if you have authority to share the workflow and prompts. Review the n8n, node, model-provider, and data-protection terms that apply to your intended use.


## Synthetic request and response examples

- [Sample request](sample/request.json)
- [Sample successful response](sample/response.json)

These examples use synthetic demonstration data and show the successful route only. HTTP status codes are returned separately by the workflow. Configure authentication and credentials in your own n8n instance before activation.
