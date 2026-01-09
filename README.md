# Weather Dashboard - Project Documentation

## Overview

Weather Dashboard is a modern, responsive web application that displays real-time weather information based on the user's location or a manually entered city name. The application features an intuitive interface with dynamic backgrounds that change based on weather conditions.

## Features

- **Automatic Location Detection**: Uses the browser's geolocation API to automatically detect and display weather for the user's current location
- **Manual City Search**: Allows users to search for weather information by entering any city name
- **Real-time Weather Data**: Fetches current weather data from OpenWeatherMap API
- **Dynamic UI**: Background colors change based on weather conditions (clear, cloudy, rainy, snowy)
- **Comprehensive Weather Information**: Displays temperature, feels-like temperature, humidity, wind speed, and cloudiness
- **Responsive Design**: Works seamlessly on desktop and mobile devices
- **Error Handling**: Provides clear error messages and fallback options

## Technology Stack

### Backend
- **Flask 3.1.0**: Python web framework for handling API routes and serving the application
- **Requests 2.32.3**: HTTP library for making API calls to OpenWeatherMap
- **Python-dotenv 1.0.1**: Environment variable management for secure API key storage
- **Werkzeug 3.1.3**: WSGI utility library (Flask dependency)

### Frontend
- **HTML5**: Semantic markup structure
- **CSS3**: Modern styling with gradients, animations, and responsive design
- **Vanilla JavaScript**: Dynamic interactions and API communication
- **Browser Geolocation API**: Automatic location detection

### External API
- **OpenWeatherMap API**: Weather data provider

## Project Structure

```
weather-dashboard/
│
├── app.py                  # Main Flask application
├── requirements.txt        # Python dependencies
├── .env                    # Environment variables (not in repo)
├── templates/
│   └── index.html         # Main HTML template
└── README.md              # Project documentation
```

## Installation & Setup

### Prerequisites
- Python 3.7 or higher
- pip (Python package manager)
- OpenWeatherMap API key (free tier available)

### Step 1: Clone or Download the Project

```bash
git clone <repository-url>
cd weather-dashboard
```

### Step 2: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 3: Get OpenWeatherMap API Key

