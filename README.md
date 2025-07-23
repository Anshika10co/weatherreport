import pandas as pd
from datetime import datetime

weather_data = []
unique_dates = set()

def validate_date(date_str):
    try:
        return datetime.strptime(date_str, "%Y-%m-%d").date()
    except ValueError:
        return None

def add_weather_data():
    date_str = input("Enter date (YYYY-MM-DD): ")
    date = validate_date(date_str)
    if not date:
        print("[ERROR] Invalid date format.")
        return
    if date_str in unique_dates:
        print("[WARNING] Entry for this date exists.")
        return
    try:
        temperature = float(input("Enter temperature (°C): "))
    except ValueError:
        print("[ERROR] Invalid temperature.")
        return
    condition = input("Enter weather condition: ").capitalize()
    weather_data.append({
        "Date": date_str,
        "Temperature (°C)": temperature,
        "Condition": condition
    })
    unique_dates.add(date_str)
    print("[OK] Weather data added.")

def view_weather_data():
    if not weather_data:
        print("No data recorded.")
        return
    df = pd.DataFrame(weather_data)
    print("\n[LOG] Weather Log:\n", df)

def show_summary():
    if not weather_data:
        print("No data to analyze.")
        return
    df = pd.DataFrame(weather_data)
    avg_temp = df["Temperature (°C)"].mean()
    print(f"\n[SUMMARY] Average Temperature: {avg_temp:.2f} °C")

def export_to_csv():
    if not weather_data:
        print("No data to export.")
        return
    df = pd.DataFrame(weather_data)
    df.to_csv("weather_log.csv", index=False)
    print("[OK] Data exported to weather_log.csv")

def main():
    while True:
        print("\n--- AgriWeather Data Recorder ---")
        print("1. Add Weather Data")
        print("2. View Weather Log")
        print("3. Show Summary")
        print("4. Export to CSV")
        print("5. Exit")
        choice = input("Enter choice (1-5): ")
        if choice == '1':
            add_weather_data()
        elif choice == '2':
            view_weather_data()
        elif choice == '3':
            show_summary()
        elif choice == '4':
            export_to_csv()
        elif choice == '5':
            print("Exiting... Goodbye!")
            break
        else:
            print("[ERROR] Invalid choice. Please enter a number between 1 and 5.")

if _name_ == "_main_":
    main()

    
