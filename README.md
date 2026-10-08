# Podcast to AI-Optimized Blog Automation

An AI automation workflow built with n8n that transforms podcast transcripts into structured, publication-ready blog content using the OpenAI API and automatically creates a Google Doc containing the finished content package.

## Current Workflow

The workflow currently:

1. Accepts a podcast transcript as input.
2. Accepts a YouTube URL associated with the episode.
3. Accepts a list of relevant business resources or offers.
4. Validates that the transcript contains non-whitespace content before processing.
5. Parses and validates supported YouTube URL formats, including:
   - Standard YouTube watch URLs
   - `youtu.be` URLs
   - YouTube Shorts URLs
6. Extracts and stores the validated 11-character YouTube video ID.
7. Stops the workflow with a clear error if the transcript is missing or the YouTube URL is invalid.
8. Sends the transcript and resource context to an OpenAI model.
9. Uses a strict JSON schema to return:
   - Blog title
   - Meta title
   - Meta description
   - Blog HTML
   - FAQ schema
10. Validates the structured OpenAI response before downstream processing.
11. Uses JavaScript in an n8n Code node to:
    - Confirm all required structured fields are present and non-empty
    - Reuse the already-validated YouTube video ID
    - Generate deterministic YouTube embed HTML
    - Insert the embed into the finished article
12. Creates a new Google Doc using the generated blog title.
13. Populates the document with:
    - Blog title
    - Meta title
    - Meta description
    - Article HTML
    - FAQ schema
    - YouTube URL

## Workflow Flow

```text
Manual Trigger
→ Edit Fields
→ Transcript Validation
→ YouTube URL Parsing
→ YouTube Validation
→ OpenAI Structured Content Generation
→ OpenAI Response Validation + JavaScript Formatting
→ YouTube Embed Generation
→ Google Docs Creation
→ Google Docs Update
```

## Tech Stack

- n8n
- OpenAI API
- Google Docs API
- Google Drive API
- JSON Schema
- JavaScript
- Docker
- Git / GitHub

## Design Decisions

The workflow separates generative AI tasks from deterministic automation logic.

OpenAI is used for tasks requiring language understanding and contextual judgment, including:

- Transforming an unstructured transcript into structured article content
- Generating search metadata
- Creating FAQ content and schema
- Determining whether provided resources are contextually relevant

n8n and JavaScript handle predictable workflow logic and transformations, including:

- Validating required input
- Rejecting blank or whitespace-only transcripts
- Parsing supported YouTube URL formats
- Extracting and storing the validated YouTube video ID
- Reusing the validated video ID downstream rather than parsing the URL multiple times
- Validating the structured OpenAI response before document creation
- Generating embed HTML
- Creating the Google Doc
- Passing structured output between workflow steps
- Stopping execution when required input or output is invalid

This reduces unnecessary reliance on the LLM, avoids duplicate parsing logic, and makes the automation more predictable and easier to troubleshoot.

## Input and Output Validation

The workflow performs validation both before and after the OpenAI API call.

Input validation includes:

- Transcript must contain non-whitespace content
- YouTube URL must match a supported YouTube format
- YouTube video ID must match the expected 11-character format

If input validation fails, the workflow stops with an explicit error rather than making an unnecessary OpenAI API call.

The structured OpenAI response is also validated before Google Docs processing begins.

The workflow confirms that the response contains non-empty string values for:

- `blog_title`
- `meta_title`
- `meta_description`
- `html`
- `faq_schema`

If the expected structured content is missing or incomplete, the workflow throws a clear error instead of creating an incomplete document.

## Google Docs Output

The workflow automatically creates a Google Doc for each successful run.

The document includes:

- Blog title
- Meta title
- Meta description
- Article HTML
- FAQ schema
- Original YouTube URL

The article is currently stored as literal HTML source inside the Google Doc. This is intentional for the current version because the output is designed to be reviewed before being copied into a publishing platform or CMS.

## Security

- API keys and OAuth secrets are stored outside the repository.
- The current public workflow export omits n8n credential references and instance-specific metadata.
- Environment-specific values, such as the Google Drive folder ID, are replaced with placeholders before publication.
- Workflow exports are reviewed and validated before being committed.
- Public repository files contain no API keys or OAuth secrets.

## Version 1 Status

Working:

- Manual transcript input
- YouTube URL input
- Relevant-resource input
- Transcript input validation
- Whitespace-only transcript rejection
- YouTube URL parsing
- YouTube watch URL support
- `youtu.be` URL support
- YouTube Shorts URL support
- YouTube video ID validation
- Explicit Stop & Error handling for invalid input
- OpenAI API integration
- Structured JSON Schema output
- Structured OpenAI response validation
- Required-field validation before downstream processing
- HTML article generation
- SEO metadata generation
- FAQ schema generation
- Relevant-resource context
- Deterministic YouTube embed generation
- Reuse of validated YouTube video ID downstream
- Google Docs document creation
- Automated content insertion into Google Docs
- Persistent local n8n environment using Docker
- Sanitized GitHub workflow export

Still to add:

- Additional testing with realistic full-length podcast transcripts
- Portfolio screenshots
- Sanitized example output
- Setup and import instructions for reproducing the workflow
- Final portfolio documentation

## Repository Files

- `workflow-v1.json` - sanitized exported n8n workflow
- `README.md` - project documentation
- `.gitignore` - excludes local credentials, environment files, n8n data, and other local-only files
- `sample-output.md` - sanitized example of the workflow's generated content package

## Project Goal

This project converts an existing manual AI-assisted podcast-to-blog process into a repeatable automation.

The project is designed to demonstrate practical AI implementation skills, including:

- Workflow design
- API integration
- Structured LLM output
- JSON Schema configuration
- JavaScript transformations
- Input validation
- Output validation
- Error handling
- OAuth integration
- Google Docs automation
- Docker-based local workflow execution
- Testing and troubleshooting
- Secure credential handling
- Git and GitHub version control
- AI-assisted development and code review

The goal is not to automate every part of publishing. The current version focuses on producing a reliable, structured content package that can be reviewed before publication.