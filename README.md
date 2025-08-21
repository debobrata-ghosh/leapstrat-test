# WordPress Custom Post Type and ACF Integration

## Overview
This project demonstrates how to create a custom post type "User Details" and integrate it with Advanced Custom Fields (ACF) to display user data from the Random User API.

## Prerequisites
- WordPress installation
- Custom Post Type UI (CPT UI) plugin
- Advanced Custom Fields (ACF) plugin

## Setup Instructions

### 1. Create Custom Post Type "User Details" using CPT UI plugin

1. Install and activate the **Custom Post Type UI** plugin
2. Go to **CPT UI → Add/Edit Post Types**
3. Configure the following settings:
   - **Post Type Slug**: `user_details`
   - **Plural Label**: `User Details`
   - **Singular Label**: `User Detail`
   - **Post Type**: `Post`
   - **Public**: ✓ Checked
   - **Show UI**: ✓ Checked
   - **Show in Menu**: ✓ Checked
   - **Show in Admin Bar**: ✓ Checked
   - **Show in REST API**: ✓ Checked
4. Click **Add Post Type**

### 2. Create Custom "Select User" Field using ACF plugin

1. Install and activate the **Advanced Custom Fields** plugin
2. Go to **Custom Fields → Add New**
3. Configure the field group:
   - **Field Group Title**: `User Details Fields`
   - **Location Rules**: 
     - **Show this field group if**: `Post Type` `is equal to` `User Details`
4. Add a new field:
   - **Field Label**: `Select User`
   - **Field Name**: `select_user`
   - **Field Type**: `Select`
   - **Choices**: Leave empty (will be populated via API)
   - **Return Format**: `Value`
   - **Allow Null**: ✓ Checked
   - **Multiple**: Leave unchecked
5. Save the field group

### 3. Populate Select Field with API Data

The select field values are automatically populated using the Random User API (https://randomuser.me/api/) when creating a new "User Details" post.

#### API Integration Features:
- **API Endpoint**: `https://randomuser.me/api/?results=10`
- **Minimum Users**: 10 users per request
- **Data Retrieved**: 
  - First Name
  - Last Name
  - Email Address
- **Display Format**: "First Last (email@example.com)"
- **Caching**: 24-hour cache to improve performance

#### Technical Implementation:
The integration is handled through WordPress hooks and the ACF filter system:

```php
add_filter('acf/load_field/name=select_user', 'populate_user_select_field', 10, 1);
```

**Key Features:**
- **Automatic Population**: Select field populates when ACF loads
- **Caching System**: Uses WordPress transients for 24-hour caching
- **Error Handling**: Graceful fallback with user-friendly error messages
- **Data Validation**: Ensures valid email addresses and complete names
- **Performance Optimized**: Single API call fetches 10 users at once
- **Secure**: Uses `wp_remote_get()` with SSL verification

**Data Processing:**
- Fetches 10 random users from the API
- Validates email addresses using WordPress `is_email()`
- Sanitizes data using `sanitize_email()`
- Formats names using `ucwords()` for proper capitalization
- Creates key-value pairs for the select field

## Usage

1. Navigate to **Posts → User Details**
2. Click **Add New**
3. The "Select User" field will automatically populate with 10 users from the API
4. Choose a user from the dropdown (formatted as "Name (email@example.com)")
5. Publish your post

## File Structure
