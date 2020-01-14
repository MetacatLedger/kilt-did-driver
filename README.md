<p align="center">
<img width="220" src="https://user-images.githubusercontent.com/9762897/67468312-9176b700-f64a-11e9-8d88-1441380a71f6.jpg">  
  <div align="center"><sup><a href="https://kilt.io">kilt.io</a></sup></div> 
</p>

# KILT DID Driver

This driver resolves a given KILT [Decentralized Identifier](https://w3c-ccg.github.io/did-spec/) to the associated DID Document.

Among others, this driver can be used in the Decentralized Identity Foundation's [Universal Resolver](https://github.com/decentralized-identity/universal-resolver).

## About

A containerized version of this driver is available on [KILT Protocol's dockerhub](https://hub.docker.com/r/kiltprotocol/kilt-did-driver).

## Dev setup

### Run with node

- (Once only: Install the required dependencies with `yarn`)
- Start the web server:

  ```bash
  node src/index.js
  ```

- Make a request:

  ```bash
  curl -X GET http://localhost:8080/1.0/identifiers/did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE
  # expected output: DID Document as JSON:
    {"id":"did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE","authentication":{"type":"Ed25519SignatureAuthentication2018","publicKey":["did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE#key-1"]},"publicKey":[{"id":"did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE#key-1","type":"Ed25519VerificationKey2018","controller":"did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE","publicKeyHex":"0x06cc7535ee1ea0f552085215de0424af463a28f41c7b72fe2bd877d92a95d021"},{"id":"did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE#key-2","type":"X25519Salsa20Poly1305Key2018","controller":"did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE","publicKeyHex":"0xf93b411bc9f56a27c7b248dc5065391883d8cc412aac05084313bfcf9ab4e803"}],"@context":"https://w3id.org/did/v1","service":[{"type":"KiltMessagingService","serviceEndpoint":"//services.kilt.io:443/messaging"}]}
  ```

### Run with docker

In `kilt-did-driver`:

```bash
docker build -t kiltprotocol/kilt-did-driver .
# return a container ID e.g. 4cf5867afbce40876a5ca2467bdb14407199a2eda29a89df1f98514c77cce6bc:
docker run -p 49160:8080 -d kiltprotocol/kilt-did-driver:latest
# see a list of container IDs, copy the relevant one e.g. 16df22cc7c5f:
docker ps
# see the logs for your container (16df22cc7c5f is the container id copied at the previous step):
docker logs 16df22cc7c5f
```

The container should be running and logs should be visible.
To query, run (note the port `49160` due to port mapping):

`curl -X GET http://localhost:49160/1.0/identifiers/did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE`

The response should be the following DID Document as JSON:


5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE

```json
{
  "id": "did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE",
  "authentication": {
    "type": "Ed25519SignatureAuthentication2018",
    "publicKey": [
      "did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE#key-1"
    ]
  },
  "publicKey": [
    {
      "id": "did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE#key-1",
      "type": "Ed25519VerificationKey2018",
      "controller": "did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE",
      "publicKeyHex": "0x06cc7535ee1ea0f552085215de0424af463a28f41c7b72fe2bd877d92a95d021"
    },
    {
      "id": "did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE#key-2",
      "type": "X25519Salsa20Poly1305Key2018",
      "controller": "did:kilt:5CDct4QDpQYfAVDrskNuiEdXyiE38oPfTHEJ65ZLSpz9WasE",
      "publicKeyHex": "0xf93b411bc9f56a27c7b248dc5065391883d8cc412aac05084313bfcf9ab4e803"
    }
  ],
  "@context": "https://w3id.org/did/v1",
  "service": [
    {
      "type": "KiltMessagingService",
      "serviceEndpoint": "//services.kilt.io:443/messaging"
    }
  ]
}
```

### Upload the container to KILT Protocol DockerHub

The Universal Resolver retrieves the DID Driver from [KILT's dockerhub](https://hub.docker.com/u/kiltprotocol).

To push a new version of the KILT DID Driver onto DockerHub, for use in the Universal Resolver:

```bash
docker build -t kiltprotocol/kilt-did-driver .
docker push kiltprotocol/kilt-did-driver:latest
```

### Other useful commands

#### Kill a container

```bash
# list container IDs:
docker ps
docker kill <containerID>
docker rm <containerID>
```

#### Run a clean build

Simply add `--no-cache` to a `docker build` command.
