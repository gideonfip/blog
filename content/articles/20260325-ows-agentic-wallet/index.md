
---
date: 2026-03-25
# I finally found a wallet to give to my agent
---

# I tested every agentic wallet option, only one actually worked (with a catch)

You'll never build a safe DeFi agent with a plaintext private key.

Anyone who can access your file will have full control over your agentic wallet.

So what's the best way to give a wallet to your agent?

This stumped me for weeks as I tried and failed to find the right wallet that worked for me.

There were so many options, yet some:

- weren't secure enough
- locked into certain ecosystems
- did not have enough network support

But I think I've finally found the answer (though it's still not the perfect solution yet).

---
## Many agentic wallets just didn't work

These were all of the options that I considered:

- Storing a private key in plaintext

While it's convenient to store a private key in plaintext, it's the easiest way to get hacked.

Anyone who accesses your computer or VPS will have full access to your private key and the funds in your agent's wallet.

*A private key is worse than an API key because while those can be rotated at any time, a private key is lost forever once it gets exposed.*

@PatrickAlphaC has been tweeting against storing private keys in plaintext and it's way too risky to leave your private keys exposed in this way.

- Storing a private key in Foundry

Patrick recommended to uss Foundry to generate and encrypt a private key, and I tried doing that on my VPS.

![[Pasted image 20260325164437.png]]

But then, I realised how this is not feasible after chatting with my agent:

There still needs to be a manual step for me to decrypt the transactions, so the agent still needs access to the private key for it to run autonomously.

![[Pasted image 20260325164709.png]]

It suggested me to store the private key in plaintext, which is not what I want.

I tried asking Grok for other alternatives, and it suggested using Account Abstraction which seemed to complex for me.

![[Pasted image 20260325164802.png]]

And I'll likely burn through tons of tokens just to troubleshoot and get everything right.

*But maybe I'm doing it wrong, and there's a way to use Foundry with agents where they can sign autonomously without my approval.*

- Agentic Wallets

While I was searching for an Agentic Wallet, I found a few options:

1. Coinbase
2. Privy
3. OKX

@wallet just launched their Agentic Wallet a few days ago, and I found it most ideal since it supports multiple chains.

While Coinbase launched theirs a while back, I don't have enough details on what networks they support (though it seems like it's just Base).

![[Pasted image 20260325170846.png]]

and when I wanted to send funds from my Coinbase Agentic Wallet, it was only possible for USDC.

![[Pasted image 20260325170926.png]]

*For other tokens, I had to use the developer portal which defeats the purpose of an agentic wallet.*

While the OKX Agentic Wallet supports more chains and functions, I couldn't get it to work with other platforms like @lifiprotocol.

![[Pasted image 20260325171138.png]]

Doing something as simple as bridging ETH from Base to Ethereum was such a pain because there was no route with the OKX skills, and LiFi couldn't work.

![[Pasted image 20260325230655.png]]

But this wallet seems to be the best fit for my needs right now:

---
## OWS wallets are the most secure for now

@OpenWallet's Open Wallet Standard (OWS) appealed to me because it:

- doesn't leave private keys in plaintext
- acts like a full private key except that it's encrypted
- supports all EVM chains and other L1s (Solana etc.)

https://x.com/OpenWallet/status/2036085019958026270

I tried generating a wallet on my Hetzner-Hermes setup, and it didn't work at first.

![[Pasted image 20260324112135.png]]

So I just asked Hermes how to do it, and it helped to create my wallet from there.
![[Pasted image 20260324112331.png]]


*It's possible to do it manually, you could throw the error into ChatGPT and tell it that the binary isn't in your path.*

The CLI generated all of the wallet addresses for me, while I didn't have access to the private keys.

![[Pasted image 20260324164145.png]]

But what I realised was how easy it is to export out the seed phrase, once someone has access to my VPS.

We just needed the wallet export command, and the entire 12-word phrase gets exposed immediately.

![[Pasted image 20260324164246.png]]

*Don't worry, I've deleted that wallet already.*

I did see the docs mention adding a passphrase, but I wasn't prompted for it during the wallet creation process (and that's something I'd like to clarify with the @moonpay team as well).

I did try encrypting the wallet and generating our passphrase with Claude,

![[Pasted image 20260324131619.png]]

but that failed and I couldn't decrypt the wallet even after typing the right passphrase.

![[Pasted image 20260324165419.png]]

For now, the biggest risk is someone accessing my VPS and typing out the export command to access my seed phrase.

So it's 'slightly' more secure than storing my private key in plaintext, as the hacker needs to know the right command to export it out.

---
## Adding policies to my wallet

Instead of giving my wallet full access via the private key, I could give it an allow-all policy with a known expiry date so that I can easily rotate that key if it gets exposed.

![[Pasted image 20260324172849.png]]

Claude (but actually, GLM-4.7) guided me on the commands to create the policy and how to give access to my Hermes agent.

![[Pasted image 20260324172457.png]]

I tried asking my Hermes Discord bot to send a transaction, and it was able to do it via the API key.

![[Pasted image 20260324174901.png]]

I tried swapping and bridging from LiFi, and both work (after installing the SDK, which you can give your agent all the instructions from [here](https://docs.li.fi/sdk/installing-the-sdk)).

![[Pasted image 20260324183237.png]]


![[Pasted image 20260325224405.png]]

Though [LiFi charges a 0.25% fee](https://docs.li.fi/faqs/fees-monetization#does-li-fi-charge-a-fee-on-every-swap-or-bridge-transaction) for every swap we make on the SDK platform, it's still the best one right now for cross-chain swaps across different chains.

---
## So, what's next?

Now that I finally have the flexibility of signing different transactions with a private key (unlike the OKX Agentic Wallet), this means that my agent can perform almost any other transactions just like I can.

I have many plans on how to trade or implement DeFi strategies with my agent:

- A recursive bot for @boros_fi (since I still don't understand how funding works)
- A bot to find the best (and relatively safe) DeFi yields
- A trading bot on both perps and normal onchain swaps

I've made a horrible error by giving my agent too much autonomy at the start, and it made a \$400+ trade for me when I just wanted it to use \$20 per trade.

https://x.com/gideonfip/status/2036517919501459894

So it's a painful lesson for me that I should build out a proper skill for my entire trading strategy, and tell the agent to execute it exactly as it is (instead of giving it vague instructions and hoping that it'll understand).

There are many more agentic MCPs, skills, and CLIs that I can play with using this wallet, and I plan to add some of them via @yq_acc's CryptoSkills site.

https://x.com/yq_acc/status/2035301943170785488

While I'm glad that the wallet works, I'm still concerned how there was no passphrase generation step when creating my wallet, so it's still exposed somewhat.

Hopefully, I can gain clarity on how to set this passphrase soon (I've struggled to find that step in the docs, even with AI).

*I'm currently running my OWS wallet with my Hermes agent (deployed on Hetzner), and I shared my setup here.*

---
## Start small with your first useful Skill

Maybe all of this still seems too complex for you, with so many foreign terms and especially having to use the CLI to create your wallet.

This is not the only way to use AI effectively, and it all starts with building out your first useful Claude Skill.

There will be workflows that you can outsource to AI. The ones that you hate doing every single day because they're so repetitive and boring.

So start from there and build out your first Claude Skill before moving on to the more complex stuff with AI.

*Get my free video course that walks you through the entire steps [here](https://gideonfip.gumroad.com/l/build-a-useful-claude-skill).*



