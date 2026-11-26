# Shlok Patel


CS & Business (FinTech) at Northeastern · [shlokpatel.tech](https://shlokpatel.tech) · [LinkedIn](https://linkedin.com/in/-shlokpatel) · [Resume](https://shlokpatel.tech/resume.pdf)

---

## [LevelEdge](https://leveledge.vercel.app)
*Python, FastAPI, React, AWS (Lambda, SQS, DynamoDB, EventBridge)*

Most trading projects show you the one backtest that worked, and I never trusted that. I wanted a platform that would tell me honestly how much of an edge was real and how much was me fooling myself, since I planned to put my own money behind it.

So I built a research engine that runs every strategy through leakage-free, out-of-sample testing after costs, and a daily forecast pipeline on AWS to run it at scale. The engine killed 13 of the 21 strategies I tried. The one honest result that survived was a trend filter that doesn't earn more but loses far less in a crash, which is now the only thing I actually trade. To make the track record impossible to fudge, I hash-chain and timestamp every forecast before its outcome exists, and the daily scan runs as an event-driven pipeline (EventBridge into SQS fan-out into Lambda into DynamoDB) that forecasts 100 tickers in about five minutes with zero failed batches.

## [PhishAware](https://phish-aware.tech)
*Next.js, TypeScript, MongoDB, FastAPI, AWS (EC2, CDK), Groq/Gemini API Integration*

Security training usually measures whether you finished a module, not whether you can actually spot a phish, and that gap is where real breaches happen. On a three-person team I took the infrastructure and ML so we could build something that tests judgment instead of attendance.

I built a two-stage LLM pipeline (Groq for generation, Gemini as failover) that writes role-specific phishing across six channels, and an onboarding model that predicts a new user's awareness score within five points about 85% of the time, so we place people at the right difficulty instead of boring everyone with the basics. The harder, less glamorous win was shrinking that model from 515 MB to 3.3 MB so it fit on a small production server, which came down to choosing gradient boosting over random forests and matching library versions in Docker. I also wrote the OIDC single sign-on from scratch to actually understand PKCE and token validation, and the whole stack deploys to AWS in under three minutes from one CDK definition.

## [NUHire](https://github.com/Oasis-NEU/NUHire)
*Next.js, Express, TypeScript, MySQL, Prisma, Gemini API*

NUHire is a hiring simulation that Northeastern's CS1210 will use in class, built by a ten-person team. Student resumes carry real contact info, and the AI screener sends resume text to an external model, so my job on the data layer was making sure nothing private leaked.

I built the contact-info stripper as the only path the code allows to reach the model, enforced through the type system, so you can't accidentally send raw text even if you try. Across eight different real PDF layouts, nothing leaked out of 87 planted details. Along the way I found hostile input taking two to three minutes to process, traced it to catastrophic regex backtracking, and rewriting that one pattern brought it under thirty milliseconds.

## [Derivative Hedging via Reinforcement Learning](https://github.com/shlok-p07/Reinforcement-Learning---Derivative-Hedging)
*Python, PyTorch, Stable-Baselines3*

I wanted to know whether reinforcement learning could out-hedge the classical rules traders have used for decades, and to answer it honestly rather than cherry-pick a good run.

I built a reproducible harness across six strategies and four market regimes with a 10,000-sample bootstrap. The answer was no: the RL edge over simple delta hedging wasn't statistically real, and an old analytical rule (Whalley-Wilmott) matched it with far fewer trades. The more interesting result came when I changed the reward to a CVaR objective to punish tail risk and the agent gamed it with a directional tilt, which I only caught by ablating the drift. That taught me to distrust a reward function until I've tried to break it.

---

## [PortIQ](https://github.com/shlok-p07/26S-Project-SigmaDevs-Repo)
*Python, Flask, Streamlit, MySQL, Plotly, Yahoo Finance & FRED APIs*

An investing analytics app built on a 30-table MySQL schema, serving four different user roles (quant trader, analyst, CIO, beginner) from one shared database. It started as a database-design course project and grew into a full market-data and backtesting platform.
 
The core is the market engine. It pulls daily prices for about 11,000 US stocks and ETFs from Yahoo and T-bill rates from FRED through a six-worker loader that respects a rate limit, saves progress per ticker so an interrupted load resumes, and only counts a run as successful once 95% of securities are current. On top of that sits a backtest engine for six strategy types that decides weights at each close and applies them the next day so there's no look-ahead, charges 5 bps per trade, and earns the T-bill rate on idle cash. A weekday scheduler refreshes and re-backtests everything, with database and thread locks so the scheduler and the manual refresh button can't collide.
 

---

## Research — Multimodal ML for Parkinson's Detection
*NU Intelligent Automation and IoT Research Group, advised by Prof. Sarita Singh*

Screening for Parkinson's from phone sensors only works if the evaluation is honest, and small leaks can quietly inflate results, so most of my work went into getting the harness right.

I split data by participant so the model can't memorize people, locked the test set, and tested significance with DeLong. Stacking voice, tapping, and gait raised held-out AUROC from 0.720 to 0.774, but the gain wasn't statistically significant (p = 0.080), and I report it that way rather than round up to a headline. The catch I'm most glad about was a test that flagged a signal-processing bug doubling a measured frequency, which would have inflated results if it had slipped through. Poster submitted to ACM SIGCSE 2027.

---

Reach me at **patel.s15@northeastern.edu**.
