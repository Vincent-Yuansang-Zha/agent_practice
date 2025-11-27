# Setup Guide: Multi-Agent Service Matcher

This guide will help you set up and run the multi-agent service matcher notebook using Google Cloud Vertex AI.

## Prerequisites

1. **Google Cloud Project Access**: You need access to the project `d-ulti-ml-ds-dev-9561`
2. **Python 3.8+**: Make sure you have Python installed
3. **Jupyter Notebook**: To run the `.ipynb` file

## Setup Steps

### 1. Environment Configuration ✅ (Already Done!)

The `.env` file is already configured with:
```
GOOGLE_CLOUD_PROJECT=d-ulti-ml-ds-dev-9561
GOOGLE_CLOUD_LOCATION=global
GOOGLE_GENAI_USE_VERTEXAI=True
```

### 2. Install Google Cloud CLI

If you don't have it already, install the Google Cloud CLI:

**macOS:**
```bash
brew install google-cloud-sdk
```

**Linux:**
```bash
curl https://sdk.cloud.google.com | bash
exec -l $SHELL
```

**Windows:**
Download from: https://cloud.google.com/sdk/docs/install

### 3. Authenticate with Google Cloud

Run these commands in your terminal:

```bash
# Login to Google Cloud
gcloud auth application-default login

# Set your default project
gcloud config set project d-ulti-ml-ds-dev-9561
```

This will:
- Open your browser for Google authentication
- Store credentials locally for the Vertex AI SDK to use
- Set the default project for all gcloud commands

### 4. Verify Authentication

Test that everything is working:

```bash
# Check which account you're authenticated as
gcloud auth list

# Verify the project is set correctly
gcloud config get-value project
```

You should see:
- Your Google account email
- Project: `d-ulti-ml-ds-dev-9561`

### 5. Install Python Dependencies

In your project directory, run:

```bash
pip install google-cloud-aiplatform python-dotenv jupyter
```

Or if you're using a virtual environment (recommended):

```bash
# Create virtual environment
python -m venv venv

# Activate it
source venv/bin/activate  # On macOS/Linux
# OR
venv\Scripts\activate     # On Windows

# Install dependencies
pip install google-cloud-aiplatform python-dotenv jupyter
```

### 6. Enable Required APIs

Make sure the Vertex AI API is enabled in your Google Cloud project:

```bash
gcloud services enable aiplatform.googleapis.com --project=d-ulti-ml-ds-dev-9561
```

### 7. Run the Notebook

```bash
jupyter notebook multi-agent-service-matcher.ipynb
```

### 8. Activate the Agents

In the notebook, find the TODO comments and uncomment the LLM call sections:

**In Agent 1 (cell with `agent_1_interpreter`):**
```python
# Uncomment these lines:
model = GenerativeModel('gemini-1.5-flash')
response = model.generate_content(prompt)
clarified_request = response.text.strip()

# Comment out or delete the mock line:
# clarified_request = f"[MOCK] User needs: {user_request}"
```

**In Agent 2 (cell with `agent_2_matcher`):**
```python
# Uncomment these lines:
model = GenerativeModel('gemini-1.5-flash')
response = model.generate_content(prompt)
response_text = response.text.strip()
if response_text.startswith("```"):
    response_text = response_text.split("```")[1]
    if response_text.startswith("json"):
        response_text = response_text[4:]
matched_service = json.loads(response_text.strip())

# Comment out or delete the mock section
```

**In Agent 3 (cell with `agent_3_polisher`):**
```python
# Uncomment these lines:
model = GenerativeModel('gemini-1.5-flash')
response = model.generate_content(prompt)
final_response = response.text.strip()

# Comment out or delete the mock section
```

### 9. Test the System

Uncomment and run the example cells at the end of the notebook:

```python
match_service("my python code keeps crashing when i try to open a file help!!")
match_service("my kid needs help with sat math word problems")
match_service("i have questions about filing my taxes this year")
```

## Troubleshooting

### Error: "Could not automatically determine credentials"

**Solution:** Run the authentication command again:
```bash
gcloud auth application-default login
```

### Error: "Permission denied" or "403 Forbidden"

**Solution:** Make sure you have the right permissions in the Google Cloud project. You may need:
- `Vertex AI User` role
- `AI Platform User` role

Ask your project admin to grant these permissions.

### Error: "API not enabled"

**Solution:** Enable the Vertex AI API:
```bash
gcloud services enable aiplatform.googleapis.com --project=d-ulti-ml-ds-dev-9561
```

### Error: "Invalid region" or "Location not found"

**Solution:** Try changing the location in `.env` to:
```
GOOGLE_CLOUD_LOCATION=us-central1
```

Then restart your Jupyter notebook kernel.

### Error: "Model not found: gemini-1.5-flash"

**Solution:** Check available models in your region:
```bash
gcloud ai models list --region=us-central1
```

You might need to use `gemini-pro` instead:
```python
model = GenerativeModel('gemini-pro')
```

## Available Gemini Models

- `gemini-1.5-flash` - Fast, cost-effective (recommended for this project)
- `gemini-1.5-pro` - More capable, slower, more expensive
- `gemini-pro` - Previous generation, widely available

## Cost Considerations

Vertex AI Gemini models have usage costs:
- Gemini 1.5 Flash: ~$0.00001875 per 1K characters input, $0.000075 per 1K characters output
- Each agent call in this notebook uses ~500-1000 characters
- Running one complete pipeline (3 agents) costs approximately $0.0001 - $0.0003

This is very affordable for learning and testing!

## Next Steps

Once everything is working:

1. ✅ Test with the provided examples
2. ✅ Try your own custom requests
3. ✅ Experiment with different prompts
4. ✅ Add more services to the tool
5. ✅ Try the debugging/inspection features

## Getting Help

If you run into issues:

1. Check the error message carefully
2. Review the troubleshooting section above
3. Check Google Cloud Console for quota/permission issues
4. Verify your authentication with `gcloud auth list`

## Summary Checklist

- [ ] Google Cloud CLI installed
- [ ] Authenticated with `gcloud auth application-default login`
- [ ] Project set to `d-ulti-ml-ds-dev-9561`
- [ ] Vertex AI API enabled
- [ ] Python dependencies installed
- [ ] Jupyter notebook running
- [ ] LLM calls uncommented in agents
- [ ] Test examples working

Happy coding! 🚀
