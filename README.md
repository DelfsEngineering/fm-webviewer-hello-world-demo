# FileMaker Web Widget - Hello World (Vue.js + Tailwind CSS)

A modern, reactive web widget built with Vue.js and styled with Tailwind CSS that demonstrates bidirectional communication between a web interface and FileMaker Pro using the `FMPerformScript` function.

## Features

- **Vue.js 3**: Modern reactive framework for better state management and component structure
- **Tailwind CSS**: Utility-first CSS framework for rapid UI development and consistent design
- **Modern UI**: Clean, responsive design with gradient backgrounds and smooth animations
- **FileMaker Integration**: Uses `FMPerformScript` to send data back to FileMaker
- **Data Reception**: Can receive data from FileMaker via JavaScript function calls
- **Error Handling**: Comprehensive error handling and status messages
- **Testing Mode**: Works both inside and outside FileMaker for development
- **Vue Transitions**: Smooth animations for status messages using Vue's transition system
- **Reactive Data**: Automatic UI updates when data changes
- **Responsive Design**: Mobile-first approach with Tailwind's responsive utilities

## Technology Stack Benefits

### Vue.js Benefits
- **Reactive Data Binding**: UI automatically updates when data changes
- **Computed Properties**: Dynamic text generation based on state
- **Event Handling**: Clean event binding with `@click` and `@keyup.enter`
- **Component Lifecycle**: Proper initialization and cleanup
- **Better State Management**: Centralized data and methods
- **Smooth Transitions**: Built-in Vue transition system

### Tailwind CSS Benefits
- **Utility-First**: Rapid development with pre-built utility classes
- **Consistent Design**: Standardized spacing, colors, and typography
- **Responsive**: Built-in responsive design utilities
- **Customizable**: Easy theme customization and extension
- **Performance**: Only includes the CSS you actually use
- **Maintainable**: No custom CSS to maintain or debug

## FileMaker Setup

### 1. Create a Web Viewer Object

1. In FileMaker Pro, create a new layout or open an existing one
2. Insert a **Web Viewer** object
3. Set the **Web Address** to the path of your `index.html` file
4. Enable **Allow interaction with web content**

### 2. Create the FileMaker Script

Create a script named `HandleWidgetMessage` in FileMaker:

```filemaker
# FileMaker Script: HandleWidgetMessage
# Parameter: $message (the data sent from the widget)

# Get the parameter passed from the widget
Set Variable [ $message; Value:Get(ScriptParameter) ]

# Log the message (optional)
Show Custom Dialog [ "Widget Message"; "Message received: " & $message ]

# You can now use $message in your FileMaker solution
# Examples:
# - Set a field value
# - Perform a find
# - Trigger other scripts
# - Update records
```

### 3. Pass Data to the Widget

To send data from FileMaker to the widget, use the `FMPerformJavaScript` function:

```filemaker
# Example: Send a name to the widget
FMPerformJavaScript("receiveFromFileMaker('John Doe');")

# Example: Send data from a field
FMPerformJavaScript("receiveFromFileMaker('" & YourFieldName & "');")
```

## Widget Functions

### Available JavaScript Functions

The widget exposes these functions that FileMaker can call:

- **`receiveFromFileMaker(data)`**: Receives data from FileMaker and updates the display
- **`sendToFileMaker()`**: Sends the current input value back to FileMaker
- **`updateHelloText()`**: Updates the hello text with received data

### Data Flow

1. **FileMaker → Widget**: Use `FMPerformJavaScript("receiveFromFileMaker('Your Data');")`
2. **Widget → FileMaker**: Click "Send to FileMaker" button, which calls `FMPerformScript('HandleWidgetMessage', message)`

## Vue.js + Tailwind Implementation Details

### Reactive Data Properties

```javascript
data() {
    return {
        filemakerData: '',        // Data received from FileMaker
        messageInput: '',         // Current input value
        showStatus: false,        // Whether to show status message
        statusMessage: '',        // Status message text
        statusType: 'success',    // Status type (success/error)
        statusKey: 0,            // Key for forcing re-renders
        isFileMakerConnected: false // Connection status
    };
}
```

### Computed Properties

```javascript
computed: {
    helloText() {
        return this.filemakerData ? `Hello ${this.filemakerData}!` : 'Hello World!';
    },
    
    receivedData() {
        return this.filemakerData;
    }
}
```

### Tailwind CSS Classes Used

#### Layout & Spacing
- **Container**: `max-w-md w-full` (max width, full width)
- **Padding**: `p-8` (padding 2rem), `px-6 py-3` (horizontal/vertical padding)
- **Margin**: `mb-6` (margin bottom), `mt-6` (margin top), `space-x-3` (horizontal spacing)

#### Colors & Backgrounds
- **Background**: `bg-white`, `bg-gray-50`, `bg-gradient-to-r from-blue-500 to-purple-600`
- **Text**: `text-gray-800`, `text-white`, `text-gray-600`
- **Borders**: `border-2 border-gray-200`, `border-blue-500`

#### Typography
- **Size**: `text-4xl`, `text-base`, `text-sm`, `text-xs`
- **Weight**: `font-bold`, `font-semibold`, `font-medium`

