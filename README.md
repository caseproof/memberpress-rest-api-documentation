# MemberPress REST API Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [API Base URL](#api-base-url)
3. [Authentication](#authentication)
4. [Core Resources and Endpoints](#core-resources-and-endpoints)
   - [Authentication & Permissions](#authentication--permissions)
   - [Members](#members)
   - [Memberships (Products)](#memberships-products)
   - [Transactions](#transactions)
   - [Subscriptions](#subscriptions)
   - [Groups](#groups)
   - [Rules](#rules)
   - [Coupons](#coupons)
   - [Reminders](#reminders)
   - [Events](#events)
5. [Webhook Integration](#webhook-integration)
6. [Search Parameters](#search-parameters)
7. [Pagination](#pagination)
8. [Missing Endpoints and Alternatives](#missing-endpoints-and-alternatives)
9. [Data Modeling and Relationships](#data-modeling-and-relationships)
10. [Code Examples](#code-examples)
    - [cURL Examples](#curl-examples)
    - [PHP Examples](#php-examples)
    - [JavaScript Examples](#javascript-examples)
11. [Legacy Integration](#legacy-integration)

## Introduction

MemberPress provides a comprehensive REST API through the MemberPress Developer Tools add-on. This API allows developers to programmatically interact with MemberPress data and functionality, including members, memberships, transactions, subscriptions, and more.

## API Base URL

All API endpoints use the base URL: `/wp-json/mp/v1/`

For example, if your site is at `https://example.com`, the API base URL would be:
```
https://example.com/wp-json/mp/v1/
```

## Authentication

The API supports two authentication methods:

1. **API Key Authentication** (Recommended): Using the API key generated in the Developer Tools settings
2. **WordPress User Authentication**: Must be logged in as a user with `remove_users` capability (typically admins)

For API key authentication, add the key to the Authorization header:
```
Authorization: YOUR_API_KEY
```

Example authentication test:
```bash
curl -X GET "https://example.com/wp-json/mp/v1/me" \
  -H "Authorization: YOUR_API_KEY"
```

## Core Resources and Endpoints

### Authentication & Permissions

#### Authentication Test
- **Endpoint**: `/me`
- **Method**: GET
- **Description**: Test authentication and get basic information about the authenticated user
- **Response**: `{"success": true, "data": {"username": "admin"}}`

#### User Permissions
- **Endpoint**: `/me/permissions`
- **Method**: GET
- **Description**: Get permissions for the authenticated user
- **Response**: List of permissions for the current user
- **Note**: This endpoint may not be available in all installations. See [Missing Endpoints and Alternatives](#missing-endpoints-and-alternatives).

### Members

#### List Members
- **Endpoint**: `/members`
- **Method**: GET
- **Description**: List all members
- **Query Parameters**:
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)
  - `search`: General search term
  - `search[id]`: Search by member ID
  - `search[email]`: Search by email
  - `search[username]`: Search by username
  - `orderby`: Field to order by
  - `order`: ASC or DESC

#### Get Member
- **Endpoint**: `/members/{id}`
- **Method**: GET
- **Description**: Get a specific member by ID

#### Create Member
- **Endpoint**: `/members`
- **Method**: POST
- **Description**: Create a new member
- **Parameters**:
  - `email`: (Required) Email address
  - `username`: (Required) Username
  - `password`: Password (auto-generated if not provided)
  - `first_name`: First name
  - `last_name`: Last name
  - `address1`: Address line 1
  - `address2`: Address line 2
  - `city`: City
  - `state`: State/province
  - `zip`: Postal code
  - `country`: Country code
  - `phone`: Phone number
  - `send_password_email`: Boolean to send password email
  - `transaction`: Nested transaction object to create along with member

#### Update Member
- **Endpoint**: `/members/{id}`
- **Method**: PUT
- **Description**: Update an existing member
- **Parameters**: Same as Create Member (except password)

#### Delete Member
- **Endpoint**: `/members/{id}`
- **Method**: DELETE
- **Description**: Delete a member

#### Member Options
- **Endpoint**: `/member_options`
- **Method**: GET
- **Description**: Get available options for members (roles, etc.)
- **Note**: This endpoint may not be available in all installations. See [Missing Endpoints and Alternatives](#missing-endpoints-and-alternatives).

### Memberships (Products)

#### List Memberships
- **Endpoint**: `/memberships`
- **Method**: GET
- **Description**: List all memberships
- **Query Parameters**:
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)
  - `search`: Search term
  - `search[id]`: Search by membership ID
  - `search[title]`: Search by title
  - `orderby`: Field to order by
  - `order`: ASC or DESC

#### Get Membership
- **Endpoint**: `/memberships/{id}`
- **Method**: GET
- **Description**: Get a specific membership by ID

#### Create Membership
- **Endpoint**: `/memberships`
- **Method**: POST
- **Description**: Create a new membership
- **Parameters**:
  - `title`: (Required) Membership name
  - `price`: Price
  - `period`: Billing period number
  - `period_type`: Billing period unit (days, weeks, months, years)
  - `trial`: Boolean for trial availability
  - `trial_days`: Number of trial days
  - `trial_amount`: Trial amount
  - `limit_cycles`: Boolean to limit billing cycles
  - `limit_cycles_num`: Number of cycles
  - `limit_cycles_action`: Action after cycles complete
  - `access_url`: Custom access URL
  - `thank_you_page_enabled`: Enable custom thank you page
  - `thank_you_page_type`: Thank you page type
  - `custom_thank_you_page_id`: Thank you page ID
  - `register_price_action`: Register price display option
  - `thank_you_message`: Custom thank you message
  - `custom_login_urls_enabled`: Enable custom login URLs
  - `custom_login_urls_default`: Default login URL
  - `custom_login_urls_expires`: Login URL for expired users
  - `is_highlighted`: Boolean to highlight in pricing
  - `group_id`: Group ID if part of a group

#### Update Membership
- **Endpoint**: `/memberships/{id}`
- **Method**: PUT
- **Description**: Update an existing membership
- **Parameters**: Same as Create Membership

#### Delete Membership
- **Endpoint**: `/memberships/{id}`
- **Method**: DELETE
- **Description**: Delete a membership

#### Membership Options
- **Endpoint**: `/membership_options`
- **Method**: GET
- **Description**: Get available options for memberships (period types, etc.)
- **Note**: This endpoint may not be available in all installations. See [Missing Endpoints and Alternatives](#missing-endpoints-and-alternatives).

#### Membership Member Options
- **Endpoint**: `/memberships/{id}/member_options`
- **Method**: GET
- **Description**: Get member-related options specific to a membership
- **Note**: This endpoint may not be available in all installations. See [Missing Endpoints and Alternatives](#missing-endpoints-and-alternatives).

### Transactions

#### List Transactions
- **Endpoint**: `/transactions`
- **Method**: GET
- **Description**: List all transactions
- **Query Parameters**:
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)
  - `search`: Search term
  - `search[id]`: Search by transaction ID
  - `search[member]`: Search by member ID
  - `search[membership]`: Search by membership ID
  - `search[subscription]`: Search by subscription ID
  - `search[status]`: Search by status
  - `orderby`: Field to order by
  - `order`: ASC or DESC

#### Get Transaction
- **Endpoint**: `/transactions/{id}`
- **Method**: GET
- **Description**: Get a specific transaction by ID

#### Create Transaction
- **Endpoint**: `/transactions`
- **Method**: POST
- **Description**: Create a new transaction
- **Parameters**:
  - `member`: (Required) Member ID
  - `membership`: (Required) Membership ID
  - `amount`: Transaction amount
  - `tax_amount`: Tax amount
  - `tax_rate`: Tax rate
  - `status`: Transaction status
  - `gateway`: Payment gateway
  - `subscription`: Subscription ID (if part of subscription)
  - `created_at`: Creation date
  - `expires_at`: Expiration date
  - `send_welcome_email`: Boolean to send welcome email

#### Update Transaction
- **Endpoint**: `/transactions/{id}`
- **Method**: PUT
- **Description**: Update an existing transaction
- **Parameters**: Same as Create Transaction

#### Delete Transaction
- **Endpoint**: `/transactions/{id}`
- **Method**: DELETE
- **Description**: Delete a transaction

#### Refund Transaction
- **Endpoint**: `/transactions/{id}/refund`
- **Method**: POST
- **Description**: Refund a transaction

#### Refund Transaction & Cancel Subscription
- **Endpoint**: `/transactions/{id}/refund_and_cancel`
- **Method**: POST
- **Description**: Refund a transaction and cancel its associated subscription

### Subscriptions

#### List Subscriptions
- **Endpoint**: `/subscriptions`
- **Method**: GET
- **Description**: List all subscriptions
- **Query Parameters**:
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)
  - `search`: Search term
  - `search[id]`: Search by subscription ID
  - `search[member]`: Search by member ID
  - `search[membership]`: Search by membership ID
  - `search[status]`: Search by status
  - `orderby`: Field to order by
  - `order`: ASC or DESC

#### Get Subscription
- **Endpoint**: `/subscriptions/{id}`
- **Method**: GET
- **Description**: Get a specific subscription by ID

#### Create Subscription
- **Endpoint**: `/subscriptions`
- **Method**: POST
- **Description**: Create a new subscription
- **Parameters**:
  - `member`: (Required) Member ID
  - `membership`: (Required) Membership ID
  - `status`: Subscription status
  - `period`: Billing period number
  - `period_type`: Billing period unit
  - `limit_cycles`: Boolean to limit billing cycles
  - `limit_cycles_num`: Number of cycles
  - `gateway`: Payment gateway
  - `trial`: Boolean for trial
  - `trial_days`: Trial days
  - `trial_amount`: Trial amount
  - `created_at`: Creation date

#### Update Subscription
- **Endpoint**: `/subscriptions/{id}`
- **Method**: PUT
- **Description**: Update an existing subscription
- **Parameters**: Same as Create Subscription

#### Delete Subscription
- **Endpoint**: `/subscriptions/{id}`
- **Method**: DELETE
- **Description**: Delete a subscription

#### Cancel Subscription
- **Endpoint**: `/subscriptions/{id}/cancel`
- **Method**: POST
- **Description**: Cancel a subscription

#### Suspend Subscription
- **Endpoint**: `/subscriptions/{id}/suspend`
- **Method**: POST
- **Description**: Suspend a subscription

#### Resume Subscription
- **Endpoint**: `/subscriptions/{id}/resume`
- **Method**: POST
- **Description**: Resume a suspended subscription

### Groups

#### List Groups
- **Endpoint**: `/groups`
- **Method**: GET
- **Description**: List all membership groups
- **Query Parameters**:
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)
  - `search`: Search term
  - `orderby`: Field to order by
  - `order`: ASC or DESC

#### Get Group
- **Endpoint**: `/groups/{id}`
- **Method**: GET
- **Description**: Get a specific group by ID

#### Create Group
- **Endpoint**: `/groups`
- **Method**: POST
- **Description**: Create a new group
- **Parameters**:
  - `title`: (Required) Group name
  - `pricing_page_disabled`: Boolean to disable on pricing page
  - `group_theme`: Theme for group display
  - `is_upgrade_path`: Boolean for upgrade path
  - `upgrade_path_text`: Text for upgrade path
  - `pricing_display`: Display option for pricing
  - `page_id`: Page ID
  - `url`: URL for group page

#### Update Group
- **Endpoint**: `/groups/{id}`
- **Method**: PUT
- **Description**: Update an existing group
- **Parameters**: Same as Create Group

#### Delete Group
- **Endpoint**: `/groups/{id}`
- **Method**: DELETE
- **Description**: Delete a group

### Rules

#### List Rules
- **Endpoint**: `/rules`
- **Method**: GET
- **Description**: List all access rules
- **Query Parameters**:
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)
  - `search`: Search term
  - `orderby`: Field to order by
  - `order`: ASC or DESC

#### Get Rule
- **Endpoint**: `/rules/{id}`
- **Method**: GET
- **Description**: Get a specific rule by ID

#### Create Rule
- **Endpoint**: `/rules`
- **Method**: POST
- **Description**: Create a new access rule
- **Parameters**:
  - `title`: (Required) Rule name
  - `rule_type`: Type of rule
  - `content_type`: Content type
  - `content_id`: Content ID
  - `drip_enabled`: Boolean for content dripping
  - `drip_amount`: Drip amount
  - `drip_unit`: Drip unit
  - `drip_after`: Drip after event
  - `drip_after_fixed`: Fixed date for dripping
  - `expires_enabled`: Boolean for expiration
  - `expires_amount`: Expiration amount
  - `expires_unit`: Expiration unit
  - `expires_after`: Expires after event
  - `expires_after_fixed`: Fixed date for expiration
  - `unauth_excerpt_type`: Unauthorized excerpt type
  - `unauth_excerpt_size`: Unauthorized excerpt size
  - `unauth_message_type`: Unauthorized message type
  - `unauth_message`: Unauthorized message
  - `unauth_login`: Show login form
  - `memberships`: Array of membership IDs that can access

#### Update Rule
- **Endpoint**: `/rules/{id}`
- **Method**: PUT
- **Description**: Update an existing rule
- **Parameters**: Same as Create Rule

#### Delete Rule
- **Endpoint**: `/rules/{id}`
- **Method**: DELETE
- **Description**: Delete a rule

### Coupons

#### List Coupons
- **Endpoint**: `/coupons`
- **Method**: GET
- **Description**: List all coupons
- **Query Parameters**:
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)
  - `search`: Search term
  - `orderby`: Field to order by
  - `order`: ASC or DESC

#### Get Coupon
- **Endpoint**: `/coupons/{id}`
- **Method**: GET
- **Description**: Get a specific coupon by ID

#### Create Coupon
- **Endpoint**: `/coupons`
- **Method**: POST
- **Description**: Create a new coupon
- **Parameters**:
  - `code`: (Required) Coupon code
  - `discount_type`: Type of discount
  - `discount_amount`: Discount amount
  - `valid_date`: Start date
  - `expiration_date`: Expiration date
  - `usage_amount`: Maximum usage count
  - `trial`: Boolean for trial
  - `trial_days`: Trial days
  - `should_apply_tax`: Boolean to apply tax
  - `memberships`: Array of membership IDs

#### Update Coupon
- **Endpoint**: `/coupons/{id}`
- **Method**: PUT
- **Description**: Update an existing coupon
- **Parameters**: Same as Create Coupon

#### Delete Coupon
- **Endpoint**: `/coupons/{id}`
- **Method**: DELETE
- **Description**: Delete a coupon

### Reminders

#### List Reminders
- **Endpoint**: `/reminders`
- **Method**: GET
- **Description**: List all reminders
- **Query Parameters**:
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)
  - `search`: Search term
  - `orderby`: Field to order by
  - `order`: ASC or DESC

#### Get Reminder
- **Endpoint**: `/reminders/{id}`
- **Method**: GET
- **Description**: Get a specific reminder by ID

#### Create Reminder
- **Endpoint**: `/reminders`
- **Method**: POST
- **Description**: Create a new reminder
- **Parameters**:
  - `name`: (Required) Reminder name
  - `event`: Event to trigger reminder
  - `trigger_length`: Trigger timing length
  - `trigger_interval`: Trigger timing interval
  - `trigger_timing`: Before or after event
  - `enabled`: Boolean to enable/disable
  - `subject`: Email subject
  - `body`: Email body
  - `use_template`: Boolean to use template
  - `memberships`: Array of membership IDs

#### Update Reminder
- **Endpoint**: `/reminders/{id}`
- **Method**: PUT
- **Description**: Update an existing reminder
- **Parameters**: Same as Create Reminder

#### Delete Reminder
- **Endpoint**: `/reminders/{id}`
- **Method**: DELETE
- **Description**: Delete a reminder

### Events

#### List Events
- **Endpoint**: `/events`
- **Method**: GET
- **Description**: List all events
- **Query Parameters**:
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)
  - `search`: Search term
  - `search[event]`: Search by event type
  - `orderby`: Field to order by
  - `order`: ASC or DESC

## Webhook Integration

MemberPress allows you to subscribe to events via webhooks to receive notifications when certain actions occur.

#### Subscribe to Events
- **Endpoint**: `/webhooks/subscribe`
- **Method**: POST
- **Description**: Subscribe to MemberPress events
- **Parameters**:
  - `url`: (Required) Callback URL for notifications
  - `event`: (Required) Event type or "all" for all events

#### Unsubscribe from Events
- **Endpoint**: `/webhooks/unsubscribe/{id}`
- **Method**: DELETE
- **Description**: Unsubscribe from webhook

### Available Events for Webhooks

The following events can be subscribed to with webhooks:

- `transaction-completed`: When a transaction is completed
- `transaction-refunded`: When a transaction is refunded
- `transaction-failed`: When a transaction fails
- `transaction-expired`: When a transaction expires
- `recurring-transaction-completed`: When a recurring transaction is completed
- `recurring-transaction-failed`: When a recurring transaction fails
- `subscription-created`: When a subscription is created
- `subscription-paused`: When a subscription is paused
- `subscription-resumed`: When a subscription is resumed
- `subscription-stopped`: When a subscription is stopped
- `subscription-upgraded`: When a subscription is upgraded
- `subscription-downgraded`: When a subscription is downgraded
- `member-signup`: When a member signs up
- `member-account-updated`: When a member account is updated

## Search Parameters

Most list endpoints support advanced search with parameters like:

- `search[id]=123`: Search by ID
- `search[email]=user@example.com`: Search by email
- `search[username]=john`: Search by username 
- `search[status]=active`: Search by status

Examples:
```bash
# Search members by email
curl -X GET "https://example.com/wp-json/mp/v1/members?search[email]=user@example.com" \
  -H "Authorization: YOUR_API_KEY"

# Search transactions by member ID
curl -X GET "https://example.com/wp-json/mp/v1/transactions?search[member]=123" \
  -H "Authorization: YOUR_API_KEY"
```

## Pagination

All list endpoints support pagination with:
- `page`: Page number (default: 1)
- `per_page`: Items per page (default: 10)

Response headers include:
- `X-WP-Total`: Total number of items
- `X-WP-TotalPages`: Total number of pages

Example:
```bash
# Get page 2 with 20 items per page
curl -X GET "https://example.com/wp-json/mp/v1/members?page=2&per_page=20" \
  -H "Authorization: YOUR_API_KEY"
```

## Missing Endpoints and Alternatives

Some endpoints documented may not be available in all installations:

- `/membership_options`
- `/member_options`
- `/me/permissions`
- `/memberships/{id}/member_options`

### Alternative for `/membership_options`

While the dedicated options endpoint may be missing, you can extract options from existing memberships:

```bash
# Get a specific membership to see available fields/options
curl -X GET "https://example.com/wp-json/mp/v1/memberships/123" \
  -H "Authorization: YOUR_API_KEY"
```

Key option fields in the response include:
- `period_type`: Available values include "lifetime", "days", "weeks", "months", "years"
- `trial_once`: Boolean for one-time trial option
- `tax_class`: Available tax classes
- `pricing_display`: Display options
- `custom_template`: Available templates

### Alternative for `/member_options`

Similarly, member options can be extracted from a member object:

```bash
# Get a specific member to see available fields/options
curl -X GET "https://example.com/wp-json/mp/v1/members/123" \
  -H "Authorization: YOUR_API_KEY"
```

Key information includes:
- Address field structure
- Profile fields structure
- Transaction data structure
- Subscription data structure

### Alternative for `/me/permissions`

While there's no direct endpoint for permissions, you can:

1. Use the `/me` endpoint to get your authenticated identity
2. Try accessing other endpoints to determine your permissions empirically

The `/events` endpoint also provides insights into what actions are tracked and potentially what permissions exist in the system.

### Alternative for `/memberships/{id}/member_options`

The relationship between memberships and members can be explored through:

1. Getting a specific membership details: `/memberships/{id}`
2. Searching transactions for a specific membership: `/transactions?search[membership]={id}`
3. Searching members with active memberships: Inspect the `active_memberships` array in member responses

## Data Modeling and Relationships

Understanding the relationships between resources is crucial for effectively using the API:

1. **Members** (WordPress users) can have:
   - Multiple transactions (purchases)
   - Multiple subscriptions
   - Multiple active memberships

2. **Memberships** (products) can:
   - Be purchased by multiple members
   - Be associated with multiple transactions
   - Be part of a group
   - Have specific rules for content access

3. **Transactions** are always linked to:
   - One member (buyer)
   - One membership (product purchased)
   - Optionally one subscription (if recurring)

4. **Subscriptions** are always linked to:
   - One member (subscriber)
   - One membership (subscribed product)
   - Multiple transactions (recurring payments)

## Code Examples

### cURL Examples

#### Authentication
```bash
curl -X GET "https://example.com/wp-json/mp/v1/me" \
  -H "Authorization: YOUR_API_KEY"
```

#### List Members
```bash
curl -X GET "https://example.com/wp-json/mp/v1/members" \
  -H "Authorization: YOUR_API_KEY"
```

#### Create Member
```bash
curl -X POST "https://example.com/wp-json/mp/v1/members" \
  -H "Authorization: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "new_customer@example.com",
    "username": "new_customer",
    "password": "securePassword123",
    "first_name": "Jane",
    "last_name": "Smith"
  }'
```

#### Create Transaction
```bash
curl -X POST "https://example.com/wp-json/mp/v1/transactions" \
  -H "Authorization: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "member": 123,
    "membership": 456,
    "amount": 25.00,
    "status": "complete",
    "gateway": "manual"
  }'
```

#### Create Subscription
```bash
curl -X POST "https://example.com/wp-json/mp/v1/subscriptions" \
  -H "Authorization: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "member": 123,
    "membership": 456,
    "status": "active",
    "period": 1,
    "period_type": "months",
    "gateway": "manual"
  }'
```

#### Search and Advanced Filtering
```bash
# Search by ID
curl -X GET "https://example.com/wp-json/mp/v1/members?search[id]=123" \
  -H "Authorization: YOUR_API_KEY"

# Get active subscriptions
curl -X GET "https://example.com/wp-json/mp/v1/subscriptions?search[status]=active" \
  -H "Authorization: YOUR_API_KEY"

# Get transactions for a specific membership
curl -X GET "https://example.com/wp-json/mp/v1/transactions?search[membership]=456" \
  -H "Authorization: YOUR_API_KEY"
```

### PHP Examples

#### Basic API Request Function
```php
<?php
function makeApiRequest($endpoint, $method = 'GET', $data = null) {
    $api_key = 'YOUR_API_KEY';
    $base_url = 'https://example.com/wp-json/mp/v1';
    
    $url = $base_url . $endpoint;
    $ch = curl_init();
    
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($ch, CURLOPT_HTTPHEADER, [
        "Authorization: $api_key"
    ]);
    
    if ($method === 'POST' || $method === 'PUT') {
        curl_setopt($ch, CURLOPT_CUSTOMREQUEST, $method);
        if ($data) {
            curl_setopt($ch, CURLOPT_HTTPHEADER, [
                "Authorization: $api_key",
                "Content-Type: application/json"
            ]);
            curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
        }
    }
    
    $response = curl_exec($ch);
    curl_close($ch);
    
    return json_decode($response, true);
}
```

#### List Members
```php
<?php
$members = makeApiRequest('/members');
print_r($members);
```

#### Create Transaction
```php
<?php
$data = [
    "member" => 123,
    "membership" => 456,
    "amount" => 25.00,
    "status" => "complete",
    "gateway" => "manual"
];

$result = makeApiRequest('/transactions', 'POST', $data);
print_r($result);
```

#### Complete Registration Flow
```php
<?php
// 1. Create a member
$memberData = [
    'email' => 'user@example.com',
    'username' => 'newuser',
    'password' => 'securePassword',
    'first_name' => 'John',
    'last_name' => 'Doe'
];
$member = makeApiRequest('/members', 'POST', $memberData);
$memberId = $member['id'];

// 2. Create a transaction
$transactionData = [
    'member' => $memberId,
    'membership' => 456, // Membership ID
    'amount' => 29.99,
    'status' => 'complete',
    'gateway' => 'manual'
];
$transaction = makeApiRequest('/transactions', 'POST', $transactionData);

// 3. Create a subscription (if recurring)
$subscriptionData = [
    'member' => $memberId,
    'membership' => 456, // Membership ID
    'status' => 'active',
    'period' => 1,
    'period_type' => 'months',
    'gateway' => 'manual'
];
$subscription = makeApiRequest('/subscriptions', 'POST', $subscriptionData);
```

### JavaScript Examples

#### List Memberships
```javascript
fetch("https://example.com/wp-json/mp/v1/memberships", {
  method: "GET",
  headers: {
    "Authorization": "YOUR_API_KEY"
  }
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error("Error:", error));
```

#### Create Member
```javascript
const memberData = {
  email: "api_user@example.com",
  username: "api_user",
  password: "SecurePassword123",
  first_name: "API",
  last_name: "User"
};

fetch("https://example.com/wp-json/mp/v1/members", {
  method: "POST",
  headers: {
    "Authorization": "YOUR_API_KEY",
    "Content-Type": "application/json"
  },
  body: JSON.stringify(memberData)
})
.then(response => response.json())
.then(data => console.log(data))
.catch(error => console.error("Error:", error));
```

## Legacy Integration

For backward compatibility, MemberPress also provides a WordPress Ajax endpoint:

### User Information
- **Endpoint**: `/wp-admin/admin-ajax.php?action=mepr_user`
- **Method**: GET
- **Authentication**: Basic Auth (username/password)
- **Response Formats**: JSON (default), XML (add `&fmt=xml`), CSV (add `&fmt=csv`)

### PHP Direct Integration

Developers can also integrate directly with MemberPress using its PHP classes:

```php
// Get a user
$user = new MeprUser(123);

// Check if user has access to a membership
$has_access = $user->has_access_to_product(456);

// Get active subscriptions
$subscriptions = $user->active_product_subscriptions();

// Record custom event
MeprEvent::record('custom-event-name', $user);
```
