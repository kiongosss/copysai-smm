# CopysAI - Technical Documentation

## Introduction

CopysAI is an AI-powered social media management and scheduling SaaS platform built with modern technologies to help businesses manage their social media presence across multiple platforms.

### Building Technology

- Backend Framework: PHP/Laravel 10x
- Frontend Framework: HTML/Jquery/VueJs

### Integration

- Meta Cloud API for Facebook & Instagram
- OpenAI API for AI Content
- LinkedIn API
- X/Twitter API
- TikTok API
- Threads API

## System Requirements

### System Requirements

- Requires PHP Version: 8.0.2 or later
- Must have SSL enabled on the server
- Database Support: MySQL, Mysqli
- MySQL Supported Version: 5.x & 8.X

### PHP Extension Requirements

- BCMath PHP Extension
- Ctype PHP Extension
- Fileinfo PHP Extension
- Mbstring PHP Extension
- OpenSSL PHP Extension
- PDO PHP Extension
- Tokenizer PHP Extension
- XML PHP Extension

## Installation Guide

### Step 1: Preparation

Download file from CodeCanyon and extract it. You will get the following folders:
- Documentation
- Installable File

### Step 2: Database Setup

1. Login to your cPanel and go to MySQL® Database Wizard
2. Create a database
3. Create Database Users (keep your database user password)
4. Add user to the database, check all privileges and click "Make Changes"
5. Upload installable file to server
6. Unzip the installable file to server
7. Check permission of index.php file, make sure its file permission is set to 777

### Step 3: Installation Wizard

