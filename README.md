# FileMaker Web Widget - Hello World (Vue.js + Tailwind CSS)

A modern, reactive web widget built with Vue.js and styled with Tailwind CSS that demonstrates bidirectional communication between a web interface and FileMaker Pro.

## Features

- **Vue.js 3**: Modern reactive framework for state management
- **Tailwind CSS**: Utility-first CSS framework for rapid UI development
- **FileMaker Integration**: Uses `FileMaker.PerformScriptWithOption` for communication
- **Date Range Selection**: HTML date inputs for selecting start and end dates
- **Message Input**: Text input for sending messages to FileMaker
- **Error Handling**: Comprehensive error handling and status messages
- **Testing Mode**: Works both inside and outside FileMaker for development

## FileMaker Setup

### 1. Create a Web Viewer Object
1. In FileMaker Pro, create a new layout or open an existing one
2. Insert a **Web Viewer** object
3. Set the **Web Address** to the path of your `index.html` file
4. Enable **Allow interaction with web content**

### 2. Create the FileMaker Scripts

Create these scripts in FileMaker:

```filemaker
# Script: HandleWidgetMessage
# Parameter: $message (the data sent from the widget)
Set Variable [ $message; Value:Get(ScriptParameter) ]
Show Custom Dialog [ "Widget Message"; "Message received: " & $message ]

# Script: HandleDateRangeMessage  
# Parameter: $dateRange (comma-separated start,end dates)
Set Variable [ $dateRange; Value:Get(ScriptParameter) ]
Show Custom Dialog [ "Date Range"; "Date range received: " & $dateRange ]
```

### 3. Pass Data to the Widget

To send data from FileMaker to the widget:

```filemaker
# Send a message
FMPerformJavaScript("receiveFromFileMaker('John Doe');")

# Send a date range
FMPerformJavaScript("receiveDateFromFileMaker('2024-01-01,2024-01-31');")
```

## Widget Functions

### Available JavaScript Functions

- **`receiveFromFileMaker(data)`**: Receives data from FileMaker and updates the display
- **`receiveDateFromFileMaker(dateString)`**: Receives date range from FileMaker
- **`sendToFileMaker()`**: Sends the current input value back to FileMaker
- **`sendDateRangeToFileMaker()`**: Sends the selected date range to FileMaker

### Data Flow

1. **FileMaker → Widget**: Use `FMPerformJavaScript("receiveFromFileMaker('Your Data');")`
2. **Widget → FileMaker**: Click buttons to call `FileMaker.PerformScriptWithOption`

## Current Implementation

The widget currently uses:
- **HTML Date Inputs**: Native browser date pickers for date range selection
- **Vue.js 3**: For reactive data binding and component management
- **Tailwind CSS**: For styling and responsive design
- **FileMaker Integration**: Direct communication with FileMaker scripts

## Testing

### Inside FileMaker
- Widget automatically detects FileMaker and shows "Connected to FileMaker"
- All functions work normally

### Outside FileMaker (Development)
- Widget runs in "test mode"
- FileMaker calls are logged to the console
- Perfect for development and testing

## Troubleshooting

### Common Issues

1. **Widget not loading**: Check the file path in the Web Viewer
2. **Script not receiving data**: Ensure script names match exactly
3. **JavaScript errors**: Check the browser console for error messages
4. **No communication**: Verify "Allow interaction with web content" is enabled

### Debug Mode

The widget includes comprehensive logging:
- Status messages for all operations
- Console logging for debugging
- Error handling with user-friendly messages

## File Structure

```
Yitzchok/
├── index.html          # Main widget file (Vue.js + Tailwind CSS)
├── Demo.fmp12          # FileMaker demo file
└── README.md           # This documentation
```

## Dependencies

- **Vue.js 3**: Loaded from CDN (unpkg.com)
- **Tailwind CSS**: Loaded from CDN (cdn.tailwindcss.com)

## Browser Compatibility

- Modern browsers (Chrome, Firefox, Safari, Edge)
- Mobile responsive design
- Works with FileMaker Pro 16+ (Web Viewer support)

## Next Steps

This widget provides a foundation for more complex FileMaker web integrations:
- Form widgets for data collection
- Data display widgets
- Interactive charts and visualizations
- Custom branded interfaces
