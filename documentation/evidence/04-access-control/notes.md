# Vulnerability 4: Missing Function-Level Access Control

## Description

The Benefits functionality was intended for administrator use. The navigation
interface hid the Benefits link from ordinary users, but the server-side GET
and POST routes only applied the isLoggedIn middleware.

## Affected components

- Route configuration: app/routes/index.js
- Session and authorization middleware: app/routes/session.js
- Protected endpoint: /benefits

## Before-fix demonstration

The administrator account could access the Benefits function as expected. A
normal user did not see the Benefits link in the application menu. However,
the same normal user could manually enter the /benefits URL and access the
restricted functionality.

## Root cause

The application implemented access control only at the presentation layer.
The server confirmed that the requester was authenticated but did not verify
that the authenticated user had administrator privileges.

## Security impact

An authenticated non-administrator could access functionality intended for
administrators. Because the POST route was also not protected by a role
check, an unauthorized user could potentially submit changes to employee
benefit information.

## Correction

An isAdminUserMiddleware function was implemented. The middleware obtains the
authenticated user ID from the server-side session, loads the corresponding
user from the database and verifies the isAdmin property. Both the GET and
POST /benefits routes now apply isLoggedIn followed by isAdmin.

## Why the correction works

The authorization decision now occurs on the server for every request. Hiding
a link is no longer the only control. A normal user cannot bypass the role
check by manually entering the URL or constructing a direct request.

## After-fix validation

The exact same normal-user request to /benefits was repeated after rebuilding
the application. The server denied access. The administrator account was
tested separately and retained legitimate access to the Benefits module.

## SAST evidence

The generic Semgrep configuration did not understand that the Benefits
function required an administrator role. A project-specific Semgrep rule was
therefore created to detect Benefits routes that used isLoggedIn without the
required isAdmin middleware.

Before remediation, the custom rule identified the unprotected GET and POST
routes. After applying isAdmin to both routes, the same rule reported zero
findings.

## Evidence

- V4-01-admin-benefits-access.png
- V4-02-normal-user-menu.png
- V4-03-normal-user-direct-access.png
- V4-04-vulnerable-routes.png
- V4-05-semgrep-before.png
- V4-06-secure-middleware.png
- V4-07-normal-user-blocked.png
- V4-08-admin-still-authorized.png
- V4-09-semgrep-after.png
