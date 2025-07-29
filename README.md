# AI Engines Configuration

This repository provides a centralized configuration file for WordPress plugins to discover available AI engines and their capabilities.

## Usage

WordPress plugins can fetch the configuration directly from GitHub:

```
https://raw.githubusercontent.com/[yourusername]/AIEngines/main/ai-engines.json
```

## File Structure

The JSON file contains:

- **version**: Configuration file version
- **last_updated**: Last update date
- **engines**: Array of available AI providers
  - Each engine includes:
    - Basic info (id, name, status)
    - Available models with capabilities
    - API endpoints
    - Authentication method

## Example Usage in WordPress

```php
function get_ai_engines() {
    $response = wp_remote_get('https://raw.githubusercontent.com/[yourusername]/AIEngines/main/ai-engines.json');
    
    if (is_wp_error($response)) {
        return false;
    }
    
    $body = wp_remote_retrieve_body($response);
    return json_decode($body, true);
}
```

## Model Capabilities

Each model includes:
- `model_id`: Unique identifier for API calls
- `name`: Human-readable name
- `type`: Model type (chat, completion, etc.)
- `context_window`: Maximum input tokens
- `max_output_tokens`: Maximum response tokens
- `supports_vision`: Image input capability
- `supports_function_calling`: Function/tool calling support

## Contributing

To update AI engine information:
1. Fork this repository
2. Update `ai-engines.json`
3. Submit a pull request

## License

MIT License