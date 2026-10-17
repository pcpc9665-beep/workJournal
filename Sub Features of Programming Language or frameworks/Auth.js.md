**Auth.js** (formerly known as NextAuth.js) is ==an open-source, flexible authentication library designed for modern JavaScript and TypeScript frameworks== (including Next.js, SvelteKit, Express, Qwik, and SolidStart). It handles user sign-ins via OAuth (Google, GitHub, Apple), magic links, credentials (email/password), and WebAuthn (passkeys), supporting both database-backed sessions and encrypted JWT cookies. [[1](https://authjs.dev/concepts/session-strategies), [2](https://daily.dev/posts/auth-js-adoption-guide-overview-examples-and-alternatives-o9zxm5rtk), [3](https://authjs.dev/getting-started), [4](https://authjs.dev/), [5](https://www.youtube.com/watch?v=R0tJdNoq74M&t=38), [6](https://medium.com/@tfan2437/auth-js-introduction-and-getting-started-98d8ae1ed538)]


In late 2025, Auth.js was acquired/absorbed by **Better Auth**. It is now officially in **maintenance mode** (receiving security patches and urgent fixes only, with no new features planned), and new projects are often encouraged to look at Better Auth or Clerk. [[1](https://www.youtube.com/shorts/QerWuETU9ss), [2](https://github.com/nextauthjs/next-auth/discussions/13252), [3](https://www.buildmvpfast.com/blog/better-auth-vs-clerk-vs-authjs-nextjs-decision-tree-2026)]

---

Trade-offs of Auth.js

**Pros (Advantages)**

- **Self-Hosted & Open Source:** You own your user data and infrastructure without paying per-user SaaS fees (unlike hosted alternatives like Clerk or Auth0).
- **Framework Flexibility:** Originally tied strictly to Next.js, Auth.js expanded to support multiple modern web frameworks.
- **Rich Provider Ecosystem:** Built-in integration with dozens of OAuth providers, magic links, and database adapters (Prisma, Drizzle, MongoDB, PostgreSQL, etc.).
- **Flexible Session Strategies:** Supports stateless JWT stored in `HttpOnly` cookies (faster, database-free) or stateful database sessions (allowing instant server-side revocation). [[1](https://better-auth.com/blog/authjs-joins-better-auth), [2](https://avishka.dev/blog/better-auth-vs-auth-js), [3](https://authjs.dev/), [4](https://daily.dev/posts/auth-js-adoption-guide-overview-examples-and-alternatives-o9zxm5rtk), [5](https://authjs.dev/concepts/session-strategies)]

**Cons (Disadvantages & Limitations)**

- **Now in Maintenance Mode:** Because it merged with Better Auth, active feature development has stopped. Only security and critical bug fixes remain. [[1](https://github.com/nextauthjs/next-auth/discussions/13252), [2](https://www.buildmvpfast.com/blog/better-auth-vs-clerk-vs-authjs-nextjs-decision-tree-2026)]
- **Steep Learning Curve for Customization:** Complex custom flows, multi-tenant organizations, fine-grained role-based access control (RBAC), or deep overrides often require writing boilerplate code or custom callbacks. [[1](https://blog.logrocket.com/auth-js-adoption-guide/), [2](https://www.reddit.com/r/buildinpublic/comments/1o1chpe/authjs_is_now_part_of_better_auth_what_this/)]
- **Migration and Version Pain:** Transitioning across major versions (such as the rocky evolution from NextAuth v4 to Auth.js v5) historically introduced breaking changes and documentation gaps. [[1](https://blog.logrocket.com/auth-js-adoption-guide/)]
- **Opinionated Defaults on Credentials:** It intentionally discourages traditional password-based credentials in favor of OAuth or passwordless magic links/passkeys, making standard username/password setups clunky to implement. [[1](https://www.youtube.com/watch?v=R0tJdNoq74M&t=38)]
- **JWT Size Constraints:** Using JWT session strategies limits payload size to roughly 4,096 bytes before requiring chunking or moving data to a database