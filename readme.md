# BigRadar Api DOC

## Base URL

    https://app.bigradar.io/api

## Authentication

### Login

    /login - POST

#### Header

    Content-type: application/json

| Attributes | Type   | Description            |
| ---------- | ------ | ---------------------- |
| email      | string | User email id to login |
| password   | string | user password          |

#### Response on success

    interface ILoginResponse {
        token: string;
        action?: "verify-email" | "add-organization";
    }
    // status code 200

#### response on fail

    error message
    // status code 422

## Authenticated request

Entire authenticated request expect token in header

#### Header

    content-type: application/json
    authorization: <token>

### Logout

    /logout - POST

### Logged user information like name, avatar, etc.

    /me - GET

###

    interface IUserOrganization {
        _id: string;
        name: string;
        default: boolean;
        role: "admin" | "agent"
    }
    interface IUser {
        name: string;
        email: string;
        avatar?: string;
    }

    interface Response {
        user: IUser;
        organizations: IUserOrganization[];
    }

## Conversations

### Conversation List

    /conversations - GET

##

    interface ILocation {
        city: string;
        country: string;
        isp: string;
        lat: string;
        lon: string;
        region: string;
        pincode: string;
    }

    interface IUserResponse {
        _id: string;
        name?: string;
        status: number; // sometime it can return string in old data, if cause the error, i wil make make it done.
        location?: ILocation;
    }

    interface IMessageResponse {
        text: string;
        createdAt: Date;
    }

    interface IConversationResponse {
        _id: string;
        status: ConversationStatus;
        updatedAt: Date;
        user: IUserResponse;
        message: IMessageResponse;
        totalMessage?: number;
        unreadMessage?: number;
    }

    Response {
        data: IConversationResponse[]
    }

### Get perticular conversation

    /conversations/:id - GET

###

    {
        "status": "open",
        "_id": "5afdc180fc69e310b79aa17c",
        "user_id": "5afdc17ffc69e310b79aa17b",
        "admin_id": "591bea3019dac5ffdba75007",
        "organization": "kw1DhZo1yOCyy2A5Qc5WL3WD",
        "createdAt": "2018-05-17T17:53:04.199Z",
        "updatedAt": "2018-05-18T10:47:05.694Z",
        "__v": 0,
        "contactAsked": true,
        "user": {
            "ref": "BkPY-Hj0M",
            "organizations": [],
            "type": "user",
            "status": "offline",
            "tags": [],
            "createdAt": "2018-05-17T17:53:03.224Z",
            "_id": "5afdc17ffc69e310b79aa17b",
            "ip_addr": "128.199.118.86",
            "name": "Miguel Connelly",
            "organization": "kw1DhZo1yOCyy2A5Qc5WL3WD",
            "browser": {
                "name": "Chrome",
                "version": "66.0.3359.158",
                "os": "Android",
                "os_version": "5.1.1",
                "engine": "WebKit",
                "device": "mobile",
                "device_model": "oG3",
                "device_vendor": "Motorola"
            },
            "__v": 0,
            "location": {
                "city": "Singapore",
                "country": "Singapore",
                "isp": "DigitalOcean",
                "lat": 1.2931,
                "lon": 103.8558,
                "region": "Central Singapore Community Development Council",
                "pincode": ""
            },
            "lastSeen": "2018-05-18T10:55:45.337Z"
        }
    }

### Get Messages by Conversation

    /conversations/:id/messages - GET

###

    type MessageStatus = "sent" | "read" | "respond" | "closed" | "email:sent" | "email:delivered" | "email:bounce" | "email:open";
    type MessageType = "text" | "form" | "info" | "option" | "campaign";
    interface IMessageResponse {
        _id: string;
        text: string;
        isAuthor: boolean;
        conversation_id: string;
        status: MessageStatus;
        readAt?: Date;
        type: MessageType;
        user: {
            _id: string;
            name: string;
        };
        options?: IOption[];
        createdAt: Date;
        repliedAt?: Date;
        entity?: MessageEntity;
    }

    Response {
        data: IMessageResponse[]
    }

### Close Conversation

    /conversations/:id/close - POST

###

    Conversation closed

### Delete Conversation

    /conversations/:id/delete - POST

###

    Conversation has been deleted!

### Assign Conversation

    /conversations/:conversation/assign/:user - PUT

###

    :user

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

###

    {
        "email": {
            "newConversation": true,
            "newMessage": true
        },
        "web": {
            "newConversation": true,
            "newMessage": true
        },
        "phone": {
            "newConversation": true,
            "newMessage": true
        }
    };

### Update Notification

    /notifications - PUT

#### request payload

    "notifications": {
        "email": {
            "newConversation": true,
            "newMessage": true
        },
        "web": {
            newConversation: true,
            newMessage: true
        },
        "phone": {
            newConversation: true,
            newMessage: true
        }
    };

#### Response

    "email": {
        "newConversation": true,
        "newMessage": true
    },
    "web": {
        newConversation: true,
        newMessage: true
    },
    "phone": {
        newConversation: true,
        newMessage: true
    }

### Set FCM Token

    /token/fcm - POST

### Get FCM Token

    /token/fcm - GET

## Websocket

### Send Message

    socket.emit('client/message/send', {
        text: 'Some text'
        conversation_id: '5afd7d0ffc69e310b79a9a15',
        type: 'text'
    }, (message: IMessageResponse) => {
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

#### payload

    IConversationResponse

### When new message

    socket.on('server/message/send', (payload: IMessageResponse) => {
        // add message to conversation
    })

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

### When user get offline

    socket.on('server/users/leave', payload => {

    });

#### Payload

    {
        user_id: '5afe5d66fc69e310b79aa7f3' // User id
    }

### When user get online

    socket.on('server/users/join', payload => {

    });

#### Payload

    {
        user_id: '5afe5d66fc69e310b79aa7f3'
    }
