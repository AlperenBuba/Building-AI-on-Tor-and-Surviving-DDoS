# Deep Web Diaries: What I Experienced on a Deep Web Site

This repository is not just a codebase; it is an **incident report**, a **case study**, and a **war story** of deploying a local Large Language Model (LLM) on the Tor network.

It documents the technical challenges, the psychological warfare in an anonymous deep web chat environment, the battle against toxic moderation, the ethical boundaries of AI, and the ultimate survival of a privacy-first infrastructure against a targeted Distributed Denial of Service (DDoS) attack.

> 💻 **Looking for the source code?** Check out the fully functional repository here: **[PrivaGPT-Core](INSERT_PRIVAGPT_CORE_LINK_HERE)**

---

## 1. The Genesis: Building in the Shadows
I have always been obsessed with privacy and operational security (OpSec). I never use clearnet platforms that demand identity verification. So, I decided to build my own AI infrastructure: a Tor Hidden Service running a local LLM, entirely on my 16GB Apple Silicon Mac. No cloud, no logs, zero data retention.

I deployed it on a deep web chat platform (often referred to as a "Black Hat" community, though strictly for legal tech discussions) under the alias **"noname"**. The goal was simple: give users a completely uncensored, private AI experience.

## 2. The Hype and The Hardware Bottleneck
I started with a Qwen 14B uncensored model, but quickly upgraded to a 20B parameter model (GPT-OSS 20B OptiQ-4bit). The users in the chat loved it. Within days, people were actively using my service, and I became a well-known figure in that anonymous circle.

However, my biggest enemy wasn't the dark web; it was my hardware:
*   **The 16GB RAM Limit:** Apple's soldered RAM meant no upgrades. Running a 20B model caused severe out-of-memory (OOM) crashes after just 3 prompts.
*   **The Thermal Throttling:** The MacBook was overheating and throttling. I had to buy an external cooling pad just to keep the server stable.
*   **The Engineering Fix:** I implemented a dynamic routing system. When a vision request came, I stopped MLX and loaded Ollama. After processing, I sent a `"keep_alive": 0` signal to instantly unload the model, and a custom Watchdog script managed the RAM.

## 3. The Ethical Line: CSAM and Aggressive Defense
In a completely uncensored AI environment, the darkest parts of human nature emerge. Some individuals tried to use the AI to generate illegal content (CSAM).

I was monitoring the network traffic using `tcpdump`. When I saw these requests, I immediately intervened:
*   I wrote strict regex-based content filters.
*   I modified the system prompt to give the AI a highly aggressive, zero-tolerance persona. I even integrated a custom "swear jar" for the model. When someone asked for illegal content, the AI would instantly hurl heavy insults at them and kick them out of the context.
*   **The Consequence:** The trolls couldn't bypass my filters. Their fragile egos were shattered when the AI insulted them instead of obeying.

## 4. Toxic Moderation and The Community's Revolt
When I shared the project's link in the chat, a highly aggressive, unstable, and seemingly depressed chat admin unjustly banned me. He was taking out his own stress on the members ("dealing with dozens of idiots all day long"). 

I tried explaining the situation with different accounts, but he kept banning me. However, I didn't give up. I directly **reported the issue to the platform's actual staff (site owners).** 
That's when everything changed: The anonymous users in the chat supported me and helped me reach the staff. The staff intervened, warned the admin, and sided with me. After that, I continued sharing the link, and no one banned me again. This event taught me the power of communication and community support, even in an anonymous chat space.

## 5. The Siege: Account Hijacking and DDoS
**Exactly one day after** I reported the toxic admin to the higher authorities, another lowlife (script kiddie) emerged to try and hack my site. (This timing strongly suggests the attacker was an associate of the banned admin, or instigated by him). 

First, he hijacked a friend's chat account (nick and password). My friend's nickname was **"Se7en"**. The attacker took over this nick. However, my friend didn't give up; he reached out to me from a new account and proved his identity by telling me **who we had reported the admin to the previous day**. This was a detail only the two of us knew. I realized it was truly him and confirmed his account was compromised.

This attacker started probing my site for vulnerabilities (checking directories like `server-status`). But my architecture was zero-trust. All chat history was stored client-side (`localStorage` and encrypted with AES-256-GCM). There was nothing to steal on the server.
Frustrated, he launched a **DDoS attack**.
*   **The Collapse:** I was using PHP's built-in development server (`php -S`), which is single-threaded. The flood of requests choked the server, exhausted system resources, and froze the app. Since he hit the Hidden Service directly, I couldn't even access the chat.
*   **The Comeback (Infrastructure & Code Defense):** I fought back. I implemented file-locking (`flock`) rate limits in `/tmp/`, enabled Tor's IntroDoS Defense (Proof of Work), and built a RAM-disk SQLite "Fair Queue" system. Furthermore, I abandoned the single-threaded PHP server and migrated the infrastructure to **Nginx + PHP-FPM**. Eventually, to cut off the attacker's gratification, I shut the site down and burned the `.onion` address.

## 6. Epilogue: Curtain Call and Open-Sourcing the Code
At the end of this entire battle, I could no longer endure the physical limits of my hardware. The 20B model was consuming all the resources of my computer, severely hindering my ability to do my own daily work (writing code, compiling, etc.). 

Therefore, I completely shut down the hidden service. However, instead of throwing this story and architecture away, I **open-sourced the entire codebase on GitHub.** My goal is to enable people to run this privacy-first AI infrastructure on their own devices (whether it's a Mac, Linux, or Raspberry Pi). This journey, which began on the deep web, now lives on in the open-source world.

**The Fate of Se7en:** In the days following the incident, I logged back into the chat a few times. My goal was to check for any traces left by the attacker and to find Se7en. However, Se7en never became active again. I never saw him again. In that dark environment, what started with an account hijacking ended with my friend completely vanishing from the digital world. Perhaps this was one of the harsh realities of the deep web: one day you exist, and the next day you are gone.

## 🛡️ The OpSec Philosophy (Why I do this)
Throughout this entire journey, my real identity was never compromised. I never uploaded an ID, I never gave my surname, and I operated entirely pseudonymously as **"noname"**.

This project proves that you don't need a LinkedIn profile with your government ID to be recognized as a capable engineer. Your code, your architecture, and your ability to survive in a hostile environment are your true passport.

## 🔗 Related Repositories
*   **[PrivaGPT-Core](INSERT_PRIVAGPT_CORE_LINK_HERE):** The actual source code of the Tor-based AI infrastructure.
*   **[HAYALET](https://github.com/AlperenBuba/HAYALET):** My custom-built, Tor-integrated OSINT cyberdeck.

---
*Written by "noname" - A privacy advocate and systems engineer.*