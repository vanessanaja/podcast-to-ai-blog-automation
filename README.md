# Podcast to AI-Optimized Blog Automation

An AI automation workflow built with n8n that transforms podcast transcripts into structured, publication-ready blog content using the OpenAI API.

## Current Workflow

The workflow currently:

1. Accepts a podcast transcript as input.
2. Accepts a YouTube URL associated with the episode.
3. Accepts a list of relevant business resources or offers.
4. Sends the transcript and resource context to an OpenAI model.
5. Uses a strict JSON schema to return structured fields:
   - Blog title
   - Meta title
   - Meta description
   - Blog HTML
   - FAQ schema
6. Uses JavaScript in an n8n Code node to:
   - Extract the YouTube video ID
   - Generate deterministic YouTube embed HTML
   - Insert the embed into the finished article
7. Outputs structured content ready for downstream publishing or review.

## Tech Stack

- n8n
- OpenAI API
- JSON Schema
- JavaScript
- Docker
- Git / GitHub

## Design Decisions

The workflow separates generative AI tasks from deterministic automation logic.

OpenAI is used for tasks requiring language understanding and contextual judgment, such as:

- Transforming an unstructured transcript into a structured article
- Generating metadata
- Creating FAQ content and schema
- Determining whether provided resources are relevant enough to include

n8n and JavaScript handle predictable transformations such as generating the YouTube embed code.

This reduces unnecessary reliance on the LLM and makes the workflow more predictable.

## Security

- API credentials are stored in n8n's credential manager.
- API keys are not stored in the repository.
- Exported n8n workflows are sanitized before being committed to GitHub.

## Version 1 Status

Working:

- Manual transcript input
- OpenAI API integration
- Structured JSON Schema output
- HTML article generation
- SEO metadata generation
- FAQ schema generation
- Relevant-resource context
- YouTube embed generation
- Persistent local n8n environment using Docker

Still to add:

- Google Docs output
- Validation and error handling
- Testing with realistic full-length podcast transcripts

## Repository Files

- `workflow-v1.json` - sanitized exported n8n workflow
- `README.md` - project documentation

## Project Goal

This project is being built as a practical AI implementation portfolio project focused on converting an existing manual AI-assisted content workflow into a repeatable automation.