1. Open browser and hit your server URL (e.g., https://example.com/)
2. If all requirements are fulfilled, click "Next"
3. Provide database credentials and activation code, then click "Next"
4. Provide Admin login credentials and click "Finish"

Your database connection details:
- Database Host: your hosting name (e.g., localhost)
- Database User: the username with all privileges
- Password: the user password
- Database Name: your database name

Your admin account details:
- First Name: Administrator first name
- Last Name: Administrator last name
- Email: email for login (cannot be changed later)
- Password: password for admin login
- Activation code: obtained when purchasing the product from Envato

## System Architecture

CopysAI follows a modern MVC architecture with the following components:

### Core Components

1. **Backend Framework**: Laravel (PHP)
2. **Database**: MySQL
3. **Frontend**: Blade templates with JavaScript/jQuery/VueJS
4. **API Integration**: OAuth-based connections to multiple social platforms
5. **Scheduling System**: Laravel's task scheduling for social media posts

### Key Modules

1. **Account Management**: Handles connecting and managing social media accounts
2. **Post Management**: Creates, schedules, and publishes content across platforms
3. **AI Writer**: Generates social media content using AI
4. **Analytics**: Tracks performance metrics across platforms
5. **User Management**: Handles user roles, permissions, and subscription plans

## Client Management

To manage clients in CopysAI:

1. Go to Admin Panel and click "Manage Client"
2. Add new clients by clicking "Add Instructor" button
3. When adding a client, you can specify:
   - First Name
   - Last Name
   - Phone Number
   - Email Address
   - Password
   - Confirm Password
   - Company Name
   - Country
   - Address
   - Logo
4. You can edit client details by clicking the edit button
5. You can delete clients from the list
6. You can login as a client by clicking the "Login as Client" button

## General Settings

To configure general settings:

1. Go to Admin Panel and click "System Settings"
2. Navigate to "General Settings"
3. Here you can set:
   - System name
   - Default language
   - Default currency
   - System icon
   - Other global configurations

## Social Media Platform Integration

CopysAI integrates with multiple social platforms through their respective APIs. Below is the detailed configuration for each platform.

### Facebook Platform Configuration

#### 1. Configure the Facebook Platform in CopysAI

- Access the CopysAI Dashboard and navigate to the Platforms section
- Locate the Facebook platform in the list
- Click on the Settings button under the "Options" column
- In the Update Configuration modal, enter:
  - Client ID: Enter the Client ID from your Facebook developer account
  - Client Secret: Enter the Client Secret from your Facebook developer account
  - App Version: Specify the API version (e.g., v21.0)
  - Graph API URL: The default URL is https://graph.facebook.com
  - Group URL: The default URL is https://www.facebook.com/groups
  - Callback URL: Copy this URL to paste into your Facebook app's configuration

#### 2. Create a Facebook App

- Visit the [Facebook Developers website](https://developers.facebook.com/) and log in
- Click on My Apps and select Create App
- Enter your App Name and a valid email address
- Select "Other" and then "Business" as the app type
- If you have a verified business, select it during this step
- After creating the app, you'll be directed to the Facebook Developer dashboard
- Set up Login for Business with the callback URI from CopysAI

#### 3. Configure OAuth2 Redirect URIs

- For Login: `https://your-hosted-domain.com/login/facebook/callback`
- For Social Account Connectivity: `https://your-hosted-domain.com/account/facebook/callback?medium=facebook`

#### 4. Request Advanced Permissions

Required Permissions (Scopes):
- pages_show_list
- business_management
- pages_manage_posts
- pages_manage_engagement
- pages_read_engagement
- read_insights

#### 5. Retrieve App Credentials and Finalize Configuration

- Go to App Settings > Basic in the Facebook Developer dashboard
- Copy the App ID (Client ID) and App Secret (Client Secret)
- Ensure you've provided the App Domain, Privacy Policy URL, and Terms of Service URL
- Paste the App ID and App Secret into the CopysAI account configuration modal and save

#### 6. Submit App for Review and Go Live

- Complete the App Review process to make your app available to all users
- Provide detailed descriptions and demonstration videos for each permission requested
- Once approved, switch your app to Live mode

### Instagram Platform Configuration

#### 1. Steps to Configure

1. Navigate to the Platforms section in the CopysAI dashboard and locate the Facebook platform
2. Click on the Settings button under the "Options" column
3. In the Update Configuration modal, enter:
   - Client ID: Enter the Client ID from your Facebook developer account
   - Client Secret: Enter the Client Secret from your Facebook developer account
   - App Version: Specify the API version (e.g., v21.0)
   - Graph API URL: The default URL is https://graph.facebook.com
   - Group URL: The default URL is https://www.facebook.com/groups
   - Callback URL: Copy this URL to paste into your Facebook app's configuration
4. Once you've created the app in your Meta Developer account, add a new product:
   - Navigate to Menu options in the Meta Developer account
   - Select Instagram and click "Set Up"
   - Follow the subsequent steps to complete the setup

#### 2. API Setup for Instagram Business Login

Follow these steps to set up the Instagram API for Business Login in your Meta Developer account:

1. Generate Access Tokens:
   - Add an Instagram account to generate access tokens and set up webhook subscriptions
   - Click on the "Add Account" button to link your Instagram account

2. Configure Webhooks:
   - Set up a custom webhook URL or use services that help you create an endpoint
   - Ensure the app mode is set to Live to receive webhooks
   - Click the "Configure" button to set this up

3. Set Up Instagram Business Login:
   - Provide a secure way for businesses to grant your app permissions
   - Click the "Set Up" button to configure this

4. Complete App Review:
   - To access live data, your app must successfully complete the app review process
   - Go through the review to request advanced access to Instagram permissions
   - Submit your app for review when ready

#### 3. Important Information

- Instagram uses the same app infrastructure as Facebook (Meta)
- You'll need to request specific Instagram-related permissions during the app review process

### LinkedIn Platform Configuration

#### 1. Create a LinkedIn Developer App

1. Go to the [LinkedIn Developer Portal](https://developer.linkedin.com/) and log in
2. Click on "My Apps" button in the top-right corner
3. Click "Create App" and fill in the required fields:
   - App Name: Enter a suitable name for your app
   - LinkedIn Page: Provide your company's LinkedIn page URL
   - Privacy Policy URL: Provide a valid URL for your privacy policy
   - App Logo: Upload a logo for your app (minimum dimensions: 100px)
   - Legal Agreement: Check the box to agree to LinkedIn's API Terms of Use
4. Click "Create App" to complete the creation process

#### 2. Add Products

1. Navigate to the Products tab of your app
2. Add the following products:
   - Share on LinkedIn: Enables sharing content on LinkedIn
   - Sign In with LinkedIn: Allows authentication using LinkedIn credentials
3. If additional products like the Advertising API are needed, request access from LinkedIn

#### 3. Configure Authentication

1. Navigate to the Auth tab of your app
2. Copy the following details:
   - Client ID: Unique identifier for your app
   - Client Secret: Secure key for authentication
3. Set the Authorized Redirect URLs for your app to:
   `https://copysai.spagreen.net/account/linkedin/callback`
4. Save the changes

#### 4. Finalize the Setup

1. Ensure all settings are correctly configured, and the app status is set to Live
2. Use the Client ID, Client Secret, and Redirect URL to integrate CopysAI with LinkedIn

#### 5. Setup CopysAI LinkedIn Configuration

Provide all the necessary fields from the LinkedIn Developer account's created app in the CopysAI platform.

### Twitter/X Platform Configuration

#### 1. Create a New App in Twitter Developer Portal

1. Head over to the Twitter Developer Portal and log in to your account
2. Sign up for a new free developer account if you haven't already
3. Create a new app in the Projects & Apps section

#### 2. Configure Your App

1. Navigate to the newly created app and click on the Settings icon
2. In the User Authentication Settings, click on "Set Up" and follow these configurations:
   - App Permission: Set it to "Read and Write"
   - Type of App: Select "Web App, Automated App, or Bot"
   - Callback URI / Redirect URL: Enter the following URL:
     `https://copysai.com/account/twitter/callback?medium=twitter`
3. Save the settings

#### 3. OAuth2 Redirect URI

The OAuth2 Redirect URI is the location where Twitter will redirect after authentication:
- For production: `https://copysai.com/account/twitter/callback?medium=twitter`
- For localhost (development): Replace the domain with your local development URL

#### 4. Obtain Keys and Tokens

1. Go to the Keys and Tokens tab in your app settings
2. Copy the following credentials for use in CopysAI:
   - API Key
   - API Secret
   - Access Token
   - Access Token Secret

#### 5. Finalize and Save

Once you have completed the setup and entered the credentials in CopysAI's platform configuration, save the settings.

### TikTok Integration

TikTok integration requires a TikTok Developer account and application with the appropriate permissions.

### Threads Integration

Threads integration uses the same authentication as Instagram, as it's also owned by Meta.

## Social API Transfer Process

When transferring the application to new owners, the following steps will be necessary:

### 1. Developer Account Transfer

You'll need to transfer ownership or create new developer accounts on each platform:

- **Facebook/Instagram**: Transfer the Facebook Developer App or create a new one
- **LinkedIn**: Transfer LinkedIn Developer Application or create a new one
- **Twitter**: Transfer Twitter Developer Account/Project or create a new one
- **TikTok**: Transfer TikTok Developer Account or create a new one
- **Threads**: Uses the same authentication as Instagram

### 2. API Credentials Update

After transferring/creating developer accounts, update the following in the application:

- Client IDs
- Client Secrets
- Redirect URIs
- API Keys
- App IDs

These settings are stored in the system settings table and can be updated through the admin interface.

### 3. OAuth Callback Configuration

Update OAuth callback URLs in both:
- The application's environment settings
- Each platform's developer console

### 4. Platform-Specific Requirements

- **Facebook/Instagram**: Review app permissions and ensure Business Account access
- **LinkedIn**: Update OAuth scopes and redirect URIs
- **Twitter**: Ensure proper API access level (Essential, Elevated, or Enterprise)
- **TikTok**: Update developer account information and ensure proper permissions
- **Threads**: Update Instagram/Facebook credentials as needed

## Technical Implementation Details

The system uses trait-based implementation for each platform:
- `FacebookAccountTrait`
- `InstagramAccountTrait`
- `LinkedInAccountTrait`
- `TwitterAccountTrait`
- `TikTokAccountTrait`
- `ThreadsAccountTrait`

Each trait handles:
1. Authentication URL generation
2. Access token retrieval
3. Profile/Page data fetching
4. Content publishing
5. Media upload handling

## System Utilities

### System Update

To manage system updates:
- Select Utility in the left menu of the admin area
- Access the System Update section
- Here you can update your system with the latest version

### Server Information

In the Server Information section, you can find:
- System information
- Server information
- Extension libraries
- File system permissions

You can update status by tapping the switch button.

## Additional Resources

For more detailed documentation, visit the official documentation site at:
https://copysai.com/docs/
