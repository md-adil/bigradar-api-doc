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


## Websocket

### Send Message
    socket.emit('server/message/send', {
        text: 'Some text'
        conversation_id: 'conversation',
        type: 'text'
    }, message => {
        // callback
        // when message delivered, this callback willbe called
    })

### Message Read
when message has been read trigger this event

    socket.emit('client/message/read', {
        id:  // messgeid,
        conversation_id:  conversationid//
    })

### Conversation Read
when open the conversation means all messages in the conversation has been read

    socket.emit('client/conversation/read', {
        id: 'conversationid' // The Conversation id
    });

### Start Typing
    socket.emit('client/typing/start', { 
        id: 'conversattionid'
    })

### Stop Typing
    socket.emit('client/typing/start', {
        id: 'conversationid'
    });

### When new conversation
    socket.on('new conversation', payload => {
        // add new conversation in the list
    });
    payload
    {
        _id: '5afd9c3ffc69e310b79a9dba' // Conversation id
        message : {
            text: "Hey There",
            createdAt: "2018-05-17T15:16:24.835Z" // Message time
        }
        user : {
            _id: "5afd9bc4.3fc69e310b79a9dad", // User id
            name: "Tressa Hills", // User name
            status: "offline" // User status either offline or online,
            location: { // location is optional
                city: "Thiruvananthapuram",
                country: "India",
                isp: "Jio",
                lat: 8.5069,
                lon: 76.9569,
                region: "Kerala"
            }
        }
        status : "open" // message status either open, close or trashed
        totalMessage : 4
        unreadMessage : 0
        updatedAt : "2018-05-17T15:14:27.398Z"
        city : "Thiruvananthapuram",
        country : "India"
    }


### When new message
    socket.on('server/message/send', payload => {
        // add message to conversation
    })
    payload
    {
        _id: '5afd9c3ffc69e310b79a9dbb', // Message id
        user: {
            name: 'Adil'
        },
        isAuthor: false,
        text: 'Hi there',
        type: 'text',
        email: 'md-adil@live.com',
        read: true', // is message read
        referer: 'https://bigradar.io',
        createdAt: 'Thu May 17 2018 20:44:53 GMT+0530 (IST)'
    }

### When Conversation read
    socket.on('server/conversation/read', payload => {
		// Do update your view and db
	});

#### When Message Read
    socket.on('server/message/read', payload => {
		// Do update your view and db
	});

### When something when wrong and server disconnected
    socket.on('error', error => {
        // try to reconnect
    });

    socket.on('exception', error => {
        // check configuration
    });

### When connection destroyed by server
    socket.on('socket destroyed', socket => {
        // Destroy socket
    });

### User update their email of phone
    socket.on('user update', payload => {
        // updated payload
    })

### When message updated
    socket.on('update message', payload => {
        // update message payload
    });

### When connect or disconnect
    socket.on('connect', socket => {

    });
	socket.on('disconnect', socket => {

    });

### When typing
	socket.on('server/typing/start', payload => {
        id: 'conversationid'
    });
	socket.on('server/typing/stop', payload => {
        id: 'conversationid'
    });

### When user leav or join
    socket.on('server/users/leave', payload => {
        
    });
    payload
    {
        _id: 'userid' // User id
    }
    
	socket.on('server/users/join', payload => {

    });