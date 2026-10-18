# RedDotStar AI Lab — Account Setup

The frontend now has real Login / Create Account / Logout flows. The Worker has server-side password hashing, unique-email registration, 30-day sessions and account lookup.

## One Cloudflare step is still required

The Worker needs a Cloudflare D1 database binding named exactly:

`DB`

In the Cloudflare dashboard:

1. Open **Workers & Pages** → `reddotstar-ai`.
2. Open **Settings** → **Bindings**.
3. Add a **D1 database** binding.
4. Variable name: `DB`.
5. Create/select a D1 database, for example `reddotstar-ai-db`.
6. Save the Worker.
7. Deploy the latest Worker code.

The Worker creates the `users` and `sessions` tables automatically on the first account request, so no manual SQL is required for the initial version.

## What is now protected server-side

- Email is normalized to lowercase.
- The same email cannot create a second account.
- Passwords are hashed with PBKDF2 + SHA-256 and a random salt.
- Raw passwords are not stored in the database or localStorage.
- Login creates a random bearer session token.
- Only the token is kept by the browser.
- Sessions expire after 30 days.
- Logout invalidates the server-side session.
- Inactive/banned users cannot authenticate.

## Important

The current AI credit counter is still a frontend/localStorage prototype. It should be moved to the database before payments or public commercial launch so users cannot manipulate credits from the browser.

Payment, email verification, password reset, rate limiting and admin moderation are separate next phases.
