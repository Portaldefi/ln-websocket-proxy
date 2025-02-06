# ln-websocket-proxy

A WebSocket-based proxy for connecting to Lightning nodes and Mutiny wallets.

## Docker

### Build the WebSocket-TCP Proxy Image

To build the Docker image, run:

```
DOCKER_BUILDKIT=1 docker build -f Dockerfile -t mutinywallet/ln-websocket-proxy .
```

### Run the Docker Image Locally

To run the image locally on your machine, use the following command:

```
docker run -d -p 3001:3001 mutinywallet/ln-websocket-proxy
```

### Deploy the Docker Image

To deploy the image to a registry, tag and push it:

```
docker tag mutinywallet/ln-websocket-proxy registry.digitalocean.com/mutiny-wallet/websocket-tcp-proxy
docker push registry.digitalocean.com/mutiny-wallet/websocket-tcp-proxy
```

## How to Test

You can change the default port by setting `LN_PROXY_PORT=3001` or whatever your desired port is.

To test the WebSocket connection, you'll need `netcat` and [`websocat`](https://github.com/vi/websocat) installed.

### Terminal 1: Run the WebSocket Server

Run the WebSocket server with the following command:

```
RUST_LOG=debug LN_PROXY_PORT=3002 cargo run --features="server"
```

### Terminal 2: Start `netcat`

For macOS:

```
netcat -l 127.0.0.1 -p 3000
```

For Linux:

```
nc -l 127.0.0.1 3000
```

### Terminal 3: Connect with `websocat`

Run `websocat` to connect to the WebSocket proxy:

```
websocat -b ws://127.0.0.1:3001/v1/127_0_0_1/3000
```

Now, you can type in the `websocat` terminal, and the text will appear in the `netcat` terminal. Similarly, typing in the `netcat` terminal will show the text in the `websocat` terminal.
