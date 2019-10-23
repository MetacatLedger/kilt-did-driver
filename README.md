# Dev setup

## Basic

* (Once only: Install the required dependencies with `yarn start`)
* Start the web server:
  
  ```bash
  node src/index.js
  ```

* Make a request:
  
  ```bash
  curl -X GET http://localhost:8080/1.0/identifiers/did:kilt:5CtPYoDuQQFLe1JU5F8KHLXkKaWxLkKH1dBAfHrUU8SoxASr
  // should output the did document as JSON:
    {"id":"did:kilt:5GZPvZadd2GWEZcUPEw2eentLsTZFoXjYPoozxsYJqaf6c5u","authentication":{"type":"Ed25519SignatureAuthentication2018","publicKey":["did:kilt:5GZPvZadd2GWEZcUPEw2eentLsTZFoXjYPoozxsYJqaf6c5u#key-1"]},"publicKey":[{"id":"did:kilt:5GZPvZadd2GWEZcUPEw2eentLsTZFoXjYPoozxsYJqaf6c5u#key-1","type":"Ed25519VerificationKey2018","controller":"did:kilt:5GZPvZadd2GWEZcUPEw2eentLsTZFoXjYPoozxsYJqaf6c5u","publicKeyHex":"0xc6d2aee1adceaed6fb742238c57851ee9ed77f6715a6765339cc91277d31eb04"},{"id":"did:kilt:5GZPvZadd2GWEZcUPEw2eentLsTZFoXjYPoozxsYJqaf6c5u#key-2","type":"X25519Salsa20Poly1305Key2018","controller":"did:kilt:5GZPvZadd2GWEZcUPEw2eentLsTZFoXjYPoozxsYJqaf6c5u","publicKeyHex":"0x1c1f6b8fa12f6bbd0e7e4283266b0ae8b3b321c14909f5cd47f293dda1cb8436"}],"@context":"https://w3id.org/did/v1","service":[{"type":"KiltMessagingService","serviceEndpoint":"//services.kilt.io:443/messaging"}]}
  ```

## Run with docker

In `kilt-did-driver`:

```bash
docker build -t kiltprotocol/kilt-did-driver .  
docker run -p 49160:8080 -d kiltprotocol/kilt-did-driver:latest // returns a container ID e.g. 4cf5867afbce40876a5ca2467bdb14407199a2eda29a89df1f98514c77cce6bc
docker ps // see a list of container IDs, copy the relevant one e.g. 16df22cc7c5f
docker logs 16df22cc7c5f // 16df22cc7c5f is the container id copied at the previous step
```

The container should be running and logs should be visible.
To query, run:

`curl -X GET http://localhost:49160/1.0/identifiers/did:kilt:5CtPYoDuQQFLe1JU5F8KHLXkKaWxLkKH1dBAfHrUU8SoxASr`

The response should be the following DID Document as JSON:

```json
{
  "id": "did:kilt:5CtPYoDuQQFLe1JU5F8KHLXkKaWxLkKH1dBAfHrUU8SoxASr",
  "authentication": {
    "type": "Ed25519SignatureAuthentication2018",
    "publicKey": [
      "did:kilt:5CtPYoDuQQFLe1JU5F8KHLXkKaWxLkKH1dBAfHrUU8SoxASr#key-1"
    ]
  },
  "publicKey": [
    {
      "id": "did:kilt:5CtPYoDuQQFLe1JU5F8KHLXkKaWxLkKH1dBAfHrUU8SoxASr#key-1",
      "type": "Ed25519VerificationKey2018",
      "controller": "did:kilt:5CtPYoDuQQFLe1JU5F8KHLXkKaWxLkKH1dBAfHrUU8SoxASr",
      "publicKeyHex": "0x245e26168c3393e9c7fce042637b980758e783840974f9fadce4c8fe6fc76cb9"
    },
    {
      "id": "did:kilt:5CtPYoDuQQFLe1JU5F8KHLXkKaWxLkKH1dBAfHrUU8SoxASr#key-2",
      "type": "X25519Salsa20Poly1305Key2018",
      "controller": "did:kilt:5CtPYoDuQQFLe1JU5F8KHLXkKaWxLkKH1dBAfHrUU8SoxASr",
      "publicKeyHex": "0xfb151a959ed0e01fd9748105c617497f329339ed22207b9185cc40c48b44e004"
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

## Upload container to KILT Protocol DockerHub

The Universal Resolver retrieves the DID Driver from [KILT's dockerhub](https://hub.docker.com/u/kiltprotocol).
To update the DID Driver:

```bash
docker push kiltprotocol/kilt-did-driver:latest
```
