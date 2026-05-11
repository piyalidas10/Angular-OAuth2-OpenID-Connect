# angular-oauth2-oidc
The angular-oauth2-oidc library is widely used in real-world enterprise Angular applications for secure authentication and authorization using OAuth2 and OpenID Connect (OIDC).

Common Real-Time Uses of angular-oauth2-oidc
**1. Enterprise SSO (Single Sign-On)**  
Used in corporate portals where users log in once and access multiple applications securely.

Examples:
- Employee dashboards
- HR systems
- Internal admin portals
- Banking applications

Works with:
- Microsoft Entra ID
- Okta
- Auth0
- Keycloak

**2. MFA (Multi-Factor Authentication)**  
Applications requiring OTP, authenticator apps, or biometric login.

Real-world industries:
- Banking
- Insurance
- Healthcare
- Government portals

Flow:
- Angular app redirects to Identity Provider
- User enters password
- MFA challenge triggered
- Tokens returned securely

**3. Secure JWT Token Management**

Used to:
- Store access tokens securely
- Refresh expired tokens automatically
- Prevent session hijacking
- Handle silent renew

Example:
```
this.oauthService.setupAutomaticSilentRefresh();
```

**4. Role-Based Access Control (RBAC)**  
Used in enterprise admin systems where different users see different modules.

Example roles:
- Admin
- Manager
- Employee
- Auditor

Angular route guards check roles from JWT claims.

Example:
```
const claims = this.oauthService.getIdentityClaims();
```

**5. API Security with Spring Boot / .NET / Node.js**  
Frontend Angular apps authenticate users and securely call backend APIs.

Typical architecture:
```
Angular App
   ↓
Identity Provider (OIDC)
   ↓
Access Token
   ↓
Spring Boot / .NET API
```

Common backend stacks:
- Spring Boot
- ASP.NET Core
- NestJS

**6. Microfrontend Authentication**  
Large organizations using Angular microfrontends share centralized login sessions.

Example:
- Shell app authenticates
- Multiple remote apps reuse token/session

Very common with:
- Module Federation
- Nx Monorepo
- Enterprise Angular architectures

**7. B2B SaaS Applications**  
Software products with customer organizations logging into the same platform.

Examples:
- CRM systems
- ERP platforms
- Analytics dashboards

Features:
- Tenant-based login
- Organization-level RBAC
- Secure token exchange

**8. Government & Compliance Systems**

Needed for:
- Audit logging
- Secure identity verification
- Compliance standards

Common compliance:
- GDPR
- HIPAA
- SOC2
- ISO 27001

**Why Companies Use angular-oauth2-oidc**

Advantages:
- Standards-based security
- Production-ready OIDC flows
- Easy Angular integration
- Automatic token refresh
- Route guard support
- PKCE support for SPA security

**Most Used OAuth Flow Today**  
For Angular SPAs:
```
Authorization Code Flow + PKCE
```
This is the recommended secure modern approach.

**Typical Production Flow**  
```
User → Angular App
      → Identity Provider Login
      → MFA
      → Access Token + ID Token
      → Angular Stores Session
      → API Calls with Bearer Token
      → Auto Refresh
```

**Real Companies Using Similar Architecture**
- Microsoft
- Google Cloud
- Salesforce
- SAP
- Adobe

**Typical Angular Features Combined With It**  
Usually integrated with:
- Angular Route Guards
- HTTP Interceptors
- NgRx Authentication State
- Session Timeout Handling
- Idle Detection
- Secure Storage
- Refresh Token Rotation

**angular-oauth2-oidc is ideal when building:**
- Enterprise Angular apps
- Secure SaaS products
- Banking dashboards
- Healthcare portals
- Government applications
- B2B admin systems
- Zero-trust frontend architectures

# @ng-idle
@ng-idle in Angular is mainly used for idle detection and session timeout management in enterprise/web applications.

**Common real-time uses of @ng-idle:**

1. Auto Logout After Inactivity
  - Detects when a user is inactive for a specific time.
  - Automatically logs out users for security.
2. Session Timeout Warning Popup
  - Shows:
    - “Your session will expire in 1 minute”
  - Gives option to continue session.
3. Banking / Insurance / Healthcare Apps
  - Prevents unauthorized access if user leaves system open.
  - Common compliance/security requirement.
4. JWT / OAuth Session Handling
  - Works with token expiration.
  - Can trigger token refresh before logout.
5. Real-Time User Activity Tracking
  - Detects mouse movement
  - Keyboard typing
  - Scroll activity
  - Touch events
6. Multi-Tab Session Sync
  - Logout all tabs together.
  - Keep session state synchronized.
7. RBAC + Enterprise Security
  - Protect admin dashboards.
  - Prevent stale privileged sessions.
8. Kiosk/Public Computer Protection
  - Auto resets application after inactivity.

