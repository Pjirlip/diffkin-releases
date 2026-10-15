# Network policy

Websites are untrusted. A crawled page can try to make Diffkin's browser load things from your own machine or network: a link to `http://127.0.0.1:8080/admin`, a redirect to a private address, an image from a cloud metadata endpoint. The network policy of a target decides what the browser may contact.

## Public (default)

For a public target the browser may only contact globally routable addresses. Blocked are:

- loopback (`127.0.0.1`, `::1`) and local names (`localhost`, `*.local`, `*.internal`, …);
- private networks (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`, IPv6 unique-local);
- link-local addresses such as `169.254.169.254`;
- carrier-grade NAT, multicast and reserved ranges;
- any hostname that resolves to one of these.

The check applies to the page itself, to every redirect hop, to every image, script, stylesheet, font, request and WebSocket the page opens, and to sitemap downloads. Only `http`, `https`, `ws` and `wss` are allowed; `file:`, `ftp:` and similar schemes are never requested.

What you see:

- A route whose page is blocked (for example because the hostname points at a private address or a redirect leads to localhost) is recorded as **skipped** with a security-policy message naming the address and why it was refused.
- A page whose subresources were blocked is still captured; the capture metadata lists the blocked requests.
- A target whose base URL is a local address or name is rejected when you save it, with a hint to switch the policy.

## Local development

Choose *local development* for targets on your machine or in your LAN (`http://localhost:3000`, `http://192.168.1.20`, `http://app.local`). The address restrictions are lifted for that target; the scheme restriction stays. Use it only for sites you run yourself: with this policy, a page can make the browser load content from your network.

## Credentials

Basic Auth and custom headers configured for a target are sent only to the target's own origin (scheme, host and port). Requests to any other host, including redirects away from the target, never carry them.

## Limits

Diffkin resolves hostnames itself before the browser does. A hostile DNS server could answer differently for the browser (DNS rebinding); Diffkin detects a response from a non-public address, refuses the capture and stores nothing, but the request has been made by then.
