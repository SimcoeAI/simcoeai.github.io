# Simcoe AI sequence diagram

The following diagram depicts the sequence of application starting from the client app e.g. UI client app, ATS, etc. to the last tier. The Gateway Web Api, as it appears, facilitates collecting requests and passes them to the backend's AI engine. 

## Gatewat's security feature

One of the key elements of Gateway Api is that it provides Azure AD B2C a couple of claims for further security enhancements.

![simcoe ai sequence diagram](assets/sequence.png)