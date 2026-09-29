# Vulnerability 1: Server-Side JavaScript Injection

## Description

The contribution update function processed user-controlled contribution
values using JavaScript eval(). This caused the server to interpret supplied
input as executable JavaScript rather than numeric data.

## Affected component

- File: app/routes/contributions.js
- Function: handleContributionsUpdate()
- Inputs: preTax, afterTax and roth

## Before-fix demonstration

A normal numeric contribution was accepted. A harmless arithmetic expression
was then submitted through the same field. The application evaluated the
expression, demonstrating that the server treated user input as JavaScript.

## Security impact

An attacker could potentially execute unauthorised JavaScript in the server
process. The possible consequences include application unavailability,
unauthorised server-side operations and exposure or modification of
application data.

## Root cause

The application passed req.body values directly to eval() without strict
type validation.

## Correction

The eval() calls were removed. Values are converted using Number() and
validated with Number.isFinite(). Negative and invalid values are rejected
with an HTTP 400 response.

## Why the correction works

Number() converts the input as data and does not execute it as JavaScript.
Number.isFinite() confirms that the complete converted value is a valid
finite number. Therefore, expressions and non-numeric input are rejected.

## After-fix validation

The exact harmless expression used during the original test was submitted
again. The corrected application rejected it as invalid input. A normal
numeric contribution remained functional.

## Evidence

- V1-01-normal-numeric-input.png
- V1-02-arithmetic-expression-input.png
- V1-03-expression-evaluated.png
- V1-04-vulnerable-code.png
- V1-05-secure-code.png
- V1-06-same-input-rejected.png
- V1-07-valid-input-still-accepted.png