1. Visit [OpenWeatherMap](https://openweathermap.org/api)
2. Sign up for a free account
3. Navigate to API keys section
4. Generate a new API key

### Step 4: Configure Environment Variables

Create a `.env` file in the project root directory:

```bash
touch .env
```

Add your API key to the `.env` file:

```
OPENWEATHER_API_KEY=your_api_key_here
```

**Important**: Never commit your `.env` file to version control. Add it to `.gitignore`.

### Step 5: Run the Application

```bash
python app.py
```

The application will start on `http://127.0.0.1:5000`

## API Endpoints

### GET `/`
Returns the main dashboard HTML page.

**Response**: HTML page

---

### POST `/api/weather/coords`
Fetches weather data based on geographic coordinates.

**Request Body**:
```json
{
  "lat": 40.7128,
  "lon": -74.0060
}
```

**Success Response** (200):
```json
{
  "name": "New York",
  "sys": { "country": "US" },
  "weather": [
    { "main": "Clear", "description": "clear sky" }
  ],
  "main": {
    "temp": 22.5,
    "feels_like": 21.8,
    "humidity": 60
  },
  "wind": { "speed": 3.5 },
  "clouds": { "all": 10 }
}
```

**Error Responses**:
- 400: Missing latitude or longitude
- 504: Request timeout
- 503: Network error

---

### POST `/api/weather/city`
Fetches weather data based on city name.

**Request Body**:
```json
{
  "city": "London"
}
```

**Success Response** (200): Same format as `/api/weather/coords`

**Error Responses**:
- 400: Missing city name
- 404: City not found
- 504: Request timeout
- 503: Network error

## Frontend Features

### Dynamic Backgrounds

The application changes its background gradient based on weather conditions:

- **Clear**: Orange to red gradient (sunny atmosphere)
- **Clouds**: Gray to dark blue gradient (overcast feeling)
- **Rain**: Blue gradient (rainy mood)
- **Snow**: Light blue gradient (winter feel)
- **Default**: Purple gradient (neutral state)

### User Flow

1. **Initial Load**: Application attempts to detect user's location automatically
2. **Location Permission Granted**: Fetches and displays weather for current location
3. **Location Permission Denied**: Shows manual search input for city entry
4. **Manual Search**: User can search for any city worldwide
5. **Refresh**: User can click "Refresh Location" to re-detect current location

### Loading States

- Spinner animation during data fetch
- Loading message for user feedback
- Smooth transitions between states

### Error Handling

The application handles various error scenarios:

- Geolocation not supported
- Location permission denied
- Location unavailable
- Request timeout
- City not found
- Network errors
- Server errors

## Weather Data Display

### Main Information
- City name and country code
- Weather description (e.g., "clear sky", "light rain")
- Current temperature in Celsius
- Feels-like temperature

### Weather Details (Grid Cards)
1. **Humidity**: Percentage of moisture in the air
2. **Wind Speed**: Measured in meters per second
3. **Cloudiness**: Percentage of cloud cover

## Customization

### Changing Temperature Units

To switch from Celsius to Fahrenheit, modify the API request in both `app.py` endpoints:

```python
params = {
    # ... other params
    'units': 'imperial'  # Change from 'metric' to 'imperial'
}
```

And update the HTML template to display °F instead of °C.

### Styling Customization

All styles are contained in the `<style>` tag in `index.html`. Key customizable elements:

- **Colors**: Modify gradient values in body classes
- **Border Radius**: Change `border-radius` values for different roundness
- **Spacing**: Adjust `padding` and `margin` values
- **Font Sizes**: Modify `font-size` properties

## Security Considerations

1. **API Key Protection**: Store API keys in `.env` file, never in code
2. **Input Validation**: Server validates all incoming requests
3. **HTTPS**: Use HTTPS in production to protect data in transit
4. **Rate Limiting**: Consider implementing rate limiting for API endpoints
5. **CORS**: Configure Cross-Origin Resource Sharing for production

## Deployment

### For Production Deployment:

1. **Set Environment Variables**: Configure `OPENWEATHER_API_KEY` in your hosting environment
2. **Update Flask Settings**: Change `debug=False` and configure production server
3. **Use Production WSGI Server**: Use Gunicorn or uWSGI instead of Flask development server
4. **Configure HTTPS**: Set up SSL certificates
5. **Set Proper Host/Port**: Update host and port settings for your environment

Example with Gunicorn:

```bash
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:8000 app:app
```

## Troubleshooting

### Issue: API Key Not Found
**Solution**: Verify `.env` file exists and contains `OPENWEATHER_API_KEY=your_key`

### Issue: Location Detection Fails
**Solution**: Ensure HTTPS is used (required for geolocation API) or use manual city search

### Issue: City Not Found
**Solution**: Check city name spelling, try including country code (e.g., "London,UK")

### Issue: Emojis Not Displaying
**Solution**: Ensure UTF-8 encoding in HTML meta tag (already included)

## Browser Support

- Chrome/Edge: Full support
- Firefox: Full support
- Safari: Full support
- Mobile browsers: Full support (iOS Safari, Chrome Mobile)

## Contributing

When contributing to this project:

1. Keep the code clean and well-commented
2. Test all changes across different browsers
3. Ensure error handling is comprehensive
4. Update documentation for any new features
5. Never commit API keys or sensitive data

## License

This project is open source and available for educational and personal use.

## Credits

- Weather data provided by [OpenWeatherMap](https://openweathermap.org/)
- Icons: Unicode emojis for cross-platform compatibility

## Future Enhancements

Potential features for future development:

- 5-day weather forecast
- Weather alerts and notifications
- Multiple location favorites
- Temperature unit toggle (Celsius/Fahrenheit)
- Weather maps integration
- Historical weather data
- Social sharing features
- Dark/light theme toggle
- Additional weather metrics (UV index, air quality)
- Internationalization (multiple languages)
