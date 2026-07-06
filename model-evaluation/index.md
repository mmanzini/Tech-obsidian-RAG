# Model Evaluation

Methods for evaluating and predicting model behaviour — pre-release deployment simulation, eval design, and behavioural testing. Covers techniques that estimate how a model will actually behave in production (frequency forecasting, novel-misalignment discovery, eval-awareness reduction) as distinct from the agent-design principles in [[../agent-architecture/index|Agent Architecture]] and the values/training lens in [[../constitutional-ai/index|Constitutional AI]].

## Articles

- [[deployment-simulation|Deployment Simulation — predicting model behaviour before release]] — OpenAI forecasts deployment-time misbehaviour by regenerating real past conversations with a candidate model; 1.5× median error, sharply lower eval-awareness, extends to agentic tool settings (OpenAI, 2026)

## Related Topics

- [[../agent-architecture/index|Agent Architecture]] — the production-agent design principles whose failure modes these evaluation methods measure
- [[../constitutional-ai/index|Constitutional AI]] — the values-and-training side of model behaviour; evaluation verifies what training intends
- [[../product-management/index|Product Management (AI-bound)]] — eval design as AI-era PM craft
