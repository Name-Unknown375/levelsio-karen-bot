# 💁‍♀️ Karen Bot by @levelsio

An AI-powered web application that helps you report issues to your city council by transforming informal complaints into formal letters.

Originally built for Lisbon, Portugal, but can be adapted to **any city and any language**!

More info: https://x.com/levelsio/status/2009011216132526407?s=20

## Features

- 📍 **Interactive Map**: Click to select the exact location of the problem
- 📸 **Photo Attachments**: Upload images as evidence (automatically resized to ~1MB)
- 🤖 **AI Letter Writing**: Uses OpenAI GPT to transform casual complaints into formal letters
- 📧 **Email Integration**: Sends letters directly to your city council via Postmark
- 🌍 **Multi-language Support**: Works with any language and city
- 🗺️ **Dual Map Views**: Switch between street and satellite views

## Prerequisites

- PHP 7.4 or higher
- Web server (Apache/Nginx)
- [Postmark](https://postmarkapp.com/) account for sending emails
- [OpenAI API](https://platform.openai.com/) key

## Installation

### 1. Upload the File

Upload the `💁‍♀️ Karen Bot by @levelsio` file to your web server.

### 2. Rename the File (Recommended)

Rename it to something easier to work with, like `council.php`:

```bash
mv "💁‍♀️ Karen Bot by @levelsio" council.php
```

### 3. Configure Your Settings

Open the file and edit the configuration section at the top (lines 12-43):

#### Required Settings:

```php
// Access key for the script
define('KEY_TO_ACCESS_THE_SCRIPT', 'your-secret-key-here');

// API Keys
define('POSTMARK_API_KEY', 'your-postmark-api-key');
define('OPENAI_API_KEY', 'your-openai-api-key');

// Email configuration
define('YOUR_NAME', 'John Doe');
define('FROM_YOUR_EMAIL', 'you@youremail.com');
define('TO_COUNCIL_EMAIL', 'complaints@yourcitycouncil.gov');
define('CC_EMAILS', 'yourself@email.com');  // Optional
```

#### City Settings:

**To change the city**, update these settings:

```php
// Map center coordinates
// Find your city's coordinates: Go to Google Maps, right-click your city center,
// and copy the latitude/longitude
define('MAP_CENTER_LAT', 40.7128);   // Example: New York
define('MAP_CENTER_LNG', -74.0060);  // Example: New York

// City information
define('CITY_NAME', 'New York');
define('COUNCIL_NAME', 'New York City Council');
define('COUNTRY_FLAG', '🇺🇸');
define('LANGUAGE', 'English');
```

**Example coordinates for different cities:**
- **Lisbon**: `38.734674, -9.16427`
- **New York**: `40.7128, -74.0060`
- **London**: `51.5074, -0.1278`
- **Tokyo**: `35.6762, 139.6503`
- **Paris**: `48.8566, 2.3522`
- **Sydney**: `-33.8688, 151.2093`

### 4. Set Up Web Server Route

#### For Nginx:

Add this to your Nginx configuration:

```nginx
location /council {
    try_files $uri /council.php?$args;
}
```

#### For Apache:

If using `.htaccess`, add:

```apache
RewriteEngine On
RewriteRule ^council$ council.php [L,QSA]
```

### 5. Set Permissions

Make sure the PHP file has the correct permissions:

```bash
chmod 644 council.php
```

## Usage

### Accessing the App

Visit your app at:
```
https://yourdomain.com/council?key=your-secret-key-here
```

**Important**: The `?key=` parameter is required for security. Only people with your secret key can access the app.

### How to Report an Issue

1. **Set Your Location**:
   - Click "Center on my location" to center the map on your current position
   - Click on the map where the problem is located
   - The address will appear automatically

2. **Upload Photos** (Optional):
   - Click the upload box or drag & drop images
   - Multiple photos are supported
   - Images are automatically resized to ~1MB for email compatibility

3. **Write Your Complaint**:
   - Type your complaint in the "Your complaint" field
   - Be informal and casual - the AI will formalize it!
   - Example: "There's trash piled up next to the container for 3 days and it smells terrible"

4. **Generate Formal Letter**:
   - Click "Write letter"
   - The AI will transform your complaint into a formal letter in your configured language
   - If not using English, you'll see both the native language letter and an English translation

5. **Send the Complaint**:
   - Review the generated letter
   - Click "Send complaint"
   - The letter will be emailed to your city council with photos attached

## Customization

### Changing the Language

To use a different language, update the `LANGUAGE` constant and the AI will automatically write letters in that language:

```php
define('LANGUAGE', 'Spanish');  // or French, German, Japanese, etc.
```

### Customizing Letter Format

The letter format is controlled by the `$systemPrompt` variable (starting around line 86). You can customize:
- The formal greeting style
- The letter structure
- The closing signature
- What information to include

### Email Service

By default, this uses [Postmark](https://postmarkapp.com/) for sending emails. To use a different email service:

1. Locate the email sending code (around line 282)
2. Replace the Postmark API call with your preferred email service API

## Troubleshooting

### "Not found" error
- Make sure you're including the `?key=` parameter in the URL
- Check that the key matches `KEY_TO_ACCESS_THE_SCRIPT`

### Email not sending
- Verify your Postmark API key is correct
- Check that your Postmark account is active
- Make sure `FROM_YOUR_EMAIL` is verified in Postmark

### Map not loading
- Check that the coordinates are correct (latitude, longitude format)
- Verify you have internet connection (map tiles load from external sources)

### AI not generating letters
- Verify your OpenAI API key is correct
- Check that you have credits in your OpenAI account
- Make sure the OpenAI API is accessible from your server

### Upload size limits
- Default max upload is 50MB (configured in lines 46-48)
- Images are automatically resized to ~1MB each
- Check your PHP configuration for `upload_max_filesize` and `post_max_size`

## Security Notes

- Always use a strong, unique key for `KEY_TO_ACCESS_THE_SCRIPT`
- Never commit API keys to version control
- Consider restricting access by IP if needed
- The script includes basic access protection via the key parameter

## Credits

Created by [@levelsio](https://twitter.com/levelsio)

## License

Use freely and adapt to your city! 🌍
