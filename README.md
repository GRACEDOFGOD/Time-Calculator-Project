# Time-Calculator-Project

Solution for Time Calculator
Close
×
PY
def add_time(start, duration, starting_day=None):
    # Define days of the week
    days_of_week = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
    
    # Split and extract the start time and AM/PM
    start_time, period = start.split()
    start_hour, start_minute = map(int, start_time.split(":"))
    
    # Convert start time to 24-hour format
    if period == "PM" and start_hour != 12:
        start_hour += 12
    elif period == "AM" and start_hour == 12:
        start_hour = 0
    
    # Split and extract the duration
    duration_hour, duration_minute = map(int, duration.split(":"))
    
    # Calculate total minutes and hours
    total_minutes = start_minute + duration_minute
    extra_hour = total_minutes // 60  # How many full hours from the extra minutes
    new_minute = total_minutes % 60
    
    total_hours = start_hour + duration_hour + extra_hour
    new_hour = total_hours % 24
    days_later = total_hours // 24
    
    # Convert back to 12-hour format
    if new_hour >= 12:
        period = "PM"
        if new_hour > 12:
            new_hour -= 12
    else:
        period = "AM"
        if new_hour == 0:
            new_hour = 12
    
    # Determine the new day if a starting day was provided
    if starting_day:
        starting_day = starting_day.capitalize()  # Handle case insensitivity
        day_index = days_of_week.index(starting_day)
        new_day = days_of_week[(day_index + days_later) % 7]
        day_result = f", {new_day}"
    else:
        day_result = ""
    
    # Add information about the number of days later
    if days_later == 1:
        day_info = " (next day)"
    elif days_later > 1:
        day_info = f" ({days_later} days later)"
    else:
        day_info = ""
    
    # Format the result
    new_time = f"{new_hour}:{new_minute:02d} {period}{day_result}{day_info}"
    
    return new_time

# Test cases
print(add_time('3:00 PM', '3:10'))  # Returns: 6:10 PM
print(add_time('11:30 AM', '2:32', 'Monday'))  # Returns: 2:02 PM, Monday
print(add_time('11:43 PM', '24:20', 'tueSday'))  # Returns: 12:03 AM, Thursday (2 days later)
print(add_time('6:30 PM', '205:12'))  # Returns: 7:42 AM (9 days later)
Close
