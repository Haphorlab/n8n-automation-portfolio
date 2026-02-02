Installation Guide
Complete setup instructions for the RAG-Powered Knowledge Base Chatbot.
Prerequisites
Before you begin, ensure you have:

 Active n8n instance (self-hosted or cloud)
 OpenAI API key with billing enabled
 Pinecone account (free tier works)
 Google account with Drive access
 Basic understanding of n8n workflows

Step 1: Import Workflow

Download workflow.json from this repository
In n8n, click Workflows → Import from File
Select the downloaded JSON file
Click Import

Step 2: Configure Credentials
OpenAI API

Get API key from OpenAI Platform
In n8n: Credentials → New → OpenAI API
Enter your API key
Name it "OpenAi account"
Save

Pinecone API

Sign up at Pinecone
Create a new project
Go to API Keys and copy your key
In n8n: Credentials → New → Pinecone API
Enter your API key
Name it "faqAPI"
Save

Google Drive OAuth2

Go to Google Cloud Console
Create/select a project
Enable Google Drive API
Create OAuth 2.0 credentials (Web application)
Add authorized redirect URIs from n8n
In n8n: Credentials → New → Google Drive OAuth2 API
Enter Client ID and Client Secret
Complete OAuth flow
Name it "Google Drive account"
Save

Step 3: Create Pinecone Index

In Pinecone dashboard, click Create Index
Configure:

Name: faqccc (or your choice)
Dimensions: 1536 (required for OpenAI ada-002)
Metric: cosine
Cloud: Choose your region


Click Create Index
Wait for index to be ready (1-2 minutes)
Copy the index URL from index details

Step 4: Update Workflow Configuration
Google Drive Folder

Create a folder in Google Drive for your knowledge base documents
Note the folder ID from the URL:

   https://drive.google.com/drive/folders/1aCOW7IA9TEAYzyy1vggku-0fBAyGYpMX
                                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                                            This is your folder ID

In workflow, click Google Drive Trigger node
Select your folder under "Folder to Watch"
Save node

Pinecone Configuration

Click Pinecone Vector Store node
Select your index from dropdown
Verify namespace is "faq" (or change as needed)
Save node
Click Pinecone - Query1 node
Update URL to your Pinecone index URL:

   Format: https://INDEX_NAME-PROJECT_ID.svc.ENVIRONMENT.pinecone.io

Save node

Step 5: Test the Workflow
Test Document Processing

Activate the workflow (toggle switch at top)
Upload a test PDF or text file to your Google Drive folder
Wait 1 minute for trigger to fire
Check Executions tab in n8n for successful run
In Pinecone dashboard, verify vectors were added

Test the Chatbot

Click Webhook2 node
Copy the Production URL
Test with curl:

bashcurl -X POST YOUR_WEBHOOK_URL \
  -H "Content-Type: application/json" \
  -d '{"question": "Test question from your document"}'

Verify you get a JSON response with an answer

Step 6: Add Your Knowledge Base
Now that everything is working:

Add your actual documents to the Google Drive folder
Supported formats: PDF, TXT, DOC, DOCX, MD
Each file will be processed automatically within 1 minute
Monitor executions to ensure all documents process successfully

Common Issues & Solutions
Workflow Not Triggering
Problem: Documents uploaded but workflow doesn't run
Solution:

Ensure workflow is Active (toggle at top)
Check Google Drive trigger is set to "Every Minute"
Verify folder permissions in Google Drive
Review n8n execution logs for errors

Embeddings Not Storing
Problem: Workflow runs but no vectors in Pinecone
Solution:

Confirm Pinecone index has 1536 dimensions
Check API key is valid and has permissions
Verify namespace matches in both nodes
Review Pinecone dashboard for errors

No Response from Webhook
Problem: Webhook returns error or no response
Solution:

Test webhook URL in browser (should show error for GET)
Verify all credentials are connected
Check n8n logs for execution errors
Ensure OpenAI API key has billing enabled

Poor Answer Quality
Problem: Chatbot gives irrelevant or incorrect answers
Solution:

Ensure documents are relevant to questions being asked
Verify documents processed successfully (check Pinecone vector count)
Increase topK value in Pinecone Query node for more context
Review system prompt in "OpenAI - Generate Answer" node

API Rate Limit Errors
Problem: OpenAI or Pinecone rate limit errors
Solution:

Implement delays between batch document uploads
Upgrade OpenAI tier if processing many documents
Consider caching frequent questions
Review API usage in respective dashboards

Customization Guide
Change Response Tone
Edit the system prompt in OpenAI - Generate Answer2 node:
javascript"role": "system",
"content": "You are a [helpful/professional/friendly/technical] assistant for [Your Company]. [Additional instructions]"
Adjust Context Amount
In Pinecone - Query1 node, change topK value:

topK: 3 - Less context, faster responses
topK: 5 - Default, balanced
topK: 10 - More context, slower but more comprehensive

Modify Response Length
In OpenAI - Generate Answer2 node:

max_tokens: 200 - Short answers
max_tokens: 500 - Default, medium length
max_tokens: 1000 - Detailed answers

Security Checklist
Before going to production:

 API keys stored as n8n credentials (not hardcoded)
 Webhook URL uses HTTPS
 Consider adding authentication to webhook
 Implement rate limiting if public-facing
 Set up monitoring and alerts
 Regular API key rotation schedule
 Document access controls reviewed

Monitoring & Maintenance
Daily Tasks

Review n8n execution logs
Check for failed document processing
Monitor API usage and costs

Weekly Tasks

Review most asked questions
Analyze response quality
Update knowledge base documents

Monthly Tasks

Rotate API keys
Review and optimize costs
Update system prompts if needed
Audit vector database for stale content

Cost Estimation
Based on typical usage:
Document Processing

100 documents/month: ~$1-2
1000 documents/month: ~$10-20

Query Handling

1000 queries/month: ~$5-10
10,000 queries/month: ~$50-100

Pinecone

Free tier: Up to 100K vectors
Paid tier starts at $70/month for more vectors

Next Steps
Once everything is set up:

✅ Test with sample questions
✅ Add comprehensive documentation to Drive
✅ Build a frontend interface (optional)
✅ Integrate with Slack/Discord/Website
✅ Set up monitoring and alerts
✅ Train team on adding new documents

Support Resources

n8n Community: community.n8n.io
OpenAI Docs: platform.openai.com/docs
Pinecone Docs: docs.pinecone.io
Google Drive API: developers.google.com/drive

