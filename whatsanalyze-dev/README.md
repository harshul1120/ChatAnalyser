# ChatAnalyser

* Analyze your WhatsApp chats in seconds
* Discover insights & statistics while all data remains on your device
* No chat data is sent to any server — everything runs locally in your browser

This is an open-source tool designed to analyze WhatsApp chats and generate PDFs from your conversations. You can run the project locally on your device or visit whatsanalyze.com to explore the hosted "main" branch. The website is deployed using GitHub Pages, and the complete source code is publicly available.

No data from your chats is transmitted to any external server.

![Bildschirmfoto 2021-03-09 um 21 31 28](https://user-images.githubusercontent.com/32100482/110533954-d192e880-811e-11eb-9a0f-ba630014f350.png)

Dev deployment: https://whatsanalyze-80665.web.app

## Encountered an issue?

If you find any bugs or issues, please report them in the GitHub issues section.

# Running WhatsAnalyze locally

## Build Setup

We recommend using Node.js version 16, as newer versions like 18 may cause issues with the linter. The project also requires Python, with a version of `3.11` or lower.

```bash
# install dependencies
$ pnpm install

# serve with hot reload at localhost:3000
$ pnpm dev

# build for production and launch server
$ pnpm build
$ pnpm start

# generate static project
$ pnpm generate
```

You can configure Prettier and ESLint in PyCharm to run automatically on file save.
You may also include `.vue` files to enable formatting and linting for Vue components.

For a more detailed explanation of how everything works, refer to the Nuxt.js docs.

## HTTPS Certificate

https://letsencrypt.org/docs/certificates-for-localhost/

```bash
openssl req -x509 -out 0.0.0.0.crt -keyout 0.0.0.0.key -newkey rsa:2048 -nodes -sha256 -subj '/CN=localhost' -extensions EXT -config <( printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
```

Also install the `.crt` file and trust it in your system settings.

Code to generate a certificate installable on Android:

```bash
openssl pkcs12 -export -legacy -in localhost.pem -inkey localhost-key.pem -out 0.0.0.0.p12
```

Rename it to `.txt` and transfer via Bluetooth.
On your phone, accept the file, locate it in downloads, rename it back to `.p12`, and install it.

```bash
brew install mkcert
mkcert localhost
```

To install on your Mac:

```bash
mkcert -install
```

On Android, install the root CA to trust the certificate:

* find root CA using `mkcert -CAROOT`
* transfer the `rootCA.pem` file to your Android device
* go to settings > security > encryption and credentials > install certificate > CA certificate
* confirm and select the file

Forward port 3000 to your Android device using Chrome:

```
chrome://inspect/#devices
```

Then open:

```
https://localhost:3000
```
