# Authenticator App

## Overview

The Authenticator App provides a modular and customizable authentication experience for iOS applications. It includes a ViewModel for managing validation logic, a configuration struct for theming and customization, and a controller to handle user interactions.

---

## Components

### 1. **AuthenticatorViewModel.swift**

#### Purpose

Manages user authentication logic, including input validation and secure storage of credentials.

#### Key Features:

* **Validation**:

  * `isValidEmail(_:)`: Validates email format.
  * `isValidPassword(_:)`: Validates password strength (min 8 chars, includes upper/lowercase and digit).
  * `isValidUsername(_:)`: Ensures username contains only alphanumeric characters.
* **Keychain Integration**:

  * Securely stores user credentials using the iOS Keychain.
* **Combine Integration**:

  * Publishes user details through `userDetailsPublisher`.

---

### 2. **AuthConfig.swift**

#### Purpose

Provides a customizable configuration for UI elements and behavior.

#### Customizable Properties:

* **Colors**:

  * `primaryColor`: The primary button color.
  * `backgroundColor`: Background color of the view.
  * `textColor`: Text color for input fields.
* **Font**:

  * `font`: Custom font for input fields and buttons.
* **Field Visibility**:

  * `isUsernameFieldHidden`: Hide username field if true.
  * `isFirstNameFieldHidden`: Hide first name field if true.
  * `isLastNameFieldHidden`: Hide last name field if true.
* **Error Handling**:

  * `errorMessage`: Message displayed for invalid input.

---

### 3. **AuthenticatorController.swift**

#### Purpose

Handles user interactions and coordinates between UI and ViewModel.

#### Key Features:

* **Dynamic UI Setup**:

  * Constructs UI elements dynamically based on `AuthConfig` properties.
* **Validation Flow**:

  * Uses `AuthenticatorViewModel` to validate email, password, and optional fields.
* **User Feedback**:

  * Displays an alert on validation failure with a customizable message.
* **User Action**:

  * Submits user details to `AuthenticatorViewModel` and stores credentials securely.

#### Key Methods:

* `applyConfig()`: Updates UI based on `AuthConfig`.
* `submitAction()`: Validates input and processes user details.

---

## Installation

1. Add all source files (`AuthenticatorViewModel.swift`, `AuthConfig.swift`, `AuthenticatorController.swift`) to your Xcode project.
2. Import the required modules:

   ```swift
   import UIKit
   import Combine
   import Security
   ```

---

## Usage

### Step 1: Configure Authenticator

Create an instance of `AuthConfig` and customize it as needed:

```swift
let config = AuthConfig(
    primaryColor: .systemGreen,
    backgroundColor: .lightGray,
    textColor: .darkText,
    font: UIFont.boldSystemFont(ofSize: 18),
    submitButtonText: "Login",
    errorMessage: "Please ensure all fields are correctly filled."
)
```

### Step 2: Initialize and Present the Authenticator Controller

```swift
let authController = AuthenticatorController()
authController.config = config

// Handle completion
authController.viewModel.onCompletion = { userDetails in
    print("User Details:", userDetails)
}

// Present the controller
let navigationController = UINavigationController(rootViewController: authController)
present(navigationController, animated: true)
```

### Step 3: Subscribe to User Details

```swift
let cancellable = authController.viewModel.userDetailsPublisher
    .sink { userDetails in
        print("Received User Details:", userDetails)
    }
```

---

## Keychain Handling

The app uses the iOS Keychain for secure storage. Credentials are stored under a fixed account identifier `com.example.authsdk.credentials`.

---

## Error Handling

Customize error messages via `AuthConfig.errorMessage`. Validation errors trigger an alert, ensuring users are informed of issues in their input.

---

## License

This project is licensed under the MIT License.

