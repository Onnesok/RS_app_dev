# HTTP Status Codes

HTTP status codes are three-digit numbers that indicate the result of an HTTP request. They help you understand what happened when your app communicates with a server.

## Status Code Categories

### 1xx - Informational
The request is being processed or has been received.

### 2xx - Success
The request was successful.

### 3xx - Redirection
The request needs to be redirected to another location.

### 4xx - Client Error
The request contains bad syntax or cannot be fulfilled.

### 5xx - Server Error
The server failed to fulfill a valid request.

## Common Status Codes

### 2xx Success Codes

#### 200 - OK
**Meaning**: The request was successful.
```dart
// Example: Successful API call
if (response.statusCode == 200) {
  // Process the data
  Map<String, dynamic> data = jsonDecode(response.body);
  print('Data received: $data');
}
```

#### 201 - Created
**Meaning**: The request was successful and a new resource was created.
```dart
// Example: User registration
if (response.statusCode == 201) {
  print('User account created successfully!');
  // Navigate to login screen
}
```

#### 204 - No Content
**Meaning**: The request was successful but there's no content to return.
```dart
// Example: Delete operation
if (response.statusCode == 204) {
  print('Item deleted successfully');
  // Remove item from UI
}
```

### 3xx Redirection Codes

#### 301 - Moved Permanently
**Meaning**: The resource has been permanently moved to a new URL.

#### 302 - Found (Temporary Redirect)
**Meaning**: The resource is temporarily available at a different URL.

#### 304 - Not Modified
**Meaning**: The resource hasn't changed since the last request.
```dart
// Example: Cached data
if (response.statusCode == 304) {
  print('Using cached data');
  // Use local cached data
}
```

### 4xx Client Error Codes

#### 400 - Bad Request
**Meaning**: The server cannot understand the request due to invalid syntax.
```dart
// Example: Invalid form data
if (response.statusCode == 400) {
  print('Please check your input data');
  // Show error message to user
}
```

#### 401 - Unauthorized
**Meaning**: Authentication is required and has failed or not been provided.
```dart
// Example: Login required
if (response.statusCode == 401) {
  print('Please log in to continue');
  // Redirect to login screen
}
```

#### 403 - Forbidden
**Meaning**: The server understood the request but refuses to authorize it.
```dart
// Example: Insufficient permissions
if (response.statusCode == 403) {
  print('You don\'t have permission to access this resource');
  // Show access denied message
}
```

#### 404 - Not Found
**Meaning**: The requested resource was not found on the server.
```dart
// Example: User not found
if (response.statusCode == 404) {
  print('User not found');
  // Show "User not found" message
}
```

#### 409 - Conflict
**Meaning**: The request conflicts with the current state of the server.
```dart
// Example: Username already exists
if (response.statusCode == 409) {
  print('Username already exists');
  // Show error message
}
```

#### 422 - Unprocessable Entity
**Meaning**: The request was well-formed but contains semantic errors.
```dart
// Example: Validation errors
if (response.statusCode == 422) {
  Map<String, dynamic> errors = jsonDecode(response.body);
  print('Validation errors: $errors');
  // Display validation errors to user
}
```

#### 429 - Too Many Requests
**Meaning**: The user has sent too many requests in a given amount of time.
```dart
// Example: Rate limiting
if (response.statusCode == 429) {
  print('Too many requests. Please try again later.');
  // Show retry after delay message
}
```

### 5xx Server Error Codes

#### 500 - Internal Server Error
**Meaning**: The server encountered an unexpected condition.
```dart
// Example: Server error
if (response.statusCode == 500) {
  print('Server error occurred');
  // Show generic error message
}
```

#### 502 - Bad Gateway
**Meaning**: The server received an invalid response from an upstream server.

#### 503 - Service Unavailable
**Meaning**: The server is temporarily unable to handle the request.
```dart
// Example: Server maintenance
if (response.statusCode == 503) {
  print('Service temporarily unavailable');
  // Show maintenance message
}
```

