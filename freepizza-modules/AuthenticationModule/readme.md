# FreePizza Authentication for React

| #    | Version | Change Log    |
| ---- | ------- | ------------- |
| 1    | 1.0     | Initial Draft |

This document covers software specifications and design contraints that needs to be followed while developing the module

------

### Proposal

An authentication module has been proposed to be developed to join the fleet of FreePizza modules. The team feels its important to have a system that enables identification of users without much hassle. Also providing such users with interfaces to control and manage their own information without seeking advanced assistance.

#### Abstract

FreePizza Authentication module for React provides a bundle of features, pages and components that enables the following functionalities:

- To authenticate using a companion server-side module
- To protect the application routes and functionalities against unauthorized access
- To recover access to lost user account (recover password and email address)
- To provide SSO based authentication for Google, Facebook and other providers
- To provide OTP based authentication for password recovery and account access

The module aims at providing necessary functionalities that enables a system to safely authenticate and manage all user identification requirements on client side. It is focused to be used by end user of the system and not by administrators or middlewares.

#### Proposed Features

- The authentication strategy should be made *configurable* by the developers
- The module should come equipped with latest 'Not a robot check' wherever necessary
- The users should be able to choose how they want to sign in, they can choose from many options like:
  - Sign in / Sign up using social providers like Google, Facebook etc.
  - Sign in / Sign up using email and password
  - Sign in / Sign up using phone number (with country code) and OTP
- Provide additional information after sign up / sign in like personal information or profile picture
- The redirect URL should be *configurable* by the developers
- Able to access 'My Account' section to update or change their personal / security preferences, such as:
  - Changing name, profile picture or other information collected in registration form
  - Updating password and security options like recovery email address / phone number
  - Add / Update / Remove email addresses or phone numbers linked to the account
  - Updating primary email address or phone number
  - Able to link their social accounts even after signin up for quick login next time
  - Ability to de-activate / delete / download account details (EU Compliance)
- Account recovery page has to be provided by the module, with necessary security checks, it should include features like:
  - Account identification using the email address or password
  - Multiple account recovery options like email address using OTP, phone number using OTP or other options (as configurable by developers)
  - After successfull recovery of password, the user should be taken back to login screen for authentication
- The module should be able to handle logout call from package
- The module should send JWT token and other user details to package upon appropriate response from server
- In case the authentication expires or is invalidated, the module should ask for user password in a popup modal / window. Without reseting the app state it should be able to renew the session.

#### Customisability Evaluation

The module should be flexible to support minimal customisations, such as:

- The module should support addition of new social provider, configuring the existing one or disable a provider entirely.
- Developers must be able to customise the registration form and add new fields

#### Design Constraints

- The design should follow our general UI / UX guidelines for developing a FreePizza module
- Additionally, the user must feel secure when using the module

------

### Architecture

FreePizza modules are developed to be used with Ark Framework

#### User Flow

We have identified 3 tracks of user flow

1. Sign In / Register with Social Login
   - Login Page > Social Provider Gateway > Callback Page > Perform Account Setup > onSuccess: Redirect to MyAccount > onFailure: Login Page
2. Sign In / Register with Email and Password
   - Login Page / Register Page > Perform Account Setup > OnSuccess: Redirect to MyAccount > OnFailure: Login Page
3. Account Recovery
   - Forgot Password > Account Identification > Choose Recovery Method
   - With OTP:
     - Validate OTP > Update Password
   - Other Options
     - Display 'Next Step'
   - onSuccess: Redirect to Login Page
   - onCancel: Redirect to Login Page

#### Module State (Redux)

> The module does not have any additional state requirement as of now

#### Views

Views are React Components that act as a individual pages mapped to a URL route. Additionally, each page will receive extra props including reference to the module instance and the redux state as well.

1. **SignIn.View** *(Functions)*

   - Sign In using Email and Password
   - Social Provider Button Functions
   - Register using Email and Password
2. **Account.View** *(Functions)*
- Update User Info
   - Upload Profile Picture
   - Change Password (with Old Password)
3. **AccountRecovery.View** *(Functions)*
- Find Account by Email Address
   - Initiate Recovery
   - Validate OTP
   - Reset Password
4. **AccountSetup.View** *(Functions)*

   - Update User Info




#### External Service Integration

##### Google Sign In

Instruction for Javascript (browser): https://developers.google.com/identity/sign-in/web/sign-in

##### Facebook Sign In

Instruction for Javascript (browser): https://developers.facebook.com/docs/facebook-login/web



#### Controller Specification

> The module does not have any additional controller requirement as of now



#### Reducer Specification

> The module does not have any additional controller requirement as of now



#### Service Specification (Declaration Map)

```typescript
type Service = {
  // SignIn.View
  signInWithEmailAndPassword: (emailAddress: string, password: string) => {
    token: string
    userInfo: {
    	type: string
    	emailAddress: string
    	requireSetup: boolean
    	// other user related information can come here
  	}
  }
	registerWithEmailAndPassword: (emailAddress: string, password: string, userInfo: any) => {
    token: string
    userInfo: {
    	type: string
    	emailAddress: string
      requireSetup: boolean
    	// other user related information can come here
  	}
  }
  finishSocialAuthentication: (token: string) => {
    token: string
    userInfo: {
    	type: string
    	emailAddress: string
      requireSetup: boolean
    	// other user related information can come here
  	}
  }
  
  // Account.View | AccountSetup.View
  updateUser: (userInfo: any) => {
    status: 'success' | 'failure'
    data: {
      type: string
      emailAddress: string
      requireSetup: boolean
      // other user related information can come here
    }
  }
  updatePassword: (oldPassword: string, newPassword: string) => {
    status: 'success' | 'failure'
  }
  updateProfilePicture: (blob: File) => {
    // TODO: PENDING
  }
  
  // AccountRecovery.View
  getRecoveryOptions: (emailAddress: string) => {
    isValid: boolean
    options: Array<{
      type: 'email' | 'phone'
      value: string
    }>
  }
  initiateRecovery: (option: Object) => {
    status: 'success' | 'failure'
    otpToken: string
  }
  validateOTP: (otp: string, otpToken: string) => {
    status: 'valid' | 'invalid'
  }
  resetPassword: (otp: string, otpToken: string, newPassword: string) => {
    status: 'success' | 'failure'
    message: string // Error Message (if any)
  }
}
```



