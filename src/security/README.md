# Security Best Practices

Angular protects many template bindings against cross-site scripting (XSS), but those protections are not a substitute for secure application design. Authentication, authorization, input validation, and API security remain responsibilities of the application and its server.

For Angular's complete security guidance, see the [official security guide](https://angular.dev/best-practices/security).

## Render untrusted values with template bindings

Treat values from users, URLs, and APIs as untrusted. Angular escapes interpolated text and sanitizes values in supported HTML and URL binding contexts. For example, interpolation displays markup as text, while binding a value to `[innerHTML]` allows safe HTML but removes unsafe markup and attributes.

See the live [safe template binding example](safe-template-bindings.md). It shows the value interpolated as text and bound as HTML; Angular preserves safe formatting and removes the unsafe event handler. Prefer interpolation when you intend to show text, and use HTML bindings only when rendering markup is a requirement.

Angular's sanitization is context-specific. In particular, a **resource URL** (for example, a URL used to load an iframe or executable resource) cannot be made safe by ordinary sanitization because it can load arbitrary code. Validate such URLs against an application-specific allowlist before use. Avoid `DomSanitizer` methods named `bypassSecurityTrust...` unless the value has been reviewed and is trusted for that exact context; calling one disables Angular's normal protection for that value.

## Keep DOM updates in Angular templates

Prefer bindings and directives over direct DOM manipulation. APIs such as `ElementRef.nativeElement`, `innerHTML`, and third-party DOM libraries do not automatically provide Angular's template sanitization. `Renderer2` is not a security sanitizer either. If direct DOM interaction is unavoidable, follow Angular's guidance for the specific operation and context.

Do not build Angular templates from user-controlled strings. Angular treats templates as executable code; use the default Ahead-of-Time (AOT) compiler for production rather than compiling dynamically generated templates.

## Enforce trust at the server

Client-side checks improve feedback, but users can bypass them. Validate all submitted data on the server, and enforce authorization on every protected server operation. A route guard or a hidden button can improve navigation and user experience, but cannot prevent a caller from sending an HTTP request directly.

Use HTTPS and configure security headers on the server. A suitable Content Security Policy (CSP) and Trusted Types enforcement provide defense in depth against DOM-based injection; configure them for the application's actual deployment and dependencies rather than copying a policy without review.

For implementation details and current recommendations, consult Angular's [security guide](https://angular.dev/best-practices/security) and [DOM API guidance](https://angular.dev/guide/components/dom-apis).
