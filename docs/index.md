---
title: Home
---
# Roblo3 - AWS SDK for Roblox Lua
Roblo3 is an unofficial Software (Game) Development Kit allowing for integration of Amazon Web Services with Roblox games. It aims to be easy to use and develop with, easing the integration of AWS into Roblox games.

Roblo3 is named in homage to the [AWS Boto3 SDK](https://github.com/boto/boto3) for Python; additionally, Roblo3 is an object oriented SDK with a similar structure to that of Boto3.

Roblo3 handles signing requests to AWS, and conforms to the Signature Version 4 required by all modern AWS Services (Signature Version 4 is supported by all AWS Services with the exception of Amazon SimpleDB, which has been largely deprecated and superseded by Amazon DynamoDB; due to this, Roblo3 does **not** support Amazon SimpleDB). Roblo3 also handles transmitting requests to AWS, then receiving, parsing, and translating requests into standard lua format (tables, strings, booleans, etc.).

# Roblo3 GitHub Repository
The repository for the Roblo3 SDK can be found at https://github.com/Roblo3/Roblo3.