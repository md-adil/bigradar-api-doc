# BigRadar Api DOC

### Base URL
    https://app.bigradar.io/api

## Authentication
### Login
    /login - POST
#### Header
    Content-type: application/json

| Attributes | Type   | Description            |
|------------|--------|------------------------|
| email      | string | User email id to login |
| password   | string | user password          |

#### Returns
on Fails:

422 http error with error message object

on Success:

token object which is used to make further request.

## Authenticated request
Entire authenticated request expect token in header
#### Header
    content-type: application/json
    authorization: <user token>

### Logout
    /logout - POST
### Logged user information like name, avatar, etc.
    /me - GET 

## Conversations
### Conversation List
    /conversations - GET
### Get perticular conversation
    /conversations/:id - GET

### Get Messages by Conversation
    /conversations/:id/messages - GET
### Close Conversation
    /conversations/:id/close - POST
### Delete Conversation
    /conversations/:id/delete - DELETE
### Assign Conversation
    /conversations/:conversation/assign/:user?
### Update user by conversation id
    /conversations/:id/user/update

## Agents
### List of Agents
    /agents - GET

### Create Agent
    /agents
### Delete Agent
    /agents/:id/delete - DELETE
### Update active agent status
    /agents/:id/status/:status - POST
### Delete Pending Agents
    /agents/:id/delete-pending-agent - DELETE

## Users
### Get all users
    /users
### Get perticular user information
    /users/:id
### Conversations by user
    /users/:id/conversations
### Update user info
    /users/:user - PUT

## Settings
### Get Notifications
    /notifications - GET
### Update Notification
    /notifications - PUT
### Set FCM Token
    /token/fcm - POST
### Get FCM Token
    /token/fcm - GET
