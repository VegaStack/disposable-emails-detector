# Free Disposable Email Domains Detection API by VegaStack

[![Disposable Email Domains Blocklist Daily Update](https://github.com/VegaStack/disposable-emails-detector/actions/workflows/daily.yml/badge.svg)](https://github.com/VegaStack/disposable-emails-detector/actions/workflows/daily.yml)

Free API to detect and block disposable email domains. Automatically updated blocklist to reduce spam, improve security, and keep your app’s data clean.

## 📋 Overview

This repository maintains a consolidated list of over 100,000 disposable email domains that are commonly used for temporary email services, spam, and abuse. The list is automatically updated daily by merging data from multiple trusted public sources. Use the Free API to detect and block disposable email domains.

## 🌐 Try It Online

Test the disposable email detector with our web interface:

**🚀 [Live Demo: https://disposable-emails-detector.vegastack.com](https://disposable-emails-detector.vegastack.com)**

- **Instant Domain Checking**: Test any email domain or full email address
- **Real-time Results**: Powered by our optimized API with 100k+ domains
- **Shareable URLs**: Share specific domain checks with query parameters
- **Professional Interface**: Clean, fast, and mobile-friendly

You can also check domains directly via URL:
- `https://disposable-emails-detector.vegastack.com/?domain=10minutemail.com`
- `https://disposable-emails-detector.vegastack.com/?domain=gmail.com`

## 🎯 Purpose

- **Spam Prevention**: Block registrations from temporary email services
- **Data Quality**: Improve user database quality by filtering out disposable emails
- **Security**: Prevent abuse from users who create accounts with temporary emails
- **Marketing**: Ensure legitimate email addresses for marketing campaigns

## 📁 Files

- `disposable_email_domains_blocklist.txt` - The main blocklist file containing all disposable email domains (one per line, alphabetically sorted)
- `outputs/disposable_email_domains.json` - JSON format for easy integration with web applications
- `outputs/disposable_email_domains.csv` - CSV format for spreadsheet applications and data analysis
- `STATS.md` - Real-time statistics and status information
- `CHANGELOG.md` - Daily update history and changes

## 🔄 Automatic Updates

The blocklist is automatically updated daily via GitHub Actions, pulling from the following trusted sources:

- [disposable.github.io](https://disposable.github.io/disposable-email-domains/domains.txt)
- [disposable/disposable-email-domains](https://raw.githubusercontent.com/disposable/disposable-email-domains/master/domains.txt)
- [StopForumSpam toxic domains](https://www.stopforumspam.com/downloads/toxic_domains_whole.txt)
- [disposable-email-domains/disposable-email-domains](https://raw.githubusercontent.com/disposable-email-domains/disposable-email-domains/master/disposable_email_blocklist.conf)

The workflow automatically:
1. Downloads lists from all sources with retry logic and error handling
2. Validates data integrity and minimum thresholds
3. Merges them into a single file
4. Removes duplicates and sorts alphabetically
5. Generates multiple output formats (TXT, JSON, CSV)
6. Creates statistics and changelog files
7. Commits the updated files to the repository

## 🚀 Usage

### Quick Start - API Usage

The fastest way to check disposable emails:

```javascript
// JavaScript/Node.js
const checkEmail = async (email) => {
  const domain = email.split('@')[1];
  const response = await fetch('https://disposable-emails-detector.vegastack.com/api/check.json');
  const api = await response.json();
  return api.domains[domain] === 1;
};

// Usage
if (await checkEmail('user@10minutemail.com')) {
  console.log('Disposable email detected!');
}
```

```python
# Python
import requests

def is_disposable(email):
    domain = email.split('@')[1]
    response = requests.get('https://disposable-emails-detector.vegastack.com/api/check.json')
    return response.json()['domains'].get(domain, 0) == 1

# Usage
if is_disposable('user@tempmail.org'):
    print('Disposable email detected!')
```

### Download the List

You can download the latest blocklist in multiple formats:

```bash
# Text format (main file)
curl -o disposable_emails.txt https://disposable-emails-detector.vegastack.com/disposable_email_domains_blocklist.txt

# JSON format
curl -o disposable_emails.json https://disposable-emails-detector.vegastack.com/outputs/disposable_email_domains.json

# CSV format
curl -o disposable_emails.csv https://disposable-emails-detector.vegastack.com/outputs/disposable_email_domains.csv

# Statistics and changelog
curl -o stats.md https://disposable-emails-detector.vegastack.com/STATS.md
curl -o changelog.md https://disposable-emails-detector.vegastack.com/CHANGELOG.md
```

### 🚀 **API Endpoints**

For real-time domain checking with optimal performance:

```bash
# Main API - Domain checking with hash lookup
curl https://disposable-emails-detector.vegastack.com/api/check.json

# Statistics API - Current stats and metadata
curl https://disposable-emails-detector.vegastack.com/api/stats.json

# API Information - Documentation and usage
curl https://disposable-emails-detector.vegastack.com/api/info.json
```

📖 **[Complete API Documentation](API.md)**

### Programming Examples

#### Python
```python
# Load the blocklist (pre-sorted for optimal performance)
with open('disposable_email_domains_blocklist.txt', 'r') as f:
    disposable_domains = set(line.strip() for line in f)

def is_disposable_email(email):
    domain = email.split('@')[-1].lower()
    return domain in disposable_domains

# Usage
if is_disposable_email("user@10minutemail.com"):
    print("Disposable email detected!")
```

**Using API (Recommended):**
```python
import requests

def is_disposable_email(email):
    """Check if email domain is disposable using the API"""
    domain = email.split('@')[-1].lower()
    
    try:
        response = requests.get('https://disposable-emails-detector.vegastack.com/api/check.json', timeout=5)
        response.raise_for_status()
        api_data = response.json()
        return api_data['domains'].get(domain, 0) == 1
    except requests.RequestException as error:
        print(f"API error: {error}")
        return False  # Fallback to allow email if API fails

# Usage
if is_disposable_email("user@10minutemail.com"):
    print("Disposable email detected!")
```

**Using JSON Format:**
```python
import json

# Load the JSON blocklist
with open('outputs/disposable_email_domains.json', 'r') as f:
    blocklist = json.load(f)
    disposable_domains = set(blocklist['domains'])

def is_disposable_email(email):
    domain = email.split('@')[-1].lower()
    return domain in disposable_domains

# Usage
if is_disposable_email("user@10minutemail.com"):
    print("Disposable email detected!")
```

#### JavaScript/Node.js

**Using Text Format:**
```javascript
const fs = require('fs');

// Load the blocklist (pre-sorted for optimal performance)
const disposableDomains = new Set(
    fs.readFileSync('disposable_email_domains_blocklist.txt', 'utf8')
      .split('\n')
      .map(line => line.trim())
      .filter(line => line.length > 0)
);

function isDisposableEmail(email) {
    const domain = email.split('@').pop().toLowerCase();
    return disposableDomains.has(domain);
}

// Usage
if (isDisposableEmail("user@guerrillamail.com")) {
    console.log("Disposable email detected!");
}
```

**Using API (Recommended):**
```javascript
// Real-time API checking - most efficient for single checks
const checkDisposableEmail = async (email) => {
  const domain = email.split('@').pop().toLowerCase();
  
  try {
    const response = await fetch('https://disposable-emails-detector.vegastack.com/api/check.json');
    const api = await response.json();
    return api.domains[domain] === 1;
  } catch (error) {
    console.error('API error:', error);
    return false; // Fallback to allow email if API fails
  }
};

// Usage
const isDisposable = await checkDisposableEmail("user@guerrillamail.com");
if (isDisposable) {
    console.log("Disposable email detected!");
}
```

**Using JSON Format:**
```javascript
const fs = require('fs');

// Load the JSON blocklist
const blocklist = JSON.parse(fs.readFileSync('outputs/disposable_email_domains.json', 'utf8'));
const disposableDomains = new Set(blocklist.domains);

function isDisposableEmail(email) {
    const domain = email.split('@').pop().toLowerCase();
    return disposableDomains.has(domain);
}

// Usage
if (isDisposableEmail("user@guerrillamail.com")) {
    console.log("Disposable email detected!");
}
```

#### PHP

**Using API (Recommended):**
```php
<?php
function isDisposableEmail($email) {
    $domain = strtolower(substr(strrchr($email, '@'), 1));
    
    // Use cURL for better error handling
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, 'https://disposable-emails-detector.vegastack.com/api/check.json');
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_TIMEOUT, 5);
    curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
    
    $response = curl_exec($ch);
    $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
    curl_close($ch);
    
    if ($httpCode === 200 && $response !== false) {
        $apiData = json_decode($response, true);
        return isset($apiData['domains'][$domain]) && $apiData['domains'][$domain] === 1;
    }
    
    return false; // Fallback to allow email if API fails
}

// Usage
if (isDisposableEmail("user@mailinator.com")) {
    echo "Disposable email detected!";
}
?>
```

**Using Text File:**
```php
<?php
// Load the blocklist
$disposableDomains = array_flip(
    array_filter(
        array_map('trim', file('disposable_email_domains_blocklist.txt')),
        'strlen'
    )
);

function isDisposableEmail($email) {
    global $disposableDomains;
    $domain = strtolower(substr(strrchr($email, '@'), 1));
    return isset($disposableDomains[$domain]);
}

// Usage
if (isDisposableEmail("user@mailinator.com")) {
    echo "Disposable email detected!";
}
?>
```

## 📊 Statistics

For real-time statistics, see the auto-generated **[STATS.md](STATS.md)** file, which includes:
- Current total domain count
- Last update timestamp
- Change from previous update
- File format availability
- Source status information

For historical changes, check the **[CHANGELOG.md](CHANGELOG.md)** file.

## 🤝 Contributing

This repository is automatically maintained, but contributions are welcome:

1. **Report Missing Domains**: Use our [issue template](.github/ISSUE_TEMPLATE/missing-domain.md) to report disposable domains not in the list
2. **Report False Positives**: Use our [issue template](.github/ISSUE_TEMPLATE/false-positive.md) to report legitimate domains incorrectly blocked
3. **Bug Reports**: Use our [issue template](.github/ISSUE_TEMPLATE/bug-report.md) for technical issues
4. **Suggest New Sources**: Propose additional trusted sources for inclusion
5. **Improve Documentation**: Help improve this README or add examples

## 🔒 Security & Performance

Based on [API security best practices](https://dev.to/zuplo/8-essential-api-security-best-practices-47fk), this service includes:

- **HTTPS Only**: All requests encrypted with TLS 1.3
- **CORS Enabled**: Cross-origin requests supported for web applications
- **Security Headers**: Protection against XSS, clickjacking, and content-type sniffing
- **Rate Limiting**: 100GB/month bandwidth limit (generous for most use cases)
- **Input Validation**: Domain validation at server level
- **No Authentication Required**: Public API for community use
- **Error Handling**: Comprehensive error handling with fallback logic
- **Global CDN**: Cloudflare hosting with 99.9% uptime guarantee

## ⚠️ Important Notes

- **API Recommended**: Use `https://disposable-emails-detector.vegastack.com/api/check.json` for optimal performance and real-time checking
- **Performance**: Domains are pre-sorted alphabetically for optimal lookup performance. API provides O(1) hash-based lookups
- **Multiple Formats**: Use API for web applications, JSON format for bulk processing, CSV for data analysis, or TXT for simple scripts
- **Reliability**: Hosted on Cloudflare with 99.9% uptime, automatic SSL, and global CDN
- **Error Handling**: All API examples include proper error handling and fallback logic
- **False Positives**: Some legitimate services might be included; use our [issue template](.github/ISSUE_TEMPLATE/false-positive.md) to report them
- **Regular Updates**: The list is updated daily with automatic validation and error handling

## 📜 License

This project is licensed under the [MIT License](LICENSE) - see the LICENSE file for details.

The consolidated blocklist data is provided as-is for the benefit of the community. Please check individual source licenses for specific terms regarding the original data sources.

## 🔗 Related Projects

- [disposable/disposable-email-domains](https://github.com/disposable/disposable-email-domains)
- [StopForumSpam](https://www.stopforumspam.com/)

## 🎧 Support

If you encounter issues or have questions:
- Open an [issue](../../issues) in this repository
- Check existing issues for similar problems
- Provide details about your use case for better assistance

---

⭐ **Star this repository** if you find it useful for your projects! 