Typical Angular flow:
```
User inactive → Idle timer starts
↓
Warning countdown popup
↓
No activity detected
↓
Logout API called
↓
Clear tokens/session
↓
Redirect to login
```

**Common packages used together:**
- @ng-idle/core
- @ng-idle/keepalive
- rxjs
- Angular Router
- OAuth/OIDC libraries

**Official package:**
@ng-idle/core GitHub

**Typical enterprise stack:**
- Angular 19
- OAuth2/OIDC
- MFA
- JWT refresh
- Idle timeout
- Session monitoring
- Route guards
- RBAC permissions

# jwt-decode
jwt-decode is commonly used in frontend applications to read and inspect JWT (JSON Web Token) contents without validating the signature. It is lightweight and mostly used in Angular, React, Vue, and plain JavaScript apps.

The library often used is: jwt-decode GitHub Repository

**1. Read Logged-in User Info Instantly**  
After login, backend returns a JWT:
```
{
  "access_token": "eyJhbGciOi..."
}
```
Frontend decodes it:
```
import { jwtDecode } from 'jwt-decode';

const token = localStorage.getItem('token');

const decoded = jwtDecode(token!);

console.log(decoded);
```
Example decoded payload:
```
{
  "sub": "123",
  "email": "user@gmail.com",
  "role": "admin",
  "exp": 1719999999
}
```
Used for:
- Showing username/email
- Displaying profile UI
- Detecting user role
- Personalizing dashboard

**2. Role-Based Access Control (RBAC)**  
Very common in enterprise Angular apps.

Example:
```
const decoded: any = jwtDecode(token);

if (decoded.role === 'admin') {
   // show admin panel
}
```
Used for:
- Admin menus
- Route guards
- Feature access
- Hiding unauthorized buttons/pages

Angular route guard example:
```
canActivate(): boolean {
   const token = localStorage.getItem('token');

   if (!token) return false;

   const decoded: any = jwtDecode(token);

   return decoded.role === 'admin';
}
```

**3. Check Token Expiry in Real Time**  
JWT contains expiration timestamp (exp).

Example:
```
const decoded: any = jwtDecode(token);

const expired = decoded.exp * 1000 < Date.now();

if (expired) {
   logout();
}
```
Used for:
- Auto logout
- Session timeout
- Silent refresh
- MFA reauthentication

**4. OAuth2 / OpenID Connect (OIDC) Apps**  
Very heavily used with:
- Microsoft Entra ID
- Auth0
- Okta
- Keycloak
- Google Identity

Decode:
- ID Token
- Access Token
- Custom claims

Example claims:
```
{
  "preferred_username": "piyali",
  "groups": ["admin", "finance"],
  "scp": "read write"
}
```

**5. Angular Interceptor Security Logic**  
Inside HTTP interceptors:
```
intercept(req: HttpRequest<any>, next: HttpHandler) {

   const token = localStorage.getItem('token');

   if (token) {
      const decoded: any = jwtDecode(token);

      if (decoded.exp * 1000 < Date.now()) {
          this.authService.logout();
      }
   }

   return next.handle(req);
}
```
Used in:
- Enterprise Angular apps
- Banking portals
- Healthcare systems
- Internal dashboards

**6. Multi-Tenant Applications**  
JWT may contain tenant/company info.

Example:
```
{
  "tenant_id": "tcs",
  "region": "india"
}
```
Frontend dynamically changes:
- Theme
- Branding
- API URL
- Permissions

**7. Feature Flags / Subscription Plans**  
JWT payload may include:
```
{
  "plan": "premium",
  "features": ["ai-chat", "analytics"]
}
```
Frontend enables premium features instantly.

**8. MFA / Authentication State**  
JWT may indicate:
```
{
  "mfa_verified": true
}
```
Frontend can:
- Skip MFA page
- Ask for OTP again
- Restrict sensitive actions

**jwt-decode DOES NOT verify JWT signature.**  
It only decodes Base64 payload.

Meaning:  
❌ Not secure for trust decisions alone  
❌ Never trust frontend-only validation  
✅ Backend must validate JWT signature  

Frontend use = UI convenience only.

**Angular 19 Example**  
Install: npm install jwt-decode  
Usage:
```
import { jwtDecode } from 'jwt-decode';

interface JwtPayload {
  email: string;
  role: string;
  exp: number;
}

const token = localStorage.getItem('access_token');

if (token) {
   const decoded = jwtDecode<JwtPayload>(token);

   console.log(decoded.email);
}
```
**Common Enterprise Stack**
```
Angular App
   ↓
OIDC Login
   ↓
Receive JWT
   ↓
jwt-decode
   ↓
Read Claims
   ↓
RBAC / Session / MFA Logic
```

**Real Companies Using Similar JWT Patterns**
- Microsoft
- Google
- Amazon
- Netflix
- Banking applications
- SaaS platforms
- Enterprise dashboards
- Healthcare systems
- Government portals


