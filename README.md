# Spring Boot WebRTC Demo

A simple Spring Boot application demonstrating WebRTC peer-to-peer communication using WebSocket as a signaling server.

## Overview

This application provides a WebSocket-based signaling server that enables WebRTC peer-to-peer connections between
browsers. It demonstrates how to establish direct communication channels between clients for real-time data exchange
without requiring server-side message processing.

## Features

- WebSocket signaling server for WebRTC connections
- Peer-to-peer messaging using WebRTC DataChannel
- Simple web interface for testing WebRTC connections
- Support for multiple simultaneous peer connections
- Real-time message exchange between connected peers

## Technologies Used

- **Spring Boot**: 3.5.7
- **Java**: 21 (compatible with Java 21+)
- **WebSocket**: For signaling
- **WebRTC**: For peer-to-peer communication
- **Maven**: Build tool
- **Bootstrap**: Frontend styling

## Architecture

### Backend Components

1. **SpringbootWebrtc2Application**: Main Spring Boot application class
2. **SocketHandler**: WebSocket handler that relays messages between connected clients
3. **WebSocketConfiguration**: Configures WebSocket endpoint at `/socket`

### Frontend Components

1. **index.html**: Simple UI for creating offers and sending messages
2. **client.js**: WebRTC client implementation handling:
    - WebSocket connection to signaling server
    - Peer connection establishment
    - ICE candidate exchange
    - DataChannel creation and messaging

## How It Works

1. Clients connect to the WebSocket server at `ws://localhost:8080/socket`
2. One peer creates an offer using the "Create Offer" button
3. The offer is sent through the signaling server to other connected peers
4. The receiving peer automatically creates and sends back an answer
5. ICE candidates are exchanged through the signaling server
6. Once the connection is established, peers can send messages directly via DataChannel
7. Messages are logged to the browser console

## Prerequisites

- Java 21 or higher
- Maven 3.x

## Installation and Running

### 1. Build the application

```bash
mvn clean package
```

### 2. Run the application

```bash
java -jar target/springboot-webrtc2-0.0.1-SNAPSHOT.jar
```

Or using Maven:

```bash
mvn spring-boot:run
```

### 3. Access the application

Open your browser and navigate to:

```
http://localhost:8080
```

## Testing the Application

To test peer-to-peer communication:

1. Open the application in two different browser windows or tabs
2. In the first window, click "Create Offer"
3. The connection will be automatically established between the two peers
4. Type a message in the input field and click "SEND"
5. Check the browser console in both windows to see the messages being exchanged

## Configuration

The application uses default Spring Boot configuration:

- Server port: 8080
- WebSocket endpoint: `/socket`
- CORS: Allowed for all origins (suitable for development only)

## Project Structure

```
springboot-webrtc2/
├── src/
│   ├── main/
│   │   ├── java/com/hendisantika/springbootwebrtc2/
│   │   │   ├── SpringbootWebrtc2Application.java
│   │   │   ├── SocketHandler.java
│   │   │   └── WebSocketConfiguration.java
│   │   └── resources/
│   │       ├── static/
│   │       │   ├── index.html
│   │       │   └── client.js
│   │       └── application.properties
│   └── test/
├── pom.xml
└── README.md
```

## Key Implementation Details

### WebSocket Handler

The `SocketHandler` class maintains a list of active WebSocket sessions and relays messages between all connected
clients except the sender:

```java
public void handleTextMessage(WebSocketSession session, TextMessage message) {
    for (WebSocketSession webSocketSession : sessions) {
        if (webSocketSession.isOpen() && !session.getId().equals(webSocketSession.getId())) {
            webSocketSession.sendMessage(message);
        }
    }
}
```

### WebRTC Flow

1. **Offer**: Peer A creates an offer and sends it via WebSocket
2. **Answer**: Peer B receives the offer, creates an answer, and sends it back
3. **ICE Exchange**: Both peers exchange ICE candidates for NAT traversal
4. **Connection**: Once ICE negotiation completes, a direct peer connection is established
5. **Messaging**: Peers can now exchange messages through the DataChannel

## Limitations and Considerations

- This is a simplified demo for educational purposes
- CORS is set to allow all origins (not recommended for production)
- No authentication or authorization implemented
- Messages are only logged to console, not displayed in UI
- Suitable for development and testing environments only

## Author

**Hendi Santika**

- Email: hendisantika@gmail.com
- Telegram: @hendisantika34

## License

This project is a demo application for educational purposes.

## References

- [WebRTC API](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API)
- [Spring WebSocket](https://docs.spring.io/spring-framework/reference/web/websocket.html)
- [Spring Boot Documentation](https://docs.spring.io/spring-boot/documentation.html)
