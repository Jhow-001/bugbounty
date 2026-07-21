# Technical Description

## Attack Scenario

The vulnerability originated from the exposure of a privileged Enterprise access token within a publicly accessible Server-Side Rendering (SSR) response.

An external attacker inspecting the application's initial HTML response could recover an administrative credential unintentionally exposed through the `window.__NUXT__` runtime state. Because the token was intended for backend authentication with an integrated Enterprise media platform, recovering this credential allowed the attacker to inherit the organization's administrative identity within that external infrastructure.

What initially appeared to be a client-side secret disclosure evolved into a complete infrastructure pivot. The recovered credential enabled authenticated administrative access to a third-party Enterprise service responsible for storing and managing the organization's digital media assets.

Depending on the granted authorization scopes, an attacker could upload, modify, download and permanently delete organizational media, access unpublished content during production, enumerate privileged administrative resources and interact directly with management APIs that were never intended to be publicly accessible.

---

## Reconnaissance & Infrastructure Pivot

The assessment began through analysis of publicly accessible SSR responses and application network traffic.

Inspection of the server-rendered runtime configuration revealed an Enterprise access token unintentionally exposed within the application's public execution context.

Validation confirmed that the recovered credential possessed high-privilege authorization scopes, including:

- `private`
- `edit`
- `upload`
- `video_files`
- `public`

Successful authentication demonstrated that the credential belonged to an Enterprise account used by the organization's integrated media infrastructure.

The exposed account managed approximately **233 TB** of digital assets distributed across more than **150,000** media objects, demonstrating the scale of the potential compromise.

---

## Validated Security Impact

### Sensitive Information Exposure

The recovered credential provided authenticated access to Enterprise management APIs that exposed privileged operational metadata and administrative capabilities.

Validation confirmed access to sensitive user-related information, media consumption metadata and organizational content management data available through integrated backend services.

---

### Administrative Control

The validated authorization scopes granted administrative capabilities over the organization's media environment.

The recovered credential allowed:

- upload of new media assets;
- modification of existing content;
- download of protected media;
- permanent deletion of organizational assets;
- enumeration of private media libraries;
- access to privileged administrative metadata.

---

### Live Media Pipeline Visibility

One of the most significant findings was unrestricted visibility into the organization's media production pipeline.

Using the recovered credential, it was possible to monitor media assets while they were actively being uploaded and processed.

The platform exposed real-time operational information, including upload progress, processing state and publication lifecycle, allowing observation of unpublished content before release.

This behavior effectively bypassed application-layer trust boundaries and exposed production workflows that should only be accessible to authorized administrative personnel.

---

### Access to Raw Media Versions

Administrative API access also exposed historical media revisions and original uploaded assets before post-processing or publication.

The ability to retrieve raw media versions significantly increased the potential impact by exposing unpublished material and original content before organizational review or privacy controls were applied.

---

### Organizational Intelligence

The exposed administrative APIs allowed enumeration of permission models, privileged roles and governance-related metadata.

This information could be leveraged to identify high-value administrative accounts, understand organizational access policies and facilitate follow-up attacks such as privilege escalation, targeted phishing and attacks against administrative governance.

---

## Security Assessment

The primary security issue was not merely the disclosure of an access token.

The exposed credential represented a trusted Enterprise identity used for communication between backend services and an external infrastructure provider.

Its unintended exposure completely broke the application's trust boundary, allowing an unauthenticated external user to inherit administrative privileges within an integrated Enterprise platform.

This transformed a frontend secret exposure into a high-impact infrastructure compromise affecting confidentiality, integrity and availability across systems that extended far beyond the original web application.
