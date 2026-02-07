# LangChain Chains

A collection of examples demonstrating different types of chains in LangChain using the LCEL (LangChain Expression Language) syntax.

## Prerequisites

- Python 3.9+
- OpenAI API Key
- Anthropic API Key (for parallel chain example)

## Installation

### Step 1: Clone the Repository

```bash
git clone <repository-url>
cd LangChainChains
```

### Step 2: Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Linux/Mac
# or
venv\Scripts\activate     # On Windows
```

### Step 3: Install Dependencies

```bash
pip install langchain langchain-openai langchain-anthropic python-dotenv pydantic
```

### Step 4: Configure Environment Variables

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=your_openai_api_key_here
ANTHROPIC_API_KEY=your_anthropic_api_key_here
```

## Project Structure

| File | Description |
|------|-------------|
| `simple_chain.py` | Basic chain example |
| `sequential_chain.py` | Multi-step sequential chain |
| `parallel_chain.py` | Parallel execution chain |
| `conditional_chain.py` | Branching chain with conditions |

---

## Chain Examples

### 1. Simple Chain (`simple_chain.py`)

**Concept:** The most basic chain pattern - Prompt → Model → Parser

**Flow:**
```
Input (topic) → PromptTemplate → ChatOpenAI → StrOutputParser → Output
```

**What it does:** Generates 5 interesting facts about a given topic.

**Run:**
```bash
python simple_chain.py
```

---

### 2. Sequential Chain (`sequential_chain.py`)

**Concept:** Chain multiple prompts sequentially where the output of one becomes the input of the next.

**Flow:**
```
Input (topic) → Prompt1 → Model → Parser → Prompt2 → Model → Parser → Output
```

**What it does:**
1. First generates a detailed report on a topic
2. Then summarizes that report into 5 key points

**Run:**
```bash
python sequential_chain.py
```

---

### 3. Parallel Chain (`parallel_chain.py`)

**Concept:** Execute multiple chains simultaneously using `RunnableParallel`, then merge the results.

**Flow:**
```
                    ┌─→ Prompt1 → Model1 → Parser (notes) ─┐
Input (text) ──────┤                                       ├──→ Merge Prompt → Model → Output
                    └─→ Prompt2 → Model2 → Parser (quiz) ──┘
```

**What it does:**
1. Takes input text about SVMs
2. Generates notes (using OpenAI) and quiz questions (using Anthropic) in parallel
3. Merges both outputs into a single document

**Run:**
```bash
python parallel_chain.py
```

---

### 4. Conditional Chain (`conditional_chain.py`)

**Concept:** Route to different chains based on conditions using `RunnableBranch`.

**Flow:**
```
                                      ┌─→ Positive Response Prompt → Model → Output
Input (feedback) → Classifier Chain ──┤
                                      └─→ Negative Response Prompt → Model → Output
```

**What it does:**
1. Classifies feedback sentiment (positive/negative) using Pydantic parser
2. Routes to appropriate response generator based on sentiment
3. Generates an appropriate response for the feedback

**Run:**
```bash
python conditional_chain.py
```

---

## Key LangChain Components Used

| Component | Description |
|-----------|-------------|
| `PromptTemplate` | Creates reusable prompt templates with variables |
| `ChatOpenAI` | OpenAI chat model wrapper |
| `ChatAnthropic` | Anthropic Claude model wrapper |
| `StrOutputParser` | Parses model output to string |
| `PydanticOutputParser` | Parses model output to Pydantic model |
| `RunnableParallel` | Executes multiple chains in parallel |
| `RunnableBranch` | Conditional routing between chains |
| `RunnableLambda` | Wraps a function as a runnable |

## LCEL Pipe Syntax

LangChain Expression Language uses the `|` (pipe) operator to chain components:

```python
chain = prompt | model | parser
```

This creates a chain where:
1. Input goes to `prompt`
2. Prompt output goes to `model`
3. Model output goes to `parser`
4. Parser output is the final result

## Visualizing Chains

Each example includes `chain.get_graph().print_ascii()` to visualize the chain structure in the terminal.

## License

MIT
