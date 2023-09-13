# Shamir Secret Sharing

Shamir Secret Sharing is a way to split a secret into multiple pieces, such that a certain number of pieces are required to reconstruct the secret. This is useful for storing secrets in multiple places, such that if one of the places is compromised, the secret is still safe.

## How it works

Example: 2-of-3 (M-of-N) splitting. Split the secret into 3 pieces, any 2 of which can reconstitute the secret.

start with secret x

pick a polynomial of degree M-1 (2-1 = 1) with a y-intercept of x. 

a polynomial of degree 1 is a line. A line is defined by 2 points. So we need 2 points to define the line and determine the y-intercept. 

Every party (3) gets a point on the line, which is a piece of the secret. A single point of the line cannot determine the slope or y-intercept. But any 2 points can. 

This works for any M-of-N because a polynomial of order X can be defined by X+1 points. So if you want 3-of-5, you need a polynomial of degree 2 (a parabola) and you give a point on the parabola to each party.