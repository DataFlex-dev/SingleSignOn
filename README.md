# Single Sign-On Library

## Table of Contents
- [Library Information](#library-information)
- [External Components](#external-components)
- [General Information](#general-information)

---

## Library Information

This repository contains a DataFlex Single Sign-On library together with a demo workspace that shows how to integrate it into a web application. The `Library` directory contains the reusable SSO package code, the `Demo` directory contains an example application, and the `Help` directory contains supporting documentation and release notes.

The library is focused on OpenID Connect and OAuth-based login flows and includes support for provider configuration, endpoint auto-discovery, token handling, logout flows, and JWT-based token validation.

###### External Components

The library depends on the following external components:

| Component | Version |
| --------- | ------- |
| `libcrypto-3.dll` / `libcrypto-3-x64.dll` | OpenSSL 3 runtime |
| JWT Library (`JWT\JWT.pkg`) | Required dependency |

`libcrypto-3` is a lightweight OpenSSL runtime dependency used only to convert an RSA modulus and exponent into a PEM-formatted public key through `RsaPublicPemFromModulusExponent` in `cSsoProvider.pkg`. This allows RSA keys loaded from a JWKS endpoint to be auto-loaded into the internal JWT object.

The library also depends on the JWT package through `JWT\JWT.pkg`. That internal JWT object uses the `l8w8jwt` DLLs for the actual JWT decoding and signature verification.

## General Information

| Product | Version |
| ------- | ------- |
| DataFlex | 26.0 |

The library is supported on DataFlex 26.0. It should also be usable in DataFlex 23.0 and up when using the `Maintain Libraries` option in older Studio versions.
