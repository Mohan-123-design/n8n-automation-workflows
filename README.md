# n8n Automation Workflows

A collection of production n8n workflow JSON files for email automation, AI image generation, and job listing exports. Import any workflow directly into your n8n instance.

## Workflows

### 📧 Email Sender (`email_sender/`)

| File | Description |
|------|-------------|
| `gmail_sender_workflow.json` | Reads from Google Sheets, sends Gmail, updates row with SENT/FAILED status + timestamp |
| `slack_sender_workflow.json` | Same trigger + status logic but sends to Slack instead of Gmail |

**Features:**
- Polls Google Sheets every minute for new rows with `Status = Pending` or blank
- Sends personalized emails/messages (Name, Subject, CustomMessage columns)
- Updates row status to `SENT` or `FAILED` with error details and timestamp
- Loops through multiple rows per trigger using batch processing

**Sheet structure required:**
```
Row id | Name | Email | Subject | CustomMessage | Status | Timestamp | Notes
```

---

### 🖼️ Image Generation (`image_generation/`)

| File | Description |
|------|-------------|
| `prompt_gen_workflow.json` | Reads LinkedIn post content → calls Perplexity → writes AI image prompt back to sheet |
| `image_gen_workflow.json` | Reads prompt from sheet → generates image via Gemini → uploads to Slack → writes URL back |

**Features:**
- Two-phase pipeline (prompt first, then image)
- Uses Google Gemini 2.5 Flash for image generation
- Uploads images to Slack workspace, saves permalink + private URL
- Skips rows that already have a Status set

**Sheet structure:**
```
S. No | Content | PROMPT | Status | URLS
```

---

### 💼 Upwork Export (`upwork_export/`)

| File | Description |
|------|-------------|
| `Upwork Job Listings Auto-Export to Google Sheets with Apify.json` | Automated Upwork job scraping via Apify actor → exports to Google Sheets |

---

## How to Import Any Workflow

1. Open your **n8n** instance
2. Click **Workflows** in the sidebar
3. Click **Import from file** (or drag & drop)
4. Select the `.json` file
5. Click **Save**
6. Connect your credentials (Google Sheets, Gmail, Slack, Perplexity, Gemini)
7. **Activate** the workflow

## Credentials Needed

| Service | Used In |
|---------|---------|
| Google Sheets (OAuth2) | All workflows |
| Gmail (OAuth2) | gmail_sender_workflow |
| Slack (OAuth2) | slack_sender_workflow, image_gen_workflow |
| Perplexity API | prompt_gen_workflow |
| Google Gemini (PaLM API) | image_gen_workflow |
| Apify API | upwork_export workflow |

## Tech Stack

- n8n (self-hosted or cloud)
- Google Sheets API
- Gmail API
- Slack API
- Perplexity API
- Google Gemini API
- Apify
