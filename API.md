# Disposable Email Detection API

**Base URL:** `https://disposable-emails-detector.vegastack.com`

## Endpoints

### GET /api/check.json

Main API endpoint for checking disposable email domains with optimized hash-based lookup.

**Response Format:**
```json
{
  "v": "1.0",
  "updated": "2024-01-15T00:00:00Z",
  "count": 108851,
  "domains": {
    "10minutemail.com": 1,
    "tempmail.org": 1,
    "...": "..."
  }
}
```

**Field Descriptions:**
- `v` - API version
- `updated` - Last update timestamp (ISO 8601 UTC)
- `count` - Total number of domains in blocklist
- `domains` - Hash table of disposable domains (value = 1 if disposable)

### GET /api/stats.json

Statistics and metadata about the current blocklist.

**Response Format:**
```json
{
  "v": "1.0",
  "endpoint": "https://disposable-emails-detector.vegastack.com",
  "count": 108851,
  "updated": "2024-01-15T00:00:00Z",
  "sources": [4],
  "formats": ["txt", "json", "csv", "api"],
  "rate_limit": "100GB/month"
}
```

## Usage Examples

### JavaScript/Node.js
```javascript
const checkDisposableEmail = async (email) => {
  const domain = email.split('@')[1].toLowerCase();
  
  try {
    const response = await fetch('https://disposable-emails-detector.vegastack.com/api/check.json');
    const api = await response.json();
    return api.domains[domain] === 1;
  } catch (error) {
    console.error('API error:', error);
    return false; // Fallback
  }
};

// Check single email
const isDisposable = await checkDisposableEmail('user@10minutemail.com'); // true
```

### Python
```python
import requests

def is_disposable_email(email):
    domain = email.split('@')[1].lower()
    
    try:
        response = requests.get(
            'https://disposable-emails-detector.vegastack.com/api/check.json',
            timeout=5
        )
        response.raise_for_status()
        api_data = response.json()
        return api_data['domains'].get(domain, 0) == 1
    except requests.RequestException:
        return False  # Fallback

# Check single email
is_disposable = is_disposable_email('user@tempmail.org')  # True
```

### PHP
```php
function isDisposableEmail($email) {
    $domain = strtolower(substr(strrchr($email, '@'), 1));
    
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, 'https://disposable-emails-detector.vegastack.com/api/check.json');
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_TIMEOUT, 5);
    
    $response = curl_exec($ch);
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    curl_close($ch);
    
    if ($httpCode === 200 && $response !== false) {
        $apiData = json_decode($response, true);
        return isset($apiData['domains'][$domain]) && $apiData['domains'][$domain] === 1;
    }
    
    return false; // Fallback
}
```

### GET /api/info.json

API documentation and usage information.

**Response Format:**
```json
{
  "api": {
    "version": "1.0",
    "name": "Disposable Email Domains API",
    "description": "Fast, reliable disposable email detection"
  },
  "endpoints": {
    "check": "/api/check.json",
    "stats": "/api/stats.json", 
    "info": "/api/info.json"
  },
  "usage": {
    "check_domain": "api.domains[\"example.com\"] === 1",
    "get_count": "api.count",
    "last_updated": "api.updated"
  }
}
```

## Performance

- **Response Time:** < 100ms globally (Cloudflare CDN)
- **Uptime:** 99.9% availability  
- **Rate Limits:** 100GB/month bandwidth (generous for most use cases)
- **Lookup Method:** O(1) hash table for instant domain checking
- **Caching:** Responses cached globally for optimal performance
- **Payload Size:** ~30% smaller than previous version (optimized JSON structure)

## Security & Rate Limiting

Based on [API security best practices](https://dev.to/zuplo/8-essential-api-security-best-practices-47fk), this API includes:

- **CORS Enabled:** Cross-origin requests allowed from all domains
- **Security Headers:** X-Content-Type-Options, X-Frame-Options, X-XSS-Protection
- **HTTPS Only:** All requests must use HTTPS
- **Rate Limiting:** 100GB/month bandwidth limit (monitored by GitHub Pages)
- **Input Validation:** Domain validation at server level
- **No Authentication Required:** Public API for community use

## Error Handling

Always implement proper error handling:

1. **Network Errors:** API may be temporarily unavailable
2. **Timeouts:** Set reasonable timeout values (5 seconds recommended)
3. **Fallback Logic:** Consider allowing emails if API fails (avoid blocking legitimate users)
4. **Caching:** Cache API responses locally for better performance
5. **Status Codes:** Handle HTTP 429 (rate limit exceeded) gracefully

## Alternative Formats

If you prefer downloading files instead of using the API:

- **JSON:** `/outputs/disposable_email_domains.json`
- **CSV:** `/outputs/disposable_email_domains.csv`
- **Text:** `/disposable_email_domains_blocklist.txt`

## Updates

The blocklist is updated daily at midnight UTC with domains from multiple trusted sources:
- disposable.github.io
- StopForumSpam
- Multiple other verified sources

## Support

- **Issues:** [GitHub Issues](../../issues)
- **Documentation:** [README.md](README.md)
- **Source Code:** [GitHub Repository](../../) 