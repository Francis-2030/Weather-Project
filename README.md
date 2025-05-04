import requests

# Your OpenWeatherMap API key
API_KEY = "3f667bce829ca6d85f63e25fcf582795"

# Function to fetch weather data
def get_weather(city_name):
    base_url = "https://api.openweathermap.org/data/2.5/weather"
    params = {
        'q': city_name.strip(),  # Clean up input
        'appid': API_KEY,
        'units': 'metric'        # Use Celsius
    }

    try:
        response = requests.get(base_url, params=params)
        response.raise_for_status()  # Raise error for bad responses

        data = response.json()

        # Display weather info
        print("\n=============================")
        print(f"🌍 City: {data['name']}, {data['sys']['country']}")
        print(f"🌡 Temperature: {data['main']['temp']}°C")
        print(f"🤒 Feels Like: {data['main']['feels_like']}°C")
        print(f"💧 Humidity: {data['main']['humidity']}%")
        print(f"🌬 Wind Speed: {data['wind']['speed']} m/s")
        print(f"☁ Condition: {data['weather'][0]['description'].title()}")
        print("=============================\n")

    except requests.exceptions.HTTPError as http_err:
        if response.status_code == 404:
            print("❌ City not found. Please check the name and try again.")
        elif response.status_code == 401:
            print("❌ Invalid API key. Please check your API key.")
        else:
            print(f"❌ HTTP error occurred: {http_err}")
    except requests.exceptions.RequestException as err:
        print(f"❌ Error connecting to the weather service: {err}")

# Main program loop
def main():
    print("🌤 Welcome to the Python Weather App 🌤")

    while True:
        city = input("\nEnter city name (or type 'exit' to quit): ")
        if city.lower() == 'exit':
            print("👋 Goodbye! Stay weather-wise! 🌈")
            break
        elif city.strip() == '':
            print("⚠️ Please enter a valid city name.")
        else:
            get_weather(city)

if __name__ == "__main__":
    main()
