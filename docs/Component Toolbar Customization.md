
# Component Toolbar Customization

This document provides a comprehensive guide to customizing component toolbars, enabling developers to create tailored editing experiences. It covers the toolbar structure, adding custom buttons, handling commands, managing button states, and leveraging the event system.

**Target Audience:** Intermediate Developers, UI Developers

## Toolbar Structure

The component toolbar is typically structured as a container holding a series of buttons or controls. The exact structure depends on the underlying framework or library being used. However, the general principle remains the same: a hierarchical arrangement of elements that trigger specific actions.

**Example (Conceptual):**

```html
<div class="toolbar">
  <button data-command="bold">Bold</button>
  <button data-command="italic">Italic</button>
  <select data-command="fontSize">
    <option value="12">12px</option>
    <option value="14">14px</option>
    <option value="16">16px</option>
  </select>
</div>
```

In this example, the `toolbar` class identifies the container.  Buttons are represented by `<button>` elements, and a dropdown is represented by a `<select>` element. The `data-command` attribute is used to associate each control with a specific command.

## Custom Buttons

Adding custom buttons to the toolbar allows you to extend the functionality of your components. This involves creating the button element and integrating it into the toolbar's structure.

**Steps:**

1.  **Create the Button Element:** Define the HTML for your button, including its appearance and any necessary attributes.
2.  **Define the Command:** Associate the button with a specific command that will be executed when the button is clicked.
3.  **Add the Button to the Toolbar:** Insert the button element into the toolbar's HTML structure.
4.  **Implement Command Handling:**  Implement the logic to handle the command associated with the button.

**Example:**

```javascript
// Create a custom button element
const customButton = document.createElement('button');
customButton.textContent = 'Custom Action';
customButton.dataset.command = 'customAction';

// Get the toolbar element
const toolbar = document.querySelector('.toolbar');

// Add the button to the toolbar
toolbar.appendChild(customButton);

// Implement command handling (see Command Handling section)
```

## Command Handling

Command handling is the process of executing specific actions when a button or control in the toolbar is activated. This typically involves listening for events (e.g., click events) and then executing the corresponding command.

**Common Approaches:**

*   **Direct Event Handling:** Attach event listeners directly to the buttons and execute the command within the event handler.
*   **Command Pattern:** Use the command pattern to encapsulate commands as objects, allowing for more flexible and maintainable code.
*   **Centralized Command Dispatcher:**  Use a central command dispatcher to route commands to the appropriate handlers.

**Example (Direct Event Handling):**

```javascript
const toolbar = document.querySelector('.toolbar');

toolbar.addEventListener('click', (event) => {
  const command = event.target.dataset.command;

  switch (command) {
    case 'bold':
      // Execute bold command
      console.log('Bold command executed');
      break;
    case 'italic':
      // Execute italic command
      console.log('Italic command executed');
      break;
    case 'customAction':
      // Execute custom action
      console.log('Custom action executed');
      break;
    default:
      console.warn(`Unknown command: ${command}`);
  }
});
```

**Example (Command Pattern):**

```javascript
// Command Interface
class Command {
  execute() {
    throw new Error("Execute method must be implemented.");
  }
}

// Concrete Command
class BoldCommand extends Command {
  execute() {
    console.log("Bold command executed (Command Pattern)");
  }
}

// Invoker (Toolbar)
class Toolbar {
  constructor() {
    this.commands = {
      'bold': new BoldCommand()
    };
  }

  executeCommand(commandName) {
    if (this.commands[commandName]) {
      this.commands[commandName].execute();
    } else {
      console.warn(`Unknown command: ${commandName}`);
    }
  }
}

const toolbar = new Toolbar();

// Example usage (simulating a button click)
toolbar.executeCommand('bold');
```

## Button States

Button states (e.g., enabled/disabled, active/inactive) provide visual feedback to the user and control the availability of commands.

**Common States:**

*   **Enabled/Disabled:** Indicates whether the button is currently active and can be clicked.
*   **Active/Inactive:** Indicates whether the command associated with the button is currently applied.

**Example (Setting Button State):**

```javascript
const boldButton = document.querySelector('[data-command="bold"]');

// Disable the button
boldButton.disabled = true;

// Enable the button
boldButton.disabled = false;

// Add an "active" class to indicate the button is active
boldButton.classList.add('active');

// Remove the "active" class to indicate the button is inactive
boldButton.classList.remove('active');
```

**Example (Updating Button State based on Component State):**

```javascript
function updateButtonStates(componentState) {
  const boldButton = document.querySelector('[data-command="bold"]');
  const italicButton = document.querySelector('[data-command="italic"]');

  if (componentState.isBold) {
    boldButton.classList.add('active');
  } else {
    boldButton.classList.remove('active');
  }

  if (componentState.isItalic) {
    italicButton.classList.add('active');
  } else {
    italicButton.classList.remove('active');
  }
}

// Example usage (simulating a component state change)
const componentState = {
  isBold: true,
  isItalic: false
};

updateButtonStates(componentState);
```

## Event System

The event system allows components and the toolbar to communicate and synchronize their states. This is particularly useful for updating button states based on component changes or for triggering actions in the component when a toolbar button is clicked.

**Common Events:**

*   **`component:state-changed`:**  Fired when the component's state changes.
*   **`toolbar:command-executed`:** Fired when a command is executed from the toolbar.

**Example (Listening for Component State Changes):**

```javascript
document.addEventListener('component:state-changed', (event) => {
  const componentState = event.detail;
  updateButtonStates(componentState); // Update button states based on the new component state
});
```

**Example (Firing a Toolbar Command Executed Event):**

```javascript
const toolbar = document.querySelector('.toolbar');

toolbar.addEventListener('click', (event) => {
  const command = event.target.dataset.command;

  // Execute the command (as shown in the Command Handling section)

  // Dispatch a custom event
  const commandExecutedEvent = new CustomEvent('toolbar:command-executed', {
    detail: {
      command: command
    }
  });
  document.dispatchEvent(commandExecutedEvent);
});
```

This event can then be listened to by other parts of the application to react to toolbar actions.

**Related Endpoints:**

*   `/components/toolbar`:  (Placeholder for API endpoint related to toolbar configuration)
*   `/commands`: (Placeholder for API endpoint related to available commands)