#### Interactive Elements
- **Buttons**: `hover:scale-105`, `disabled:opacity-50`, `transition-all duration-200`
- **Input**: `focus:outline-none focus:border-blue-500 transition-colors duration-300`

#### Responsive Design
- **Mobile**: `min-h-screen`, `p-5` (responsive padding)
- **Flexbox**: `flex items-center justify-center`

### Event Handling

- **Click Events**: `@click="sendToFileMaker"`
- **Input Binding**: `v-model="messageInput"`
- **Enter Key**: `@keyup.enter="sendToFileMaker"`
- **Conditional Rendering**: `v-if="showStatus"`

### Vue Transitions with Tailwind

```html
<transition
    enter-active-class="transition-all duration-500 ease-out"
    enter-from-class="opacity-0 transform translate-y-2"
    enter-to-class="opacity-100 transform translate-y-0"
    leave-active-class="transition-all duration-300 ease-in"
    leave-from-class="opacity-100 transform translate-y-0"
    leave-to-class="opacity-0 transform translate-y-2"
>
```

## Testing

### Inside FileMaker
- The widget will automatically detect FileMaker and show "Connected to FileMaker"
- All functions will work normally

### Outside FileMaker (Development)
- The widget runs in "test mode"
- `FMPerformScript` calls are logged to the console
- Perfect for development and testing

## Customization

### Changing the Hello Text
The widget automatically updates the "Hello World!" text when data is received from FileMaker. The text becomes "Hello [Your Data]!" using Vue's reactive computed property.

### Styling with Tailwind
All styling is now done with Tailwind utility classes:
- **Colors**: Change `from-blue-500 to-purple-600` to any Tailwind color palette
- **Spacing**: Modify `p-8`, `mb-6`, etc. for different spacing
- **Typography**: Adjust `text-4xl`, `font-bold`, etc. for different text styles
- **Shadows**: Change `shadow-2xl` to `shadow-lg`, `shadow-xl`, etc.

### Adding More Fields
To add more input fields with Vue.js and Tailwind:
1. Add new data properties in the `data()` function
2. Add HTML input elements with `v-model` binding and Tailwind classes
3. Modify the `sendToFileMaker()` method to collect all values
4. Update the FileMaker script to handle multiple parameters

### Tailwind Customization
The widget includes a custom Tailwind configuration for animations:

```javascript
tailwind.config = {
    theme: {
        extend: {
            animation: {
                'fade-in': 'fadeIn 0.5s ease-in-out',
                'fade-out': 'fadeOut 0.5s ease-in-out',
            },
            keyframes: {
                fadeIn: { '0%': { opacity: '0' }, '100%': { opacity: '1' } },
                fadeOut: { '0%': { opacity: '1' }, '100%': { opacity: '0' } }
            }
        }
    }
}
```

### Vue.js Enhancements
- **Add Components**: Break down into smaller, reusable components
- **Add Vuex**: For more complex state management
- **Add Router**: For multi-page widgets
- **Add Animations**: Leverage Vue's transition system with Tailwind

## Troubleshooting

### Common Issues

1. **Widget not loading**: Check the file path in the Web Viewer
2. **Script not receiving data**: Ensure the script name matches exactly (`HandleWidgetMessage`)
3. **JavaScript errors**: Check the browser console for error messages
4. **No communication**: Verify that "Allow interaction with web content" is enabled
5. **Vue not loading**: Check internet connection for CDN access
6. **Tailwind not loading**: Check internet connection for CDN access

### Debug Mode

The widget includes comprehensive logging:
- Status messages for all operations
- Console logging for debugging
- Error handling with user-friendly messages
- Vue DevTools support (when available)

## File Structure

```
Yitzchok/
├── index.html          # Main widget file (Vue.js + Tailwind CSS)
└── README.md           # This documentation
```

## Browser Compatibility

- Modern browsers (Chrome, Firefox, Safari, Edge)
- Mobile responsive design
- Works with FileMaker Pro 16+ (Web Viewer support)
- Vue.js 3 compatibility
- Tailwind CSS 3 compatibility

## Dependencies

- **Vue.js 3**: Loaded from CDN (unpkg.com)
- **Tailwind CSS**: Loaded from CDN (cdn.tailwindcss.com)

## Security Notes

- The widget runs in FileMaker's sandboxed environment
- `FMPerformScript` calls are limited to FileMaker's security settings
- External CDN requests for Vue.js and Tailwind CSS
- All data stays within the FileMaker environment

## Next Steps

This Hello World widget provides a foundation for more complex FileMaker web integrations:

- **Form Widgets**: Collect user input and save to FileMaker
- **Data Display**: Show FileMaker data in custom layouts
- **Interactive Charts**: Create dynamic visualizations with Vue.js
- **Real-time Updates**: Live data synchronization
- **Custom UI Components**: Branded interfaces for your solutions
- **Vue.js Ecosystem**: Leverage Vue Router, Vuex, and other Vue.js libraries
- **Tailwind Components**: Use Tailwind UI or other component libraries

## Support

For issues or questions:
1. Check the browser console for JavaScript errors
2. Verify FileMaker script names and parameters
3. Ensure Web Viewer settings are correct
4. Test with simple data first before complex integrations
5. Check Vue.js documentation for framework-specific issues
6. Check Tailwind CSS documentation for styling issues
7. Verify CDN connectivity for external dependencies
