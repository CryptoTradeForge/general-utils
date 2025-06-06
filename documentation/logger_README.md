# Logger

A utility for logging general and error messages with timezone support.

## Features

- Write logs to rotating daily files
- Separate general logs from error logs
- Timezone support for accurate timestamps
- Support for different time formats (timestamps, datetime objects, string dates)
- Daily log file rotation with configurable retention
- Automatic creation of log directories

## Installation

### Dependencies

This utility requires the `pytz` package.

```bash
# Install directly
pip install pytz

# Or install from requirements.txt (recommended)
pip install -r requirements.txt
```

## Usage

### Basic Usage

```python
from logger import Logger

# Create a logger instance with default settings
logger = Logger()

# Write general logs
logger.write_log("Application started")
logger.write_log("User logged in: user123")

# Write error logs
logger.write_error_log("Database connection failed")
logger.write_error_log("Invalid input received: missing required field")
```

### Log File Organization

The Logger creates the following directory structure:

```
logs/
├── general_logs/
│   ├── general.log          # Current log file
│   ├── general.log.2023-05-01  # Archived log file
│   └── ...
└── error_logs/
    ├── error.log            # Current log file
    ├── error.log.2023-05-01    # Archived log file
    └── ...
```

### Using with Custom Configuration

```python
# Create a logger with custom settings
custom_logger = Logger(
    log_dir="~/app_logs",           # Custom log directory
    timezone_str="America/New_York", # Custom timezone
    backup_count=60                  # Keep 60 days of logs
)
```

### Logging with Different Time Formats

```python
import time
from datetime import datetime

# 1. Log with current time (default behavior)
logger.write_log("Log with current server time")

# 2. Log with a datetime object
current_dt = datetime.now()
logger.write_log("Log with datetime object", time=current_dt)

# 3. Log with a datetime string
date_str = "2023-06-15 14:30:00"
logger.write_log("Log with string date", time=date_str)

# 4. Log with Unix timestamp (seconds since epoch)
timestamp = time.time()
logger.write_log("Log with Unix timestamp", time=timestamp)

# 5. Log with millisecond timestamp (common in JavaScript/frontend)
ms_timestamp = int(time.time() * 1000)
logger.write_log("Log with millisecond timestamp", time=ms_timestamp)
```

### Practical Examples

#### Application Events Logging

```python
# Application lifecycle events
logger.write_log("Application started")
logger.write_log("Configuration loaded from config.ini")
logger.write_log("Connected to database successfully")
logger.write_log("Web server started on port 8080")
logger.write_log("Application shutdown gracefully")

# User activity logging
logger.write_log("User 'john_doe' logged in")
logger.write_log("User 'john_doe' updated profile")
logger.write_log("User 'john_doe' logged out")

# Feature usage logging
logger.write_log("Report 'Q2 Sales' generated by user 'jane_smith'")
logger.write_log("Payment processed: Order #12345, Amount: $99.95")
```

#### Error Logging Examples

```python
# Database errors
logger.write_error_log("Failed to connect to database: Connection timeout")
logger.write_error_log("Query execution failed: Syntax error near 'SELECT'")

# API errors
logger.write_error_log("API request failed: Endpoint '/users' returned 404")
logger.write_error_log("Authentication failed: Invalid API key")

# Application errors
try:
    result = 10 / 0
except Exception as e:
    logger.write_error_log(f"Calculation error: {str(e)}")
    
try:
    with open('missing_file.txt', 'r') as file:
        content = file.read()
except FileNotFoundError as e:
    logger.write_error_log(f"File operation failed: {str(e)}")
```

## API Reference

### Constructor

- `Logger(log_dir=None, timezone_str="Asia/Taipei", backup_count=30)` - Initialize with custom settings
  - `log_dir`: Base directory for log files (default: `./logs`)
  - `timezone_str`: Timezone string (default: "Asia/Taipei")
  - `backup_count`: Number of daily log files to keep (default: 30)

### Methods

- `write_log(message, time=None)` - Write a message to the general log
  - `message`: The log message to write
  - `time`: Optional custom timestamp (defaults to current time)
  
- `write_error_log(message, time=None)` - Write a message to the error log
  - `message`: The error message to write
  - `time`: Optional custom timestamp (defaults to current time)

### Supported Time Formats

The Logger accepts the following time formats:
- `None` - Uses current time
- `datetime` object - Uses the provided datetime (timezone aware or naive)
- String in ISO format - Parses the string as a datetime
- Unix timestamp (seconds since epoch) - Converts to datetime
- Millisecond timestamp - Converts to datetime
