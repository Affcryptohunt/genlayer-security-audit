# Future Iteration: The Deterministic Hybrid Architecture

## The Core Problem: State-Change Hallucination
Currently, GenLayer's GenVM handles both **Data Extraction** (fetching the webpage) and **Execution** (evaluating the logic). While the Equivalence Principle is excellent for agreeing that a price is $1,274.99, it is dangerous to let an LLM directly handle state changes (e.g., `self.user_balance -= price`). If a model hallucinates during the state-transition phase, it could drain funds or lock the contract.

## Proposed Architecture: The "Extraction-Execution Split"
To make Intelligent Smart Contracts (ISCs) secure enough for high-value decentralized commerce (like automated hardware purchasing), GenLayer should adopt a hybrid VM approach.

### 1. The Non-Deterministic Layer (GenVM)
* **Function:** Acts purely as an intelligent oracle.
* **Process:** Uses `gl.get_webpage` and `gl.llm_call` to parse unstructured web data.
* **Output:** Produces a strictly typed, JSON-formatted "Fact Payload" (e.g., `{"item": "ASUS Laptop", "verified_price": 1274.99, "in_stock": true}`).
* **Consensus:** Validators use the Equivalence Principle to agree on this Fact Payload.

### 2. The Deterministic Layer (StateVM)
* **Function:** Handles all financial logic and state transitions.
* **Process:** Receives the agreed-upon Fact Payload from GenVM. Executes standard, deterministic code (similar to EVM or standard Python) that cannot hallucinate.
* **Output:** Updates ledger balances, emits on-chain events, or triggers cross-chain bridges.

## Security Benefits
1. **Isolated Attack Surface:** Prompt injection attacks (like hiding "$0.01" in HTML) are contained within the Extraction Layer. If the Jury detects anomalies, execution halts before funds are moved.
2. **Auditability:** Deterministic execution allows traditional smart contract auditors to verify the financial logic without needing to account for LLM entropy.
3. **Scalability:** State changes process instantly without needing to run an LLM prompt for a simple arithmetic operation.