# Podcast to AI-Optimized Blog Automation

An AI automation workflow built with n8n that transforms podcast transcripts into structured, publication-ready blog content using the OpenAI API and automatically creates a Google Doc containing the finished content package.

## Current Workflow

The workflow currently:

1. Accepts a podcast transcript as input.
2. Accepts a YouTube URL associated with the episode.
3. Accepts a list of relevant business resources or offers.
4. Sends the transcript and resource context to an OpenAI model.
5. Uses a strict JSON schema to return:
   - Blog title
   - Meta title
   - Meta description
   - Blog HTML
   - FAQ schema
6. Uses JavaScript in an n8n Code node to:
   - Extract the YouTube video ID
   - Generate deterministic YouTube embed HTML
   - Insert the embed into the finished article
7. Creates a new Google Doc using the generated blog title.
8. Populates the document with:
   - Blog title
   - Meta title
   - Meta description
   - Article HTML
   - FAQ schema
   - YouTube URL

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

- Extracting the YouTube video ID
- Generating embed HTML
- Creating the Google Doc
- Passing structured output between workflow steps

This reduces unnecessary reliance on the LLM and makes the automation more predictable and easier to troubleshoot.

## Security

## Security

- API keys and OAuth secrets are stored outside the repository.
- The current public workflow export omits n8n credential references and instance-specific metadata.
- Environment-specific values, such as the Google Drive folder ID, are replaced with placeholders before publication.
- Workflow exports are reviewed and validated before being committed.

## Version 1 Status

Working:

- Manual transcript input
- YouTube URL input
- Relevant-resource input
- OpenAI API integration
- Structured JSON Schema output
- HTML article generation
- SEO metadata generation
- FAQ schema generation
- Relevant-resource context
- Deterministic YouTube embed generation
- Google Docs document creation
- Automated content insertion into Google Docs
- Transcript input validation
- YouTube URL validation
- Explicit Stop & Error handling for invalid input
- Persistent local n8n environment using Docker

Still to add:

- Additional testing with realistic full-length podcast transcripts
- Portfolio documentation and example output

## Repository Files

- `workflow-v1.json` - sanitized exported n8n workflow
- `README.md` - project documentation

## Project Goal

This project converts an existing manual AI-assisted podcast-to-blog process into a repeatable automation.

The project is designed to demonstrate practical AI implementation skills, including workflow design, API integration, structured LLM output, deterministic scripting, OAuth integration, testing, troubleshooting, and secure handling of credentials.