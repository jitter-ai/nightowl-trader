# Nightowl Trader

## Executive summary

Nightowl Trader is open-source AI trading agents that run in your own AWS account: they watch the market, size a signal, check it against hard-coded risk limits and trade through [Alpaca](https://alpaca.markets/). Paper trading is on by default, so no real money moves until you switch it on, and every decision is logged for replay. <img src="assets/icons/terraform.png" width="18" alt=""> [Terraform](https://www.terraform.io/) deploys it to <img src="assets/icons/eks.png" width="18" alt=""> [Amazon EKS](https://aws.amazon.com/eks/) inside a <img src="assets/icons/vpc.png" width="18" alt=""> [VPC](https://aws.amazon.com/vpc/), behind an <img src="assets/icons/alb.png" width="18" alt=""> [Application Load Balancer](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/) where <img src="assets/icons/cognito.png" width="18" alt=""> [Cognito](https://aws.amazon.com/cognito/) authenticates every request; <img src="assets/icons/github-actions.png" width="18" alt=""> [GitHub Actions](https://github.com/features/actions) builds the images into <img src="assets/icons/ecr.png" width="18" alt=""> [Amazon ECR](https://aws.amazon.com/ecr/), and credentials come from <img src="assets/icons/secrets-manager.png" width="18" alt=""> [Secrets Manager](https://aws.amazon.com/secretsmanager/) at run time. The <img src="assets/icons/react.png" width="18" alt=""> [React](https://react.dev/) desk fits one screen, in three views:

- **Performance:** profit and loss, and the open positions behind it.
- **Trading Floor:** a <img src="assets/icons/tradingview.png" width="18" alt=""> [TradingView Lightweight Charts](https://github.com/tradingview/lightweight-charts) chart and an order ticket. Behind it, a <img src="assets/icons/langgraph.png" width="18" alt=""> [LangGraph](https://www.langchain.com/langgraph) team of <img src="assets/icons/langchain.png" width="18" alt=""> [LangChain](https://www.langchain.com/) agents over <img src="assets/icons/mcp.png" width="18" alt=""> [MCP](https://modelcontextprotocol.io/) sizes each signal and passes it through the risk check before it reaches the broker; <img src="assets/icons/redis.png" width="18" alt=""> [Redis](https://redis.io/) holds agent state and <img src="assets/icons/postgresql.png" width="18" alt=""> [PostgreSQL](https://www.postgresql.org/) stores each run.
- **Strategies:** the registry. A channel post becomes transcripts, <img src="assets/icons/bedrock.png" width="18" alt=""> [Amazon Bedrock](https://aws.amazon.com/bedrock/) turns them into rules, and each strategy is backtested against Nasdaq buy-and-hold with [backtesting.py](https://github.com/kernc/backtesting.py) before it reaches the desk. A passing set asks for approval by email, with a signed Yes/No link per rule. <img src="assets/icons/litellm.png" width="18" alt=""> [LiteLLM](https://litellm.ai/) routes the model calls to DeepSeek, traced in <img src="assets/icons/langfuse.png" width="18" alt=""> [Langfuse](https://langfuse.com/).

The document ingestion pipeline is drawn below; it is not wired to the desk yet.

## In action

**Walk the desk.** The clip opens on Performance, switches the range to All and the bottom panel to open positions, pauses the auto loop, types MSFT on the Trading Floor with confidence 0.8, then refreshes the Strategies registry. (Recorded on demo data.)

![Walking Performance, the Trading Floor, and Strategies](assets/trade-desk-2026-10-02_17-20-28.gif)

## Architecture

Nightowl Trader: one AWS deployment serves the React trading pages and the LangGraph agents, fed by one Qdrant vector store.

![Nightowl Trader: a Cognito-gated trading desk on EKS with the agents fed by one ingestion pipeline](assets/nightowl-trader-v13.png)

## License

PolyForm Noncommercial 1.0.0 with a required notice from Haya Tech LLC. Noncommercial use is free; any commercial use, including resale, hosting for others, or a paid product, needs a written license from Haya Tech LLC first. See [LICENSE](LICENSE).
