---
title: "Weather CLI Tool"
date: 2024-02-20
draft: true
tags: ["python", "cli", "api"]
summary: "A command-line tool for checking weather forecasts with a clean, colorful interface."
---

## Overview

A Python command-line tool that fetches and displays weather information for any city. My first project working with external APIs and building a CLI application.

**GitHub:** [github.com/yourusername/weather-cli](https://github.com/yourusername/weather-cli)

## Motivation

I wanted to check the weather without opening a browser or dealing with ad-filled weather sites. Plus, I wanted to learn how to build CLI tools and work with APIs.

## Tech Stack

- **Language:** Python 3.10
- **API:** OpenWeatherMap API
- **Libraries:** 
  - `requests` for API calls
  - `click` for CLI interface
  - `rich` for colored terminal output
  - `python-dotenv` for environment variables

## Features

```bash
# Basic usage
$ weather Seattle
Seattle, US
Temperature: 52°F (11°C)
Conditions: Partly cloudy
Humidity: 65%
Wind: 8 mph NW

# 5-day forecast
$ weather Seattle --forecast

# Different units
$ weather Tokyo --units metric
```

Key features:
- Current weather for any city
- 5-day forecast option
- Support for metric and imperial units
- Colorful, easy-to-read output
- Error handling for invalid cities
- Caching to reduce API calls

## Implementation Details

### API Integration
```python
def get_weather(city, api_key, units='imperial'):
    base_url = "http://api.openweathermap.org/data/2.5/weather"
    params = {
        'q': city,
        'appid': api_key,
        'units': units
    }
    response = requests.get(base_url, params=params)
    response.raise_for_status()
    return response.json()
```

### Error Handling
I learned to handle various error cases:
- Invalid city names (404 errors)
- Network connectivity issues
- API rate limits
- Missing API keys

### Configuration Management
Used environment variables for the API key:
```bash
# .env file
OPENWEATHER_API_KEY=your_api_key_here
```

This taught me about keeping secrets out of code and configuration best practices.

## What I Learned

### Working with APIs
- Reading API documentation
- Understanding rate limits and quotas
- Handling JSON responses
- Error codes and status handling

### CLI Design
- Argument parsing with `click`
- Creating intuitive command interfaces
- Providing helpful error messages
- Making output readable and attractive

### Python Best Practices
- Virtual environments
- Requirements.txt for dependencies
- Project structure and organization
- Writing docstrings and comments

## Challenges

**Challenge 1: API Key Security**  
Initially hardcoded the API key (bad!). Learned about environment variables and `.gitignore` to keep secrets safe.

**Challenge 2: Rate Limiting**  
Hit API rate limits during testing. Implemented simple caching to store recent queries for 10 minutes.

**Challenge 3: Terminal Colors**  
Different terminals display colors differently. The `rich` library solved this with cross-platform support.

## Results

- ✅ Fully functional CLI tool
- 📦 Published to GitHub with clear documentation
- 🎨 Clean, colorful output that's easy to read
- 🔒 Secure handling of API keys
- 📚 Learned fundamentals of API integration

## Usage Stats

After sharing with classmates:
- 12 GitHub stars
- 3 people using it regularly
- 2 feature requests (added to roadmap)

## Future Enhancements

- Add weather alerts and warnings
- Support for multiple cities at once
- Historical weather data
- ASCII art weather icons
- Package for PyPI distribution

## Takeaways

This project proved that even small tools can be valuable. I use this daily and it's satisfying to use something I built. The project taught me that:

1. **Start small:** A simple weather checker was perfect for learning APIs
2. **User experience matters:** Even CLI tools benefit from good design
3. **Documentation is crucial:** Clear README helped others use and contribute
4. **Security from day one:** Learning proper secret management early is important

The complete code and installation instructions are available on [GitHub](https://github.com/yourusername/weather-cli). Feel free to try it out or contribute!
