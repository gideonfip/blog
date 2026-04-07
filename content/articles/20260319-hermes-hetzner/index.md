---
title: I deployed a Hermes agent on Hetzner, here's how you can too
date: 2026-03-19
status: writing
brand: FIP
writing_started: 2026-03-19
folder_index: "[[Tasks_articles_FIP_writing_Hermes Hetzner_index]]"





---
# Hermes is easier than OpenClaw (how I deployed mine on Hetzner)
# I deployed a Hermes agent on Hetzner, here's how you can too

After spending almost a week just to get OpenClaw up and running, I almost gave up on building my autonomous agent.

OpenClaw is great if you're technical, but the entire experience is not friendly for non-coders like me.

So when everyone mentioned how easy it is to create a Hermes agent, I decided to try it out.

*Instead of migrating my entire OpenClaw setup, I decided to build a new agent from scratch.*

This meant having to source another VPS to host my agent (since my WiFi is unstable), and I decided to go with Hetzner.

So here's the full process of how I deployed my Hermes agent to a Hetzner VPS:

---
## Hetzner's prices will increase in April

Before we start, just something to take note that Hetzner will increase their plans, starting from April.

Most of the cost-optimised plans (the one that I chose) are affected, and it costs ~ $1.50 extra per month.


![[Pasted image 20260315213023.png]]

*Hetzner still provides one of the cheapest plans for hosting a VPS, so it's a good low-commitment way to try out an OpenClaw deployment.*

---
## Registering for an account

I followed [Digital Benjamins' video](https://www.youtube.com/watch?v=IpJqAW8F4Lk), which provided a great overview on installing OpenClaw through a VPS.

*It included some good security tips that I didn't use for my initial OpenClaw setup on Tencent Cloud.*

But to create an account, I had to verify via KYC, so that's something that may stop you from signing up.

![[Pasted image 20260315212958.png]]

---
## Purchasing a plan

I chose the cost-optimised plan at 4GB RAM that costs me $4.46/month (which will increase in April).

![[Pasted image 20260315213427.png]]

I followed Benjamins' guide to generate an SSH Key, which allows me to log into my VPS directly from my Mac's terminal.


![[Pasted image 20260315213630.png]]

I got ChatGPT to help me with this portion to generate an SSH key on my desktop.

I used this command via the Terminal to create my key,

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

before using [this guide](https://fipcrypto.notion.site/Securing-your-VPS-324c7c36702f80d9adedfb2a2201072c?source=copy_link) that I created to connect it to Hetzner.

![[Pasted image 20260315214044.png]]

This created my server, and I had to copy the IP address (this is something you need to secure and not share it with anyone).


![[Pasted image 20260315214453.png]]

I went to my terminal and entered this command:

```
ssh root@<YOUR_IP>
```

After entering the passphrase (which I set inside Hetzner), I could access my VPS via Terminal in the root account.

![[Pasted image 20260317194159.png]]



This is not a good security practice since root access is very powerful (if it goes into the wrong hands), so I followed Benjamins' advice and created a new user to install my Hermes Agent there instead.

*You could follow the guide [here](https://fipcrypto.notion.site/Securing-your-VPS-324c7c36702f80d9adedfb2a2201072c?source=copy_link) on how to do something similar too.*

---
## Setting up Hermes on Hetzner

With this new user, I started the setup wizard for @NousResearch's Hermes.

![[Pasted image 20260315222447.png]]

*If you used OpenClaw before, the experience is somewhat similar.*

I'm using my @Zai_org Coding Lite plan, where I generated an API key,
![[Pasted image 20260315222502.png]]

and added it to Hermes.

![[Pasted image 20260315222605.png]]


I tried to install Docker, but I wasn't sure what I should be doing here, and I decided to skip this step.

![[Pasted image 20260315222845.png]]

*I plan to ask my bot how to add the docker in future.*

Since I'm already familiar with the Discord workflow after doing it multiple times with OpenClaw, I created a Discord bot instead of Telegram or Slack.

![[Pasted image 20260315223014.png]]

*There are a few configurations, and the full guide can be found [here](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/discord).*

I had to reload my bash first, as the `hermes` command didn't work before that,

![[Pasted image 20260315225712.png]]

and I finally launched my Hermes agent with glm-4.7 as the LLM.


![[Pasted image 20260315224159.png]]

---
## Troubleshooting my Discord bot

I tried posting a test message on Discord, which failed, but I liked how the `hermes` command brought up a chatbox that I can instantly talk to the LLM.

*Instead of OpenClaw's `openclaw tui` command.*

![[Pasted image 20260315224513.png]]

I asked GLM to troubleshoot my configuration, and the test showed that it worked.

![[Pasted image 20260315224521.png]]

What I realised was that the Discord bot would only respond if I mentioned it on Discord. Leaving the message blank was not enough, and I had to write something like 'hi' before it finally worked.

![[Pasted image 20260318150512.png]]

Going back to Hermes' [docs](https://hermes-agent.nousresearch.com/docs/user-guide/messaging/discord/), I realised it's because the `REQUIRE_MENTION` variable is on by default across all channels, so I got my Discord bot to remove it entirely.

---

## Troubleshooting my GLM connection

While I was installing @okx's Agentic Wallet, I suddenly faced an `insufficient balance` error, even though I did not hit the rate limits for my Coding Lite plan yet.

![[Pasted image 20260319143058.png]]

This led to a frustrating 3 hours of troubleshooting everything, 

https://x.com/fipcrypto/status/2034520522202247490

and here's what finally worked for me:

Instead of using the Z.AI option in the models page, I switched over to the custom endpoint.

![[Pasted image 20260319143357.png]]

I used the coding endpoint instead of the normal one, `https://api.z.ai/api/coding/paas/v4`, and it finally worked.

![[Pasted image 20260319143641.png]]

*So here's a troubleshooting tip if you're facing the same problem as I did.*

---
## Why are you even using Hermes?

So what's even the point of having a Hermes agent?

For me, I want to take advantage of its persistent memory that seems to build on OpenClaw's MEMORY.md system, and create my own trading agent.

https://x.com/NousResearch/status/2026758996107898954

I was inspired by @RHLSTHRM's post on building a 24/7 crosschain agent that:

- Gets market data from the CoinGecko API
- Swaps crosschain with @lifiprotocol
- Executes gasless transactions with Pimlico + EIP-7702

https://x.com/RHLSTHRM/status/2031383989488070972

I'm still in the process of building this out, as I want to generate a private key securely first using @PatrickAlphaC's recommendations instead of storing it in plaintext in my .env file.

https://x.com/PatrickAlphaC/status/2027870295722238080

There's so much to learn about building secure agents, and I'll share everything I learn in the next few articles.

*Feel free to use my [referral link](https://z.ai/subscribe?ic=UZU3EY63I5) for GLM-4.7 to get a $10/month plan with 10% off.* 

---
## You don't need autonomous agents to use AI effectively

Maybe you read through this article and are completely overwhelmed about where to start.

There are so many steps to set it up properly, and many people will give up halfway.

*I took a week just to get everything working, without building any useful Skills yet.*

For most people, you don't need a complex system right from the start.

Instead, all you need is to find one soul-sucking, repetitive task that you hate doing and outsource it to AI.

So start letting AI work for you by building your very first useful Claude Skill with [this free video course](https://fipcrypto.gumroad.com/l/build-a-useful-claude-skill).
