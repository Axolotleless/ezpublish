# ezpublish
This is a public test version of EZ Publish Service a tool which allows users to upload/create new Roblox experiences without the use of a computer or any emulators
(More Info below)



# EZ Publish Service - How It Works

# THE CLOUDFLARE WORKER
Cloudflare Workers are small scripts that run on 'Cloudflare's global edge
network' instead of a traditional server. The worker exists because browsers
block direct requests to the Roblox API from a webpage due to 'CORS', a browser
security rule that prevents a site from calling a different domain unless that
domain allows it. Roblox does not allow this, so the worker sits in the middle
and makes the call on your behalf.

# SECURITY
Only requests from 'axolotleless.github.io' are accepted. Anything else is
rejected before it touches the Roblox API. Each IP is also rate limited to
20 requests per 60 seconds to prevent abuse.

# HOW A UNIVERSE OR PLACE GETS CREATED
The worker first makes a dummy request to Roblox to get a 'CSRF token', which
is a one time key Roblox requires to prove the request is intentional. It then
makes the real request with that token, your cookie for authentication, and a
template ID that tells Roblox what to base the new universe or place on.

# YOUR ROBLOX COOKIE
Your '.ROBLOSECURITY' cookie is your Roblox session token. Anyone who has it can
act as you on Roblox, so it should be treated like a password. In this tool it
is sent over HTTPS to the worker, used once to authenticate the Roblox API
call, then immediately set to null. It is never logged, never stored, and never
sent anywhere other than directly to Roblox. The frontend also wipes it from
the input field the moment you click Start and again when you close or leave
the tab.

# YOUR IP ADDRESS
Your IP is used only for rate limiting. It is stored temporarily in Cloudflare
KV with a request count and expires automatically after 60 seconds. It is
never logged permanently, never sent to Roblox, and never included in any
response. Roblox sees a Cloudflare data center IP, not yours. Cloudflare as a
platform will have its own access logs as they do for all traffic that passes
through their network, but that is standard for any Cloudflare-hosted service
and is outside the control of this worker.

# YOUR OTHER DATA
The only other things sent to the worker are your Universe ID and template
Place ID if applicable, both of which are just numbers. They are used to
construct the Roblox API request and nothing else. Nothing you enter is stored
anywhere.
