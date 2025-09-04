# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository maintains a JSON configuration file (`ai-engines.json`) that WordPress plugins can fetch to discover available AI engines and their capabilities. The file is hosted on GitHub and accessed via raw.githubusercontent.com.

## Regular Maintenance Tasks

### Updating AI Engine Information (Quarterly)

Every ~3 months, update the `ai-engines.json` file with the latest information from each AI provider:

**IMPORTANT: Process engines ONE AT A TIME, not all at once. This prevents overwhelming changes and allows for better validation.**

1. **Research New Engines First**
   - Search for new AI providers that have emerged
   - Check if any should be added to the JSON
   - Note any providers that have been discontinued

2. **For Each Engine (Process ONE at a time):**
   
   **Step 1: Research and Verify**
   - Use web search to find current API documentation
   - Verify the base URL and endpoint structure
   - Check for new/deprecated models
   - Look for capability changes
   
   **Step 2: Validate Current Data**
   - Test model names against official documentation
   - Verify endpoint URLs are correct
   - Check authentication requirements
   - Confirm context windows, token limits, and features
   
   **Step 3: Update JSON**
   - Update ONLY the current engine being processed
   - Commit changes for this single engine
   - Test the updated JSON before proceeding to next engine
   
   **Engine Priority Order:**
   - **OpenAI**: Chat completions, embeddings, image generation/editing, audio (TTS/STT), latest GPT models
   - **Anthropic**: Messages API, latest Claude models, new features (code execution, file handling)
   - **Google AI**: Gemini models, embedding support, both direct API and Vertex AI endpoints
   - **Mistral AI**: Latest models including specialized ones (OCR, code), embedding capabilities
   - **Groq**: Fast inference models, vision models, batch processing capabilities
   - **xAI (Grok)**: Latest Grok models (4, 3, 2), tool use, live search integration, structured outputs
   - **Perplexity AI**: Sonar models, real-time search, citations, pricing tiers
   - **Other providers**: Cohere, DeepSeek, Together AI, Fireworks AI

3. **Update Structure Elements:**
   - `capabilities`: What the engine can do (text_generation, image_generation, embeddings, etc.)
   - `models`: Current model IDs, context windows, output limits, vision/function calling support
   - `endpoints`: API endpoints for each capability
   - `authentication`: How to authenticate (bearer token, API key header, etc.)

4. **Version and Date**
   - Update the `last_updated` field to current date only after ALL engines are processed
   - Consider incrementing version if making structural changes

## JSON Structure Reference

```json
{
  "version": "1.0.0",
  "last_updated": "YYYY-MM-DD",
  "engines": [
    {
      "id": "unique-identifier",
      "name": "Display Name",
      "status": "active|deprecated",
      "capabilities": {
        "text_generation": boolean,
        "image_generation": boolean,
        "image_editing": boolean,
        "embeddings": boolean,
        "speech_to_text": boolean,
        "text_to_speech": boolean,
        "tool_use": boolean,
        "live_search": boolean,
        "real_time_search": boolean,
        "citations": boolean,
        // Additional capabilities as needed
      },
      "models": [
        {
          "model_id": "api-model-name",
          "name": "Human Readable Name",
          "type": "chat|embedding|ocr|code",
          "context_window": number,
          "max_output_tokens": number,
          "supports_vision": boolean,
          "supports_function_calling": boolean,
          "reasoning_model": boolean,
          "search_enabled": boolean,
          "knowledge_cutoff": "YYYY-MM",
          // Model-specific fields
        }
      ],
      "endpoints": {
        "base_url": "https://api.example.com",
        // Endpoint paths for each capability
      },
      "authentication": {
        "type": "bearer|header|query",
        "header": "header-name",
        "parameter": "param-name",
        "prefix": "Bearer"
      }
    }
  ]
}
```

## WordPress Plugin Usage

Plugins fetch the configuration from:
```
https://raw.githubusercontent.com/nholzmann/AIEngines/main/ai-engines.json
```

Example PHP code for plugins:
```php
function get_ai_engines() {
    $response = wp_remote_get('https://raw.githubusercontent.com/nholzmann/AIEngines/main/ai-engines.json');
    if (is_wp_error($response)) {
        return false;
    }
    $body = wp_remote_retrieve_body($response);
    return json_decode($body, true);
}

// Check capabilities
if ($engine['capabilities']['image_generation']) {
    // This engine can generate images
}

// Check for real-time search capability
if ($engine['capabilities']['real_time_search'] || $engine['capabilities']['live_search']) {
    // This engine has real-time search capabilities
}
```

## Testing Changes

Before committing:
1. Validate JSON syntax
2. Ensure all required fields are present
3. Verify model IDs match current documentation
4. Check that endpoints are correctly formatted