#### 504 - Gateway Timeout
**Meaning**: The server didn't receive a timely response from an upstream server.

## Handling Status Codes in Flutter

### Basic Error Handling
```dart
Future<void> makeApiCall() async {
  try {
    final response = await http.get(Uri.parse('https://api.example.com/data'));
    
    switch (response.statusCode) {
      case 200:
        // Success
        handleSuccess(response.body);
        break;
      case 401:
        // Unauthorized
        handleUnauthorized();
        break;
      case 404:
        // Not found
        handleNotFound();
        break;
      case 500:
        // Server error
        handleServerError();
        break;
      default:
        // Other errors
        handleError('Unexpected error: ${response.statusCode}');
    }
  } catch (e) {
    handleError('Network error: $e');
  }
}
```

### Using HTTP Package
```dart
import 'package:http/http.dart' as http;

Future<Map<String, dynamic>> fetchUserData(String userId) async {
  final response = await http.get(
    Uri.parse('https://api.example.com/users/$userId'),
    headers: {
      'Authorization': 'Bearer $token',
      'Content-Type': 'application/json',
    },
  );

  if (response.statusCode == 200) {
    return jsonDecode(response.body);
  } else if (response.statusCode == 404) {
    throw Exception('User not found');
  } else if (response.statusCode == 401) {
    throw Exception('Unauthorized access');
  } else {
    throw Exception('Failed to load user data');
  }
}
```

### Custom Response Handler
```dart
class ApiResponse<T> {
  final T? data;
  final int statusCode;
  final String? message;

  ApiResponse({
    this.data,
    required this.statusCode,
    this.message,
  });

  bool get isSuccess => statusCode >= 200 && statusCode < 300;
  bool get isClientError => statusCode >= 400 && statusCode < 500;
  bool get isServerError => statusCode >= 500;
}

Future<ApiResponse<Map<String, dynamic>>> apiCall() async {
  try {
    final response = await http.get(Uri.parse('https://api.example.com/data'));
    
    return ApiResponse(
      data: response.statusCode == 200 ? jsonDecode(response.body) : null,
      statusCode: response.statusCode,
      message: response.reasonPhrase,
    );
  } catch (e) {
    return ApiResponse(
      statusCode: 0,
      message: 'Network error: $e',
    );
  }
}
```

## Best Practices

### 1. Always Check Status Codes
```dart
// Good
if (response.statusCode == 200) {
  // Handle success
} else {
  // Handle error
}

// Bad
// Don't assume success without checking
```

### 2. Provide User-Friendly Messages
```dart
String getErrorMessage(int statusCode) {
  switch (statusCode) {
    case 400:
      return 'Please check your input and try again';
    case 401:
      return 'Please log in to continue';
    case 404:
      return 'The requested information was not found';
    case 500:
      return 'Something went wrong. Please try again later';
    default:
      return 'An unexpected error occurred';
  }
}
```

### 3. Handle Network Errors
```dart
try {
  final response = await http.get(url);
  // Handle response
} catch (e) {
  if (e is SocketException) {
    print('No internet connection');
  } else if (e is TimeoutException) {
    print('Request timed out');
  } else {
    print('Network error: $e');
  }
}
```

### 4. Use Appropriate HTTP Methods
- **GET**: Retrieve data
- **POST**: Create new data
- **PUT**: Update existing data
- **DELETE**: Remove data

## Testing Status Codes

```dart
// Unit test example
test('should handle 404 error', () async {
  // Mock server response
  when(mockHttpClient.get(any))
      .thenAnswer((_) async => http.Response('Not found', 404));

  final result = await apiService.fetchData();
  
  expect(result.statusCode, 404);
  expect(result.isSuccess, false);
});
```

## Summary

Understanding HTTP status codes is crucial for building robust Flutter apps that communicate with APIs. Always:

1. Check status codes before processing responses
2. Handle different error scenarios gracefully
3. Provide meaningful feedback to users
4. Implement proper error handling and retry logic
5. Test your app with various status code scenarios

Remember: Good error handling improves user experience and makes your app more reliable